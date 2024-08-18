# コマンドライン

オプティマイザー用のPythonパッケージがあります。このパッケージはtorchを依存関係としているため、約500MBの空き容量が必要になることに注意してください。

## インストール

次のコマンドでパッケージをインストールします：

```
python -m pip install fsrs_optimizer
```

定期的にアップグレードして、最新バージョンの[FSRS-Optimizer](https://github.com/open-spaced-repetition/fsrs-optimizer)を使用するようにしてください：

```
python -m pip install fsrs_optimizer --upgrade
```

## 使用方法

デッキをエクスポートし、エクスポート先のフォルダーにcdで移動します。  
その後、以下のコマンドを実行できます：

```
python -m fsrs_optimizer "package.(colpkg/apkg)"
```

複数のファイルをリストすることもできます。例えば：

```
python -m fsrs_optimizer "file1.akpg" "file2.apkg"
```

ワイルドカードがサポートされています：

```
python -m fsrs_optimizer *.apkg
```

いくつかのオプションがあります。以下の通りです：

```
オプション:
  -h, --help           このヘルプメッセージを表示して終了します
  -y, --yes, --no-yes  すべての標準入力設定で自動的にデフォルトを選択します
  -o OUT, --out OUT    自動生成されたプロファイルを追加するファイル
```

## 期待される機能

![image](https://github.com/Luc-mcgrady/fsrs4anki/assets/63685643/ac2e8ae0-726c-46fd-b110-0701fa87cb66)

![image](https://github.com/Luc-mcgrady/fsrs4anki/assets/63685643/1fe8b0bb-7ac0-4a31-b594-465239ea3a1e)

# Ankiアドオン **実験的** (Anki <= 2.1.66)

[こちら](https://github.com/Luc-mcgrady/fsrs4anki-helper/tree/optimizer)のバージョンのAnkiヘルパーアドオンをダウンロードしてインストールします。Ankiアドオンフォルダにgit cloneするか、[zipとしてダウンロード](https://github.com/Luc-mcgrady/fsrs4anki-helper/archive/refs/heads/optimizer.zip)してAnkiアドオンフォルダに解凍します。

オプティマイザーをローカルにインストールします。

![image](https://user-images.githubusercontent.com/63685643/236647263-b1e57db1-4ad0-441b-9abe-91cbd36c13b0.png)  

ポップアップに注意してください。

![image](https://github.com/Luc-mcgrady/fsrs4anki/assets/63685643/ebe42eb4-f63d-4e58-b593-c173891dd29c)

ダウンロードとインストールが完了したら、Anki内からオプティマイザーを実行できるようになります。

任意のデッキの横にある歯車アイコンを押し、「最適化」オプションを選択します。

![image](https://user-images.githubusercontent.com/63685643/236647245-757ca803-b8cf-41cd-a1ae-8ed9af852ad8.png)

Ankiがオプティマイザーを読み込む間、少しの間ハングすることがあります。

![image](https://github.com/Luc-mcgrady/fsrs4anki/assets/63685643/e160e5ba-c51f-46a9-9813-9dceb18e47ff)

「はい」を押して最適な保持率を見つけるか、「いいえ」を押して見つけないか、「キャンセル」を押して別のデッキを選択します。

すべてが正常に動作していれば、最適化の進行状況を示すツールバーポップアップが表示されるはずです。

![image](https://user-images.githubusercontent.com/63685643/236647707-38101c10-ccd2-4417-aa3f-f2e4e10bb4c3.png)

その後、JavaScriptスケジューラに簡単にコピーできる形式で統計情報が表示されるはずです。

![image](https://user-images.githubusercontent.com/63685643/236647716-bfd8099a-6e7f-46e7-bce8-e18e75e75d46.png)

これらの値はアドオンの設定ファイルに保存され、Anki内で手動で保持率を変更する場合などに編集できます。

![image](https://user-images.githubusercontent.com/63685643/236647915-7a865bb0-f057-4404-af0f-27c81be99082.png)

何か問題がある場合は、このプルリクエストで言及してください [こちら](https://github.com/open-spaced-repetition/fsrs4anki-helper/pull/91)。