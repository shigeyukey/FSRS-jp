# 更新

このWikiページでは、カスタムスケジューリングコードの実行プロセスについて説明していますが、FSRSがAnki 23.10+およびAnkiDroid 2.17+に統合されて以来、内容が古くなっています。

# 要件

FSRS4Ankiスケジューラの動作プロセスをより深く理解するために、[AnkiWebView Inspector](https://ankiweb.net/shared/info/31746032)をインストールすることをお勧めします。以下のテキストでは、`Inspector`ウィンドウのスクリーンショットを使用します。

レビューを開始する前にインスペクタを開いてください。

# スケジューラの実行を確認する方法

`今すぐ勉強`をクリックする前にインスペクタを起動し、デッキに入ると次のような画面が表示されます：

![image](https://user-images.githubusercontent.com/32575846/192134957-1c058d80-984e-4420-8206-5de5b404835a.png)

左側のウィンドウには、カスタムスケジューリングの実行中のコードが表示されます。

`F10`キーまたは次の図のボタンを使用して、コードを1行ずつ実行します。

![image](https://user-images.githubusercontent.com/32575846/192135103-0cbe2eae-4e7e-449b-a1b6-70f4fb756900.png)

重要！: コードが最後まで実行される前に`答えを表示`をクリックしないでください。`F8`キーを使用して最後まで実行できます。

# スケジューラの詳細

スケジューラのコードは3つの部分に分かれています。

## パラメータの設定

5行目から18行目はすべてのカードに対してパラメータを設定しています。24行目から40行目は特定のデッキに対してパラメータを設定しています。

> 関連する議論: [[質問] Ankiのカスタムスケジューリングが「デッキごと」にならない問題の回避方法](https://github.com/open-spaced-repetition/fsrs4anki/issues/7)

![image](https://user-images.githubusercontent.com/32575846/192135583-713edf25-4ca5-41ea-b5fb-3f61b9b8775d.png)

## スケジューリングの状態を確認

142行目から192行目はスケジューリングの状態を確認するために使用されます。Ankiでは、カードのスケジューリングには4つの状態があります：`New`、`Learning`、`Review`、および`Relearning`です。`Learning`と`Relearning`はスケジューリングにおいて同じです。また、デッキには通常のデッキとフィルターデッキの2種類があります。通常のデッキでは、すべてのレビューがカードの間隔を変更します。しかし、フィルターデッキでは、`このデッキでの回答に基づいてカードを再スケジュールする`ボックスにチェックを入れた場合にのみ、Ankiはカードの間隔を更新します。FSRS4Ankiスケジューラはフィルターデッキもサポートしています。

![image](https://user-images.githubusercontent.com/32575846/192136063-a8f777b6-b79d-4a2a-9c41-d018e80f49b0.png)

## 記憶状態の計算

FSRS4Ankiスケジューラは、あなたの評価とDSRモデルから記憶状態を計算します。スケジュールされた間隔は、記憶状態とカスタムパラメータに基づいています。

![image](https://user-images.githubusercontent.com/32575846/192137490-dba0944d-a163-469d-af2c-5e3f82efa0ce.png)

レビュー中に記憶状態が利用できない場合、FSRS4AnkiスケジューラはAnkiの内蔵スケジューリング情報を記憶状態に変換します。

![image](https://user-images.githubusercontent.com/32575846/192137829-d987d748-5ff2-44a0-9aa3-d58d4cb52ebb.png)

Ankiの内蔵スケジューリングの`interval`と`factor`（SM-2の変種）は、自動的にFSRSの記憶状態に変換されます。

### 間隔から安定性へ

カードの記憶状態が空の場合、FSRSはAnkiによって与えられた$\text{Interval}$が$\text{Stability} \times \text{Interval modifier}$に等しいと仮定します。ここで、
$\text{Interval modifier} = 9 \times \left(\frac{1}{\text{RequestRetention}} - 1 \right)$

したがって、$\text{Stability} = \text{Interval} / \text{Interval modifier}$ 

### イーズファクターから難易度へ

SM-2では、リコール後、間隔（安定性）はイーズファクターによって増加します。

FSRSでは、リコール後、安定性は次の係数によって増加します：$(1 + e^{w_8} \cdot (11-D) \cdot S^{-w_9} \cdot (e^{w_{10}\cdot(1-R)}-1) \cdot w_{15}(\textrm{if G = 2}) \cdot w_{16}(\textrm{if G = 4}))$。

これら二つを等しくすると、難易度は次のように計算できます：

$$D = 11 - \cfrac{factor - 1}{e^{w_8}\cdot S^{-w_9}\cdot(e^{w_{10}\cdot(1-R)}-1)}。$$

上記のように安定性と難易度が計算された後、それらは次の安定性と次の難易度（レビュー後）の計算に使用されます。