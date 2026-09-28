# 自分専用 Swift 学習ノート

> **氏名：** （安住光司）
> **学籍番号：** （26cm0101）
> **開始日：** 2026年9月

---

## この学習ノートについて

このリポジトリは、Swift（1年生・後期）の授業で使う、あなた専用の学習ノートです。.

文法の説明（リファレンス）は先生が書きます。ここに書くのは、あなた自身の言葉と実験です。

- 教材コードを読んで分かったこと
- 生成AIに書かせたコードを、自分がどう変えて、どう説明できるか（4点セット）
- コードを壊して試したら何が起きたか
- 生成AIに何を聞いて、何が分かったか（AI質問ログ）

同じ教材コードから始めても、つまずく場所も、気づくことも、一人ひとり違います。だからこの学習ノートは、世界に一つだけのものになります。

毎回の授業の最低ラインは「コミット1回」です。書きかけでかまいません。GitHub に保存された状態で授業を終えてください。

---

## 目次

| 週 | 単元 | リファレンス | ファイル | 状態 |
|---|------|------|------|------|
| 日専祭 | 展示アプリ | — | [festival/README.md](festival/README.md) ／ [festival/READING.md](festival/READING.md) | 未着手 |
| week01 | 型と制御構文の再確認、配列・辞書・範囲 | 01 | [week01/NOTE.md](week01/NOTE.md) ／ [week01/main.swift](week01/main.swift) | 未着手 |
| week02 | Optional | 02 | [week02/NOTE.md](week02/NOTE.md) ／ [week02/main.swift](week02/main.swift) | 未着手 |
| week03 | 関数とタプル | 03 | [week03/NOTE.md](week03/NOTE.md) ／ [week03/main.swift](week03/main.swift) | 未着手 |
| week04 | 構造体とクラス、値型と参照型 | 04 | [week04/NOTE.md](week04/NOTE.md) ／ [week04/main.swift](week04/main.swift) | 未着手 |
| week05 | 列挙型と switch | 05 | [week05/NOTE.md](week05/NOTE.md) ／ [week05/main.swift](week05/main.swift) | 未着手 |
| week06 | クロージャと高階関数 | 06 | [week06/NOTE.md](week06/NOTE.md) ／ [week06/main.swift](week06/main.swift) | 未着手 |
| week07 | プロトコルと extension | 07 | [week07/NOTE.md](week07/NOTE.md) ／ [week07/main.swift](week07/main.swift) | 未着手 |
| week08 | エラー処理とジェネリクス入門 | 08 | [week08/NOTE.md](week08/NOTE.md) ／ [week08/main.swift](week08/main.swift) | 未着手 |
| week09 | 総合演習（買い物メモアプリを読む・直す・広げる） | 09 | [week09/NOTE.md](week09/NOTE.md) ／ [week09/main.swift](week09/main.swift) | 未着手 |
| week10 | 総復習、模擬試験、持ち帰り版 | 全章 | [week10/NOTE.md](week10/NOTE.md) | 未着手 |

> 💡 その週の課題を終えたら「状態」を「✅ 完了」に更新してください。
> 💡 リファレンスの列の番号は、教材コードリポジトリ `swift-grammar-code` の `reference/` にある章の番号です。

---

## AI利用レベルの凡例

| レベル | 名前 | 意味 |
|:--:|------|------|
| 0 | 使わない | 生成AIを開かない。確認テストと期末試験 |
| 1 | 質問のみ | 聞いてよいが、コードは書かせない。白紙再実装と読解ノート |
| 2 | 生成コード可 | 書かせてよい。ただし4点セットを NOTE.md に書く。金曜の演習と日専祭アプリ |

詳しくは配布資料「生成AIの使い方ルール」を読んでください。

---

## 提出物の定義

| 提出物 | 何をもって提出とするか |
|------|------|
| weekNN/main.swift | 教材コードの「演習」と書かれている部分のチェックがすべて OK になったものを Upload files で上書きしてコミット |
| weekNN/NOTE.md | 4点セット（プロンプト／変えた点／自分の言葉の説明／壊す実験）と AI質問ログが埋まっている |
| festival/README.md | アプリ名、できること、生成AIの使い方が書いてある |
| festival/READING.md | 自分のアプリのコード3箇所を説明している |

4点セットが無い週は、コードが動いていても未提出と同じ扱いです。

---

## 学期末：持ち帰り版の作り方

学期の最後に、先生が書いたリファレンス（PDF）と、この学習ノートと、期末試験用に自分で作る A4 一枚のシートを、一冊にまとめる手順を配ります。就職してからも手元に置ける、あなた専用の Swift の本になります。手順は第30回（2/1）の授業メモに載せます。
