FSRSは、Jarrett Yeによって開発された最新の[間隔反復](https://en.wikipedia.org/wiki/Spaced_repetition)アルゴリズムです。これは、Ankiの従来のSM-2アルゴリズムよりも効率的にレビューをスケジュールし、あなたの記憶パターンを学習することを目指しています。

間隔反復アルゴリズムの目標は、レビュー間の最適な間隔を計算することです。しかし、何が「最適」な間隔を決定するのでしょうか？FSRSでは、カードを思い出す確率に対応する間隔が最適と見なされます。例えば、次回カードを見たときに90%の確率で思い出せるようにしたい場合、その確率が90%になる間隔が最適な間隔です。

FSRSは「記憶の三成分モデル」に基づいています。このモデルは、人間の脳内の単一の記憶の状態を説明するのに3つの変数が十分であると主張しています。これらの3つの変数には以下が含まれます：

* 再認可能性 (R): 特定の情報を特定の瞬間に成功裏に思い出せる確率。これは、最後のレビューから経過した時間と記憶の安定性 (S) に依存します。

* 安定性 (S): Rが100%から90%に低下するのに必要な日数。例えば、S = 365の場合、特定のカードを思い出す確率が90%に低下するまでに1年が経過することを意味します。

* 難易度 (D): 特定の情報の固有の複雑さ。これは、レビュー後に記憶の安定性を向上させるのがどれほど難しいかを表します。

FSRSでは、これらの3つの値をまとめて「記憶状態」と呼びます。Rの値は毎日変化しますが、DとSはカードがレビューされた後にのみ変化します。FSRSはその日の最初のレビューのみを考慮します。各カードには独自のDSR値、つまり各カードには独自の記憶状態があります。

DSR値を正確に推定するために、FSRSはユーザーのレビュー履歴を分析し、レビュー履歴に最も適合するパラメータを計算するために機械学習を使用します。FSRSの最新バージョンでは、DとSの計算式に17のパラメータを使用しています（再認可能性の計算式にはパラメータは必要ありません）。詳細に興味がある場合は、次のWikiページを参照してください: [アルゴリズム](The-Algorithm.md) および [最適化の仕組み](The-mechanism-of-optimization.md)。ユーザーに十分なレビューがない場合は、代わりにデフォルトのパラメータが使用されます。これらのパラメータは、約2万人のユーザーからの数億回のレビューを基にFSRSオプティマイザーを実行して見つけられました。デフォルトのパラメータでも、FSRSはAnkiのデフォルトアルゴリズムよりも優れています。

ユーザーはパラメータを手動で調整しないように注意してください。スケジューリングを調整したい場合は、望ましい保持率の適切な値を選択するだけで済みます。70%から97%の値が合理的と見なされます。言い換えれば、FSRSを使用すると、ユーザーは特定の保持率の値を目標にすることができ、どれだけ覚えているかと、どれだけのレビューを行う必要があるかのバランスを取ることができます。保持率が高いほど、1日に行うレビューの数が増えます。

ユーザーが望む保持率を選択できることに加えて、FSRSにはAnkiのデフォルトアルゴリズムと比較していくつかの利点があります。FSRSを使用すると、同じ保持率を達成するためにAnkiのデフォルトアルゴリズムよりも20〜30％少ないレビューで済みます。また、FSRSは遅延してレビューされたカードのスケジューリングが非常に得意です。例えば、ユーザーが数週間Ankiを休んだ場合などです。さらに、[FSRS4Anki Helperアドオン](https://github.com/open-spaced-repetition/fsrs4anki-helper)は、他では利用できないいくつかの便利な機能を提供します。

Ankiのバージョンが23.10以降の場合は、[このガイド](tutorial.md)を読んでください。Ankiのバージョンが23.10より古い場合は、FSRSのスタンドアロンバージョンを使用できます。インストール方法については、[このガイド](intro.md#始め方)を読んでください。

FSRSが他のアルゴリズムと比較してどのように機能するかを確認したい場合は、次のページを読んでください: [ベンチマーク](https://github.com/open-spaced-repetition/fsrs-benchmark) および [FSRS vs SM-17](https://github.com/open-spaced-repetition/fsrs-vs-sm17)（最新のSuperMemoアルゴリズムの一つ）。

FSRSに関するさらなる質問がある場合は、[FAQ](FAQ.md)を確認してください。

間隔反復アルゴリズムについてもっと知りたい場合は、[間隔反復アルゴリズム: 初心者から専門家までの3日間の旅](Spaced-Repetition-Algorithm-‐-A-Three‐Day-Journey-from-Novice-to-Expert.md)をチェックしてください。