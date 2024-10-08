オプティマイザーは、*最も可能性の高い結果を見つける方法（最尤推定）*と*誤差を減らすために過去にさかのぼって調整する方法（時間逆伝播）*を適用して、メモリの安定性を推定し、時系列レビューのログからメモリの法則を学習します。次に、確率的最短経路アルゴリズムを使用して、繰り返しを最小限に抑えるための最適な保持を見つけることができます。

FSRSは**DSR**（難易度、安定性、再現性）メモリモデルに基づいています。**DSR**モデルは、間隔反復練習中のメモリダイナミクスを記述するために、17のパラメーターと6つの方程式を使用します（詳細は[アルゴリズム](The-Algorithm.md)を参照してください）。

ここでは、FSRSのトレーニングプロセスの簡単な紹介をします。

# Ankiのレビューログの前処理

Ankiのレビューログのデータベーススキーマは以下の通りです（[データベース構造](https://github.com/ankidroid/Anki-Android/wiki/Database-Structure)からコピー）：

```SQL
-- revlogはレビュー履歴です。これまでに行ったすべてのレビューに対して行が存在します！
CREATE TABLE revlog (
    id              integer primary key,
       -- レビューを行った時刻のエポックミリ秒タイムスタンプ
    cid             integer not null,
       -- cards.id
    usn             integer not null,
        -- 更新シーケンス番号：同期時に差分を見つけるためのもの。
        -- 詳細はcardsテーブルの説明を参照してください
    ease            integer not null,
       -- リコールを評価するために押したボタン。
       -- レビュー:  1（間違い）、2（難しい）、3（普通）、4（簡単）
       -- 学習/再学習:  1（間違い）、2（普通）、3（簡単）
    ivl             integer not null,
       -- 間隔（カードテーブルのものと同様）
    lastIvl         integer not null,
       -- 最後の間隔（ivlの最後の値。この値は必ずしもこのレビューと前のレビューの間の実際の間隔と等しいわけではありません）
    factor          integer not null,
      -- ファクター
    time            integer not null,
       -- レビューにかかったミリ秒数、最大60000（60秒）
    type            integer not null
       --  0=学習、1=レビュー、2=再学習、3=詰め込み
);
```
revlogは、カードcidがidの時刻に評価easeでレビューされたことを記録します。カードの全レビュー履歴を追跡するのは簡単です。カードのメモリ状態はそのレビュー履歴に依存します。revlogをカードごとにグループ化し、時間順に並べ替え、各隣接するレビュー間の評価と間隔を連結します。これにより、各カードの評価履歴と間隔履歴を生成できます。以下は前処理後のサンプルです：

![image](https://user-images.githubusercontent.com/32575846/202617053-d9c44a82-ef85-412e-9987-a6205ba8f986.png)

# 時系列データを用いたトレーニング

機械学習の用語では、評価と間隔の履歴は時系列データの特徴量です。この時系列データを使用して**DSR**モデルをトレーニングしたいと考えています。入力はr_historyとt_historyで、出力はStabilityとDifficultyです。このデータタイプを処理するために、PyTorchを使用して時系列モデルを構築しました。このモデルは、時間を通じた逆伝播（**BPTT**）を自動的に適用してトレーニングすることができます。

![image](https://user-images.githubusercontent.com/32575846/202617068-0c4cdb04-b754-4893-9cbd-e2e0583dc159.png)

# エンドツーエンドのトレーニング

時系列データからモデルをトレーニングする際には、別の問題があります。FSRSの主な目標の一つは、一連のレビュー後のメモリ状態を予測することです。しかし、時系列データは安定性や難易度ではなく、評価のみを記録します。安定性は、同じレビュー履歴を持つレビュー群の統計変数です。生データでメモリの安定性を推定することは可能でしょうか？この場合、最尤推定（**MLE**）が適しています。

忘却曲線の関数を $f(t,S)$ としましょう。$t$ 日後のリコールの可能性（ $r=1$ ）は $P(r=1,t|S) = f(t,S)$ であり、忘却の確率（ $r=0$ ）は $P(r=0,t|S) = 1 - f(t,S)$ です。同じ安定性 $S$ で $n$ 回のレビューを行い、$p$ 回リコールし、$n - p$ 回失敗したとします。安定性 $S$ をどのように推定するのでしょうか？

**MLE** 方法によれば、$P(r=1,t|S)^p \cdot P(r=0,t|S)^{n-p}$ は $S$ に対する尤度関数です。尤度関数の対数を取ることで、対数尤度の式を得ることができます。

$$p \cdot \log P(r=1,t|S) + (n-p) \cdot \log P(r=0,t|S)$$

$n = 1$ と設定し、次を得ます：

$$loss = - [p \cdot \log P(r=1,t|S) + (1-p) \cdot \log P(r=0,t|S)],$$

これは $S$ を更新するための損失関数です。

この時点で、$S$ を **DSR** モデルに置き換え、複数のレビューの場合を考慮します。

$$P(r=1,t,\vec{r},\vec{t}|\vec\theta) = f(t,DSR(\vec{r},\vec{t})),$$

ここで、$\vec\theta$ はモデルのパラメータです。

**MLE** と **BPTT** を使用して、時系列データから直接 **DSR** のパラメータをトレーニングすることができます。