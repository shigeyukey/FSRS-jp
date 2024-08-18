<p align="center">
  <a href="https://github.com/open-spaced-repetition/fsrs4anki/wiki">
    <img src="https://github.com/open-spaced-repetition/fsrs4anki/assets/32575846/9efb2ca5-51bd-411d-9694-a77b09f51fa7" width="150" height="150" alt="FSRS4Anki">
  </a>
</p>

<div align="center">

# FSRS4Anki

_✨ [フリー間隔反復スケジューラーアルゴリズム](https://github.com/open-spaced-repetition/fsrs4anki/wiki/The-Algorithm) に基づいたAnki用の最新の間隔反復スケジューラー ✨_

</div>

<p align="center">
  <a href="https://raw.githubusercontent.com/open-spaced-repetition/fsrs4anki/main/LICENSE">
    <img src="https://img.shields.io/github/license/open-spaced-repetition/fsrs4anki" alt="license">
  </a>
  <a href="https://github.com/open-spaced-repetition/fsrs4anki/releases/latest">
    <img src="https://img.shields.io/github/v/release/open-spaced-repetition/fsrs4anki?color=blueviolet" alt="release">
  </a>
</p>

# 目次

- [はじめに](#はじめに)
- [始め方](#始め方)
- [アドオンの互換性](#アドオンの互換性)
- [貢献](#貢献)
  - [貢献者](#貢献者)
- [スターの推移](#スターの推移)

# はじめに

FSRS4Ankiは、スケジューラーとオプティマイザー(最適化装置)の2つの主要な部分で構成されています。

- スケジューラーはAnkiの内蔵スケジューラーを置き換え、FSRSアルゴリズムに従ってカードをスケジュールします。
- オプティマイザーは機械学習を使用してあなたの記憶パターンを学習し、レビュー履歴に最適なパラメータを見つけます。オプティマイザーの動作の詳細については、[最適化の仕組み](https://github.com/open-spaced-repetition/fsrs4anki/wiki/The-mechanism-of-optimization)をお読みください。

FSRSアルゴリズムの詳細については、[アルゴリズム](https://github.com/open-spaced-repetition/fsrs4anki/wiki/The-Algorithm)をお読みください。興味があれば、私の論文もご覧いただけます：
- [間隔反復スケジューリングを最適化するための確率的最短経路アルゴリズム](https://www.maimemo.com/paper/)（無料アクセス）、および
- [記憶のダイナミクスを捉えて間隔反復スケジュールを最適化する](https://www.researchgate.net/publication/369045947_Optimizing_Spaced_Repetition_Schedule_by_Capturing_the_Dynamics_of_Memory)（リクエストを提出）。

FSRS4Anki Helperは、FSRS4Anki Schedulerを補完するAnkiのアドオンです。詳細はこちらをご覧ください：https://github.com/open-spaced-repetition/fsrs4anki-helper

# 始め方

Anki v23.10以降を使用している場合は、[このチュートリアル](https://github.com/open-spaced-repetition/fsrs4anki/blob/main/docs/tutorial.md)を参照してください。

古いバージョンのAnkiを使用している場合は、[このチュートリアル](https://github.com/open-spaced-repetition/fsrs4anki/blob/main/docs/tutorial2.md)を参照してください。

Anki v23.10以降では、FSRSの設定がはるかに簡単になっています。

# アドオンの互換性

いくつかのアドオンはFSRSと競合する可能性があります。一般的なルールとして、カードの間隔に影響を与えるアドオンはFSRSと一緒に使用すべきではありません。

| アドオン                                                       | 互換性あり? | コメント |
| ------------------------------------------------------------ |-------------------| ------- |
| [Review Heatmap](https://ankiweb.net/shared/info/1771074083) | はい :white_check_mark: | FSRSに関連するものには影響しません。 |
| [Advanced Browser](https://ankiweb.net/shared/info/874215009) | はい :white_check_mark: | 最新バージョンを使用してください。 |
| [Advanced Review Bottom Bar](https://ankiweb.net/shared/info/1136455830) | はい :white_check_mark: | 最新バージョンを使用してください。 |
| [The KING of Button Add-ons](https://ankiweb.net/shared/info/374005964) | はい :white_check_mark: | 最新バージョンを使用してください。 |
| [Pass/Fail](https://ankiweb.net/shared/info/876946123) | はい :white_check_mark: | `Pass`は`Good`に相当し、`Fail`は`Again`に相当します。 |
| [AJT Card Management](https://ankiweb.net/shared/info/1021636467) | はい :white_check_mark: | Anki 23.12以降と互換性があります。 |
| [Incremental Reading v4.11.3 (unofficial clone)](https://ankiweb.net/shared/info/999215520) | 不明 :question: | スタンドアロン版のFSRSを使用している場合、Ankiの内蔵スケジューラーによって提供される間隔が表示され、カスタムスケジューラーでは表示されません。このアドオンは技術的には内蔵FSRSと互換性がありますが、FSRSはインクリメンタルリーディング用に設計されておらず、FSRSの設定はIRカードには適用されません。IRカードは他のカードタイプとは異なる方法で動作します。 |
| [Delay siblings](https://ankiweb.net/shared/info/1369579727) | いいえ :x:| Delay siblingsはFSRSによって提供される間隔を変更します。ただし、FSRS4Anki HelperアドオンにはFSRSとよりよく連携する類似の機能があります。FSRS4Anki Helperアドオンを使用してください。 |
| [Auto Ease Factor](https://ankiweb.net/shared/info/1672712021) | いいえ :x: | FSRSが有効な場合、Ease Factorはもはや関連性がないため、このアドオンを使用してもメリットはありません。 |
| [autoLapseNewInterval](https://ankiweb.net/shared/info/372281481) |いいえ :x:| FSRSが有効な場合、`New Interval`設定はもはや関連性がないため、このアドオンを使用してもメリットはありません。 |
| [Straight Reward](https://ankiweb.net/shared/info/957961234) | いいえ :x: | FSRSが有効な場合、Ease Factorはもはや関連性がないため、このアドオンを使用してもメリットはありません。 |

FSRSと他のアドオンの互換性を確認してほしい場合は、[issues](https://github.com/open-spaced-repetition/fsrs4anki/issues)でお知らせください。

# 貢献

FSRS4Ankiに貢献する方法として、ベータテスト、コードの提出、データの共有があります。データを共有したい場合は、こちらのフォームにご記入ください：https://forms.gle/KaojsBbhMCytaA7h8

## 貢献者

<!-- ALL-CONTRIBUTORS-BADGE:START - Do not remove or modify this section -->
[![All Contributors](https://img.shields.io/badge/all_contributors-3-orange.svg?style=flat-square)](#貢献者-)
<!-- ALL-CONTRIBUTORS-BADGE:END -->

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
<table>
  <tbody>
    <tr>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/Expertium"><img src="https://avatars.githubusercontent.com/u/83031600?v=4?s=100" width="100px;" alt="Expertium"/><br /><sub><b>Expertium</b></sub></a><br /><a href="https://github.com/open-spaced-repetition/fsrs4anki/commits?author=Expertium" title="テスト">⚠️</a> <a href="https://github.com/open-spaced-repetition/fsrs4anki/commits?author=Expertium" title="ドキュメント">📖</a> <a href="#data-Expertium" title="データ">🔣</a> <a href="#ideas-Expertium" title="アイデア、計画、フィードバック">🤔</a> <a href="https://github.com/open-spaced-repetition/fsrs4anki/issues?q=author%3AExpertium" title="バグ報告">🐛</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/user1823"><img src="https://avatars.githubusercontent.com/u/92206575?v=4?s=100" width="100px;" alt="user1823"/><br /><sub><b>user1823</b></sub></a><br /><a href="https://github.com/open-spaced-repetition/fsrs4anki/commits?author=user1823" title="テスト">⚠️</a> <a href="https://github.com/open-spaced-repetition/fsrs4anki/commits?author=user1823" title="ドキュメント">📖</a> <a href="#data-user1823" title="データ">🔣</a> <a href="#ideas-user1823" title="アイデア、計画、フィードバック">🤔</a> <a href="https://github.com/open-spaced-repetition/fsrs4anki/issues?q=author%3Auser1823" title="バグ報告">🐛</a></td>
      <td align="center" valign="top" width="14.28%"><a href="http://chrislongros.com"><img src="https://avatars.githubusercontent.com/u/98426896?v=4?s=100" width="100px;" alt="Christos Longros"/><br /><sub><b>Christos Longros</b></sub></a><br /><a href="#data-chrislongros" title="データ">🔣</a> <a href="#content-chrislongros" title="コンテンツ">🖋</a></td>
    </tr>
  </tbody>
</table>

<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->

<!-- ALL-CONTRIBUTORS-LIST:END -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->

<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->

<!-- ALL-CONTRIBUTORS-LIST:END -->

# スターの推移

[![スター履歴チャート](https://api.star-history.com/svg?repos=open-spaced-repetition/fsrs4anki&type=Date)](https://star-history.com/#open-spaced-repetition/fsrs4anki&Date)
