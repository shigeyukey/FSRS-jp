中文版请见：[FSRS4Anki 使用指北](https://zhuanlan.zhihu.com/p/636564830)

(Anki v23.10より古いバージョンでカスタムスケジューリングバージョンのFSRS)

# 目次
- [ステップ1: FSRSスケジューラを有効にする](#%E3%82%B9%E3%83%86%E3%83%83%E3%83%971-fsrs%E3%82%B9%E3%82%B1%E3%82%B8%E3%83%A5%E3%83%BC%E3%83%A9%E3%82%92%E6%9C%89%E5%8A%B9%E3%81%AB%E3%81%99%E3%82%8B)
- [ステップ2: FSRSをパーソナライズする](#%E3%82%B9%E3%83%86%E3%83%83%E3%83%972-fsrs%E3%82%92%E3%83%91%E3%83%BC%E3%82%BD%E3%83%8A%E3%83%A9%E3%82%A4%E3%82%BA%E3%81%99%E3%82%8B)
- [異なるデッキに異なるパラメータを設定する](#%E7%95%B0%E3%81%AA%E3%82%8B%E3%83%87%E3%83%83%E3%82%AD%E3%81%AB%E7%95%B0%E3%81%AA%E3%82%8B%E3%83%91%E3%83%A9%E3%83%A1%E3%83%BC%E3%82%BF%E3%82%92%E8%A8%AD%E5%AE%9A%E3%81%99%E3%82%8B)
- [FAQ](#faq)

FSRSを始めるには、2つのステップを踏む必要があります。

- まず、AnkiアプリケーションでFSRSスケジューラを有効にする必要があります。
- 次に、FSRSをあなたの学習パターンに合わせてパーソナライズする必要があります。

それでは、これらのステップについて詳しく説明します。

## ステップ1: FSRSスケジューラを有効にする

### 1.1 AnkiのV3スケジューラを有効にする

ツール > 設定 > レビュー > V3スケジューラを有効にする。

<p align="center"><img width="625" alt="image" src="https://github.com/open-spaced-repetition/fsrs4anki/assets/32575846/8f91fba8-9b8b-405c-8aa9-42123ba5faeb"></p>

### 1.2 FSRSスケジューラのコードを貼り付ける

- 以下のページにアクセスし、すべてのコードをコピーします。 https://github.com/open-spaced-repetition/fsrs4anki/blob/main/fsrs4anki_scheduler.js
- Ankiで、任意のデッキのデッキオプションを開きます（どのデッキでも構いません）。「高度な設定」列を見つけ、コピーしたコードを「カスタムスケジューリング」フィールドに貼り付けます：
<p align="center"><img width="625" alt="image" src="https://github.com/open-spaced-repetition/fsrs4anki/assets/32575846/5c292f91-8845-4f8c-ac42-55f9a0f2946e"></p>

- FSRSを使用するデッキでは、学習および再学習のステップが1日未満であることを確認してください。「卒業間隔」や「簡単間隔」などの他の設定は重要ではありません。どのAnki設定が重要で、どの設定が不要かについての詳細は、[よくある質問 (FAQ)](FAQ.md)を参照してください。
<p align="center"><img width="625" alt="image" src="https://github.com/user1823/fsrs4anki/assets/32575846/ba36847d-28f5-4df3-b4b3-4ff425609c04"></p>

上記の手順を実行すると、FSRS4Ankiスケジューラが正常にアクティブになるはずです。これを確認したい場合は、コードのこの部分を変更できます：

```javascript
const display_memory_state = false;
```

を次のように変更します：

```javascript
const display_memory_state = true;
```

次に、任意のデッキを開いてレビューを開始すると、以下のメッセージが表示されます：

<p align="center"><img width="625" alt="image" src="https://github.com/open-spaced-repetition/fsrs4anki/assets/32575846/0a5d4561-6052-45f3-91a5-5f21dd6497b9"></p>

これはFSRSスケジューラが正常に動作していることを示しています。D、S、Rが表示されず、「FSRS enabled」のみが表示される場合、そのカードは「学習」または「再学習」段階にあり、「レビュー」段階にはないことを意味します。

その後、コードを元に戻すと、メッセージは表示されなくなります。

## ステップ2: FSRSをパーソナライズする

FSRSをあなたの学習ニーズに合わせてパーソナライズするには、2つのステップが必要です。

- まず、FSRSオプティマイザーを使用してコレクションのFSRSパラメータをトレーニングし、アルゴリズムをあなたの学習パターンに合わせます。
- 次に、望ましい保持率と最大間隔を選択します。

それでは、これらのステップについて詳しく説明します。

### ステップ2.1 FSRSパラメータのトレーニング

ほとんどのユーザーにとって、パラメータをトレーニングするためには、以下の2つの方法（Google ColabとHugging Face）のいずれかを使用することをお勧めします。上級ユーザーは、[こちら](Advanced-methods-of-optimization.md)に記載されている他のオプションを探索することもできます。

FSRSオプティマイザーは、正確な結果を得るために最低2,000回のレビューが必要であることに注意してください。データが十分でない場合は、このステップをスキップして、スケジューラコードに既に入力されているデフォルトのパラメータを使用することができます。

<details>
  <summary>方法1: Google Colabを使用したトレーニング</summary>

[オプティマイザーのノートブック](https://colab.research.google.com/github/open-spaced-repetition/fsrs4anki/blob/main/fsrs4anki_optimizer.ipynb)を開きます。コーディング環境を自分で設定する必要はなく、Googleのマシンを無料で使用できます（Googleアカウントが必要です）：

<p align="center"><img width="625" alt="image" src="https://github.com/open-spaced-repetition/fsrs4anki/assets/32575846/5f5af21b-583d-496c-9bad-0eef0b1fb7a6"></p>

Colabのウェブサイトが開いたら、フォルダタブに切り替えます。オプティマイザーがGoogleのマシンに接続されたら、右クリックしてAnkiからエクスポートしたデッキファイル/コレクションファイルをアップロードできます。

<p align="center"><img width="625" alt="image" src="https://github.com/open-spaced-repetition/fsrs4anki/assets/32575846/66f9e323-fca8-4553-bcb2-b2e511fcf559"></p>

これらのファイルをエクスポートする際は、「スケジューリング情報を含む」と「古いAnkiバージョンをサポート」を選択してください。メディアを含める必要はありません。

<details>
  <summary>プライバシーに関する注意</summary>
  
オプティマイザーにアップロードするデッキは、FSRSの作者がアクセスできません。オプティマイザーのコードはオープンソースであるため、コードを理解できる人なら誰でもこれを確認できます。

Googleはアップロードされたデータにアクセスできる可能性がありますが、リスクは個人のGoogleドライブフォルダにデータをアップロードするのと同様です。

プライバシーが心配な場合は、以下の2つのオプションがあります。
- 上級ユーザーは、[こちら](Advanced-methods-of-optimization.md)に記載されているオプションを使用してスクリプトをローカルで実行できます。
- 他のユーザーは、フィールドを空白にしてコレクションをエクスポートできます。これを行うには、以下の手順に従ってください：
    - 万が一問題が発生した場合に備えて、`ファイル → バックアップを作成`でバックアップを取ります。
    - `ブラウズ > ノート > 検索と置換`に移動します。
    - 「検索」フィールドに `(.|\n)*` と入力し、「置換」フィールドは空白のままにします。
    - 「入力を正規表現として扱う」オプションにチェック（✓）を入れます。「選択されたノートのみ」のチェックを外します。これをすべてのノートに適用する場合は、チェックを外してください。<p align="center"><img width="625" alt="image" src="https://github.com/user1823/fsrs4anki/assets/32575846/eaaf818d-e0b1-486f-875a-4aa6b96e258a"></p>
    - 上記の手順に従ってコレクションをエクスポートします。
    - `編集 → 検索と置換の元に戻す`に移動して、ノートの内容を復元します。

</details>

<p align="center"><img width="625" alt="image" src="https://github.com/open-spaced-repetition/fsrs4anki/assets/32575846/65da272d-7a01-4c46-a1d9-093e548f1a2d"></p>

- ファイルをアップロードした後、`collection-2022-09-18@13-21-58.colpkg`をアップロードしたファイルの名前に置き換えます。
- `Asia/Shanghai`をあなたのタイムゾーンに置き換えます。ノートブックにはタイムゾーンのリストへのリンクが含まれています。
- `next_day_starts_at`の値も置き換えます。この値を見つけるには、Ankiの`ツール > 設定 > レビュー > 次の日の開始時刻`に移動します。

<p align="center"><img width="625" alt="image" src="https://github.com/open-spaced-repetition/fsrs4anki/assets/32575846/f344064c-4ccf-4884-94d0-fc0a1d3c3c24"></p>

次に、`Ctrl+F9`を押すか、`ランタイム > すべて実行`に移動してオプティマイザーを実行します。

<p align="center"><img width="625" alt="image" src="https://github.com/open-spaced-repetition/fsrs4anki/assets/32575846/77947790-6916-4a99-ba28-8da42fd5b350"></p>

コードの実行が完了するのを待ちます。その後、セクション2.2（結果）に移動し、最適化されたパラメータが利用可能になります。これらのパラメータをコピーします。

<p align="center"><img width="625" alt="image" src="https://github.com/open-spaced-repetition/fsrs4anki/assets/32575846/8df1d210-73c3-4194-9b3b-256279c4c2fd"></p>
</details>

<details>
  <summary>方法2: Hugging Faceを使用したトレーニング</summary>
  
エクスポートしたデッキをこのウェブサイトにアップロードするだけで、最適化してくれます。  
https://huggingface.co/spaces/open-spaced-repetition/fsrs4anki_app

<p align="center"><img width="625" alt="image" src="https://github.com/Luc-mcgrady/fsrs4anki/assets/63685643/a03217f0-6627-4854-971f-f2bc9d14da5c"></p>
</details>

上記のいずれかの方法でパラメータをトレーニングした後、以前にコピーしたFSRSコード内のパラメータを置き換えます。

<p align="center"><img width="625" alt="image" src="https://github.com/open-spaced-repetition/fsrs4anki/assets/32575846/70b3b45a-f014-4574-81eb-cad6d19f93d9"></p>

⚠️注意: これらのパラメータを置き換える際に、角括弧や閉じ括弧の後のコンマを誤って消さないようにしてください。これらがないとコードが動作しません。

FSRSを使用し始めた後も、2ヶ月に一度はパラメータを再トレーニングするべきです。ただし、これはコレクションの古さによります。比較的新しいコレクションを持つユーザーは、毎月再最適化することを検討するかもしれません。再最適化により、FSRSが現在の学習パターンにうまく適応することが保証されます。

### ステップ2.2: 望ましい保持率と最大間隔の選択

次に、FSRSが達成しようとする保持率（つまり、カードが成功裏に想起される割合）を示す`requestRetention`を選択する必要があります。

この値を決定するための補助として、Ankiの統計で過去の保持率を確認できます。例えば、過去の保持率が90%であった場合、`requestRetention`を0.90に設定できます。

より高い`requestRetention`を設定することもできますが、`requestRetention`を0.90以上にすると、レビューの負荷（1日あたりのレビュー数）が非常に急速に増加することに注意してください。同じ理由で、`requestRetention`を0.97以上に設定することはお勧めしません。

`requestRetention`の値を決定したら、これをスケジューラコードに入力します。同時に、任意のカードが達成できる最大間隔を示す`maximumInterval`の値も決定します。FSRSスケジューラコード内の値は、Ankiのデッキオプションで設定された値を上書きします。

<p align="center"><img width="625" alt="image" src="https://github.com/user1823/fsrs4anki/assets/32575846/6989b282-7988-4d9e-9fbe-0b79985e9952"></p>

上記の手順を実行した後、FSRSの使用を開始する準備が整いました。レビューを開始すれば、FSRSがその役割を果たします。

### FSRS4Anki Helperアドオンを使用して既存のカードを再スケジュールする
AnkiでFSRSを設定した後、[FSRS4Anki Helperアドオン](https://ankiweb.net/shared/info/759844606)をインストールし、既存のカードを再スケジュールするために使用できます。これは、Ankiの内蔵アルゴリズムに従って以前にスケジュールされたカードを再スケジュールするための一度限りの措置です。このアドオンは他にも多くの便利な機能を提供します。アドオンの詳細については、こちらをご覧ください: https://github.com/open-spaced-repetition/fsrs4anki-helper

<p align="center"><img width="625" alt="image" src="https://github.com/user1823/fsrs4anki/assets/32575846/92289976-8b35-44b3-b5cd-3e6f89759c8d"></p>


## 異なるデッキに異なるパラメータを設定する

異なるデッキに対して異なるパラメータを生成し、コード内でそれぞれを設定することもできます。デフォルトの設定では、`deckParams`にはすでに3つのパラメータグループが含まれています。

「FSRS4Ankiのグローバル設定」グループは、グローバルパラメータです。

「MainDeck1」グループは、デッキ「MainDeck1」とそのサブデッキに適用されるパラメータです。

同様に、3番目のグループはデッキ「MainDeck2::SubDeck::SubSubDeck」とそのサブデッキに適用されるパラメータです。これらを設定したいデッキに置き換えることができます。さらに必要な場合は、自由にコピーして追加してください。

```javascript
const deckParams = [
  {
// FSRS4Ankiのデフォルトパラメータ（グローバル設定）
    "deckName": "FSRS4Ankiのグローバル設定",
    "w": [0.4, 0.6, 2.4, 5.8, 4.93, 0.94, 0.86, 0.01, 1.49, 0.14, 0.94, 2.18, 0.05, 0.34, 1.26, 0.29, 2.61],
    // 上記のパラメータはFSRS4Ankiオプティマイザーで最適化できます。
    // パラメータの詳細については、こちらをご覧ください: https://github.com/open-spaced-repetition/fsrs4anki/wiki/The-Algorithm
    // ユーザーのカスタムパラメータ（グローバル設定）
    "requestRetention": 0.9, // 推奨設定: 0.75 - 0.95
    "maximumInterval": 36500,
    // FSRSは長期スケジューリングのみを変更します。そのため、デッキオプションの（再）学習ステップは通常通り機能します。
    // 1日未満のステップを設定することをお勧めします。
  },
  {
    // 例1: このデッキおよびそのサブデッキに対するユーザーのカスタムパラメータ。
    "deckName": "MainDeck1",
    "w": [0.6, 0.9, 2.9, 6.8, 4.72, 1.02, 1, 0.04, 1.49, 0.17, 1.02, 2.15, 0.07, 0.35, 1.17, 0.32, 2.53],
    "requestRetention": 0.9,
    "maximumInterval": 36500,
  },
  {
    // 例2: このデッキおよびそのサブデッキに対するユーザーのカスタムパラメータ。
    // いかなるキーも省略しないでください。
    "deckName": "MainDeck2::SubDeck::SubSubDeck",
    "w": [0.6, 0.9, 2.9, 6.8, 4.72, 1.02, 1, 0.04, 1.49, 0.17, 1.02, 2.15, 0.07, 0.35, 1.17, 0.32, 2.53],
    "requestRetention": 0.9,
    "maximumInterval": 36500,
  }
];
```

FSRSを使用したくないデッキがある場合は、その名前を `skip_decks` リストに追加できます。

```javascript
const skip_decks = ["MainDeck3", "MainDeck4::SubDeck"];
```

# FAQ

Q0: なぜFSRSでは（再）学習ステップを短く保つべきなのでしょうか？FSRSにとって長い間隔とはどのくらいですか（FAQによると1日かもしれません）？

A0: Ankiのカスタムスケジューリングの制限により、FSRSスケジューラは学習または再学習ステップのレビューが同じ日に行われるか、異なる日に行われるかを判断できません。そのため、スケジューラはすべてのレビューが同じ日に行われると仮定します。これにより、最後の学習ステップが1日以上であっても、FSRSスケジューラによって設定される最初の間隔は1日と等しくなる可能性があります。この予期しない動作を防ぐために、1日以上の学習または再学習ステップを使用しないことをお勧めします。

それでも1日以上の学習（または再学習）ステップを使用したい場合は、FSRSヘルパーアドオンを使用してカードを毎日再スケジュールすることをお勧めします。ヘルパーアドオンはカードの全レビュー履歴を読み取ることができるため、そのような場合により正確な次の間隔を提供できます。

「各レビュー後にカードを自動再スケジュールする」オプションを有効にすると、1日以上の学習ステップを設定できます（それ以外は推奨されません）が、その場合、回答ボタンの上に表示される間隔は実際の間隔と一致しません。したがって、このオプションを無効にして1日以上の学習ステップを使用しないか、有効にして表示される間隔が実際の間隔と一致しないかを選択してください。この機能を有効にするには、ヘルパーアドオンをインストールする必要があります: https://ankiweb.net/shared/info/759844606

***

Q1: AnkiDroidはこれを無効にしますか（まだv3スケジューリングを使用していないため）？

A1: AnkiDroidは現在、v3スケジューラをサポートしており、高度な設定から有効にすることができます。ただし、FSRSが動作するために必要なカスタムスケジューリングはまだサポートしていません。AnkiDroidはv2.17でFSRSをサポートする予定です。詳細については、こちらをご覧ください: https://github.com/ankidroid/Anki-Android/issues/12620

それまでは、FSRSヘルパーアドオンで「同期後に自動再スケジュール」オプションを有効にすることができます。これにより、AnkiDroidからデスクトップにレビューを同期すると、FSRSアルゴリズムに従って自動的に再スケジュールされます。最良の結果を得るためには、レビューを毎日同期することをお勧めします。

ただし、AnkiDroidのみを使用している場合は、残念ながら対応できません。

***

Q2: 使用されていない「休眠」デッキは結果に影響しますか？

A2: 「休眠」デッキのカードは長い間隔を持つことになります。そのため、FSRS4Ankiはそれらを忘れる可能性が高いと予測します。もし覚えていた場合、FSRS4Ankiはより長い間隔を設定しますが、その間隔は遅延に対して線形にはなりません。

***

Q3: アルゴリズムはカードのイーズの変化方法を変更しますか？「良い」と「難しい」だけを選択すると、イーズ地獄に陥りますか？

A3: FSRS4Ankiはカードのイーズを難易度変数に置き換えます。難易度が低いほど、ファクターが高くなります。イーズ地獄は難易度の平均回帰によって解決されます。「良い」を連続して押すと、難易度は$D_0(3)$に収束します。アルゴリズムの詳細については、[アルゴリズム](The-Algorithm.md)をご覧ください。

***

Q4: ちゃんと動作しているかどうかを確認するにはどうすればいいですか？

A4: [AnkiWebView Inspector](https://ankiweb.net/shared/info/31746032)アドオンを使用できます。カードをレビューする前にインスペクターを開きます。すると、デバッグモードに入り、インスペクターでカスタムスケジューリングコードを見ることができます。詳細については、こちらをご覧ください: [スケジューラーの仕組み](How-does-the-scheduler-work.md)

***

Q5: 過去のレビュー履歴を使用して既存のカードに対して機能しますか？それとも、将来作成される情報にのみ機能しますか？

A5: スケジューラはレビュー履歴を使用しませんが、オプティマイザーは使用します。スケジューラはカードのイーズファクターと間隔を使用して、既存のカードのメモリ状態を設定します。また、[FSRS4Anki Helper](https://ankiweb.net/shared/info/759844606)を使用して、全レビュー履歴に基づいて既存のカードを再スケジュールすることができます。

***

Q6: 既存のデッキでこのFSRSアルゴリズムを使用し始めた後、同じデッキでAnkiの内蔵アルゴリズムに戻す必要がある場合、それは可能ですか？

A6: イーズファクターは通常通りv3スケジューラによって変更され、FSRS4Ankiはそれに介入しません。FSRS4Ankiはカードの間隔のみを変更します。したがって、問題なく戻すことができます。

***

Q7: FSRSが動作している場合、どのAnki設定が無関係になりますか（つまり、変更してもスケジューリングに影響しなくなる設定）？また、どの設定が影響を受けないままですか？

A7: FSRSは長期スケジューリングのみを変更します。そのため、「学習ステップ」と「再学習ステップ」は通常通り機能します。そして、1日以上のステップを設定しないことをお勧めします。例えば、現在のステップが「10分、1時間、1日、2日」である場合、「1日、2日」をステップから削除することをお勧めします。

最新バージョンのFSRS4Ankiでは、「最大間隔」がサポートされています。これを[fsrs4anki_scheduler.js](https://github.com/open-spaced-repetition/fsrs4anki/blob/main/fsrs4anki_scheduler.js)で変更できます。

「卒業間隔」、「簡単間隔」、「新しい間隔」、「開始イーズ」、「間隔修正」、「簡単ボーナス」、および「難しい間隔」は無関係になります。

***

Q8: 特定のデッキに異なるパラメータを設定するにはどうすればよいですか？

A8:

ステップ1: カードの表面テンプレートにコードを追加する（使用しているAnkiのバージョンが2.1.62以上の場合、このステップをスキップできます）

以下のコード `<div id=deck deck_name="{{Deck}}"></div>` をコピーして、カードの表面テンプレートに貼り付けます。

![image](https://user-images.githubusercontent.com/32575846/193453322-2e1220e1-3601-43c3-ad9f-fcd46fd85de6.png)

ステップ2: 特定のデッキのパラメータを最適化する

![image](https://user-images.githubusercontent.com/32575846/192762296-d7bd9b5e-d2d0-45af-b51d-95cd7774b353.png)

![image](https://user-images.githubusercontent.com/32575846/215369494-9a14387f-14a2-4731-8d6f-87e39c23316c.png)

ステップ3: 特定のデッキおよびそのサブデッキに異なるパラメータを設定する

![image](https://user-images.githubusercontent.com/32575846/221179126-0942a10c-5dcc-4c76-a50f-29d13b554ac0.png)

***

Q9: Ankiのアルゴリズムを変更し、その背後の計算を変更する（アドオン名）をFSRSと一緒に使用できますか？

A9: いいえ。Ankiの内蔵スケジューラに影響を与えるアルゴリズムは、FSRSを使用し始めると無効になるか、FSRSと干渉して問題を引き起こします。

***

Q10: FSRSが最近学習したカードに非常に長い間隔を設定しています。どうすれば修正できますか？

A10: 多くのユーザーにとって、Ankiのデフォルトスケジューラ（SM-2）は新しいカードを不必要に短い間隔で表示する傾向があります。そのため、ユーザーがFSRSに切り替えると、新しいカードに設定される間隔が長すぎると感じることがあります。しかし、これらの長い間隔はスケジューラコードで設定された目標保持率により適合しています。これらの長い間隔を使用することで、FSRSはSM-2を使用した場合に発生する多くの不必要なレビューを防ぐことができます。したがって、これらの長い最初の間隔を数日間試してみることをお勧めします。

それでも間隔を短くしたい場合は、スケジューラ内のrequestRetentionを増やすことができます。ただし、これにより最初の間隔だけでなく、すべての間隔が短くなることに注意してください。

***

Q11: FSRSをより効果的にするためにカードをどのように評価すればよいですか？

A11: カードの評価は、カードを答えるのがどれだけ簡単だったかに基づいて選択するべきであり、次にそのカードを見るまでの期間をどれだけ長くしたいかに基づいて選択するべきではありません。

例えば、長い間隔が表示されるために「簡単」ボタンを避ける習慣がある場合、負のサイクルに陥る可能性があります。「簡単」な状況をますます稀にし、「簡単」評価の間隔をますます長くしてしまいます。

これは、回答ボタンの上に表示される間隔を無視し、情報をどれだけよく思い出せるかに集中するべきであることを意味します。

それでもデッキを早く見たい場合、例えば試験が近づいているために早く見たい場合は、HelperアドオンのAdvance機能を使用できます。Advanceはカードの評価履歴を歪めないため、好ましい方法です。

***

Q12: 「もう一度」と「良い」しか使わないのですが、FSRSは正常に動作しますか？

A12: はい。FSRSは、「難しい」と「簡単」をほとんど使わない人と、4つのボタンすべてを頻繁に使う人に対して、ほぼ同じ精度で動作します。ただし、これは最終的な結論ではなく、データが増えるにつれてこの結論が変わる可能性があります。

***

あなたの質問に対する答えが見つかりませんか？他の人が尋ねた多くの質問はこちらです: https://github.com/open-spaced-repetition/fsrs4anki/issues?q=is%3Aissue+label%3Aquestion+

それでも問題が解決しない場合は、新しいIssueを開いて詳細を提供してください。

https://github.com/open-spaced-repetition/fsrs4anki/issues/new/choose