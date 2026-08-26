# Q1 IEE Competence guideはネットにありますか。
A1 by chatGPT

はい、あります。ただし「IEE Competence Guide」という名前だけだと、**IEEが何を指すかで資料が変わります**。

もしご質問のIEEが、旧 **Institution of Electrical Engineers（英国電気技術者協会）** のことで、**技術者・安全関連システム技術者の能力（Competence）を評価するガイド**を指しているのであれば、かなり重要な資料がネット上にあります。

特に見つかったのが、

**“Competence Criteria for Safety-related System Practitioners”**

です。これはIET（Institution of Engineering and Technology）が、HSEおよびBCSと協力して作ったものです。IEEは2006年にIIEと統合されてIETになっています。([Scribd][1])

この資料は、

* Competence Model
* Using the Competence Criteria
* Assessment of Competence
* Context and Competence Criteria
* 12のFunctions
* IEC 61508ライフサイクルとの対応

などを含んでいます。特に、**「知識があるか」だけではなく、実際にどのようなEvidence（証拠）によってCompetenceを判定するか**という考え方が入っています。([Scribd][1])

また、興味深いことに、この文書のHistoryには、もともと1999年に

> *Safety, Competency and Commitment: Competency Guidelines for Safety-Related System Practitioners*

としてIEEが出版したガイドラインが元になっている、と記載されています。([Scribd][1])

[Competence Criteria for Safety-related System Practitioners の公開資料](https://www.scribd.com/document/147072387/Competence-Criteria-for-SR-S-P?utm_source=chatgpt.com)

ただ、**「IEE Competence Guide」そのものを探しているのであれば、1999年版を含めてもう少し絞って探したほうがよさそうです。**

もし今回探されているものが、以前お話しされていた **GitHubでコンパイルエラー・実行時エラーなどの作業履歴を蓄積して、技術者のスキル／Competenceを評価する仕組み**に関係する資料でしたら、かなり近い資料だと思います。

その前提なら、**IEEの1999年版「Competency Guidelines」を中心に、現在入手できるPDF、Competence Model、評価項目を調べて、GitHubの活動履歴によるCompetence評価と対応表を作る**ところまで調べられます。

[1]: https://www.scribd.com/document/147072387/Competence-Criteria-for-SR-S-P?utm_source=chatgpt.com "Competence Criteria For SR S P | PDF | Competence (Human Resources) | Safety"


# Q2 IEE Competence guideは購入したことがあり、日本のETSS, ITSSを作成するときに参考にしたとお聞きしています。最近の生成ＡＩ利用を含めた、より実用的なものを検討しています。
A2

はい。その背景なら、かなり面白いテーマになります。

調べてみると、ご記憶の資料はほぼ間違いなく **IEE/HSE/BCSの1999年版 *Safety, Competency and Commitment: Competency Guidelines for Safety-Related System Practitioners*** だと思います。ISBNは **0-85296-787-X** です。1999年にIEEがBCSと協力して出版し、HSEが初期調査を委託しています。([OBNB][1])

そして、以前お話しされていた **ETSSとの統合**についても、公開されている記録があります。Qiitaの記事では、IEE Competency GuidelinesをETSSと統合してスキルを管理したことが明記されています。([Qiita][2])

### 私は「新しいCompetence Guide」を作るなら、かなり違うものにしたいです

1999年版の非常に良いところは、

**「何を知っているか」ではなく、「その人が仕事を遂行できることを、どのような証拠で確認するか」**

という発想です。

例えば、

> Task → Competency → Competency statement → Assessment

という構造を持っています。

さらに評価レベルとして **Supervised Practitioner / Practitioner / Expert** を設定しています。([ソフトウェアサポータビリティ][3])

これは現在でも非常に使える考え方です。

ただし、2026年のソフトウェア開発では、私はここに **Evidence（実績証拠）を大幅に強化する**のがよいと思います。

---

## 例えば「AI-era Competence Guide」にする

こんな構造です。

| 層          | 従来       | 生成AI時代                     |
| ---------- | -------- | -------------------------- |
| Knowledge  | 知識       | 知識＋AIに質問する能力               |
| Skill      | 技能       | 人間＋AIで仕事を遂行する技能            |
| Experience | 経験年数・実績  | 実際のGitHub等の活動              |
| Competence | 能力       | **成果を安定して出せる能力**           |
| Evidence   | 評価者による確認 | **コード、Issue、PR、テスト、失敗履歴等** |
| AI         | ほぼ対象外    | **AIを適切に使う能力そのものを評価**      |

特に重要なのが最後から2番目です。

### 「成功したコード」だけを評価しない

ここが、以前お話しされていたアイデアと非常に相性がいいと思います。

例えばGitHubから、

```text
Commit
  ↓
Build
  ↓
Compiler Error
  ↓
修正
  ↓
Test
  ↓
Runtime Error
  ↓
原因分析
  ↓
修正
  ↓
Test Pass
  ↓
Code Review
```

という**開発プロセスそのもの**をEvidenceとして記録する。

すると、

> 「この人はC++を知っています」

ではなく、

> 「この人はC++で実際に問題を発見し、原因を推定し、修正し、テストによって確認できる」

というCompetence評価になります。

---

## さらに生成AIを入れると面白くなる

例えば同じプログラムを、

**Aさん**

```text
AIにコードを書かせる
↓
そのままcommit
↓
動かない
↓
AIに再度聞く
↓
偶然動く
```

と、

**Bさん**

```text
自分で要求仕様を分析
↓
AIに実装案を質問
↓
生成コードをレビュー
↓
コンパイル
↓
エラーを分析
↓
AIに原因候補を質問
↓
自分で原因を判断
↓
修正
↓
テスト
↓
境界条件を追加
```

では、**同じ「完成したコード」でもCompetenceが全く違います。**

ここを評価対象にするわけです。

---

### IEE方式を現代化すると

私は次のようなモデルがかなり有望だと思います。

```text
                 Competence
                     │
       ┌─────────────┼─────────────┐
       │             │             │
   Knowledge       Skill        Experience
       │             │             │
       └─────────────┼─────────────┘
                     │
                 Performance
                     │
              ┌──────┴──────┐
              │             │
           Outcome       Evidence
              │             │
              └──────┬──────┘
                     │
              Competence Level
```

そしてEvidenceを、

```text
GitHub
 ├─ Commit
 ├─ Issue
 ├─ Pull Request
 ├─ Code Review
 ├─ CI/CD
 ├─ Compiler Error
 ├─ Runtime Error
 ├─ Test Result
 ├─ Security Finding
 └─ AI interaction metadata
```

などから集める。

**「自己申告型Skill Sheet」から「Evidence-based Competence」へ移行する**わけです。

---

## これはETSS/ITSSの次の世代になり得る

ETSS/ITSSは基本的に、

**Skill → Level**

という整理が中心です。

IEEの考え方を取り込んで、

**Task → Competency → Evidence → Assessment**

にすると、さらに実務的になります。

そして現在なら、

**Human + Generative AI**

を1つの作業単位として考える必要があります。

つまり、

> **「AIを使わずにできる能力」だけを評価するのではなく、「AIを適切に使いながら、最終成果物に責任を持てる能力」を評価する。**

これはかなり重要な転換だと思います。

実は、以前お話しされていた **「GitHubでコンパイルエラーや実行時エラーも記録して、スキル判定・経験に加算する」**というアイデアは、この方向と非常にきれいにつながります。([Qiita][2])

さらに、1999年版を2007年にIETが更新した ***Competence Criteria for Safety-related System Practitioners*** も公開資料があり、そこにはCompetence Model、Assessment of Competence、Functions/Tasks/Attributes、IEC 61508ライフサイクルとの対応などが整理されています。([Scribd][4])

**ここを原型として「IEE → ETSS/ITSS → AI時代のCompetence Guide」という系譜を整理すると、かなりしっかりした新しいモデルを作れそうです。**

もしよければ次に、**IEE 1999の12 Functions × ETSS × ITSS × GitHub Evidence × Generative AI**を1枚のマトリクスにして、**「2026年版 Competence Guide」の初版**を設計してみましょう。

[1]: https://obnb.uk/a00009692-institution-of-electrical-engineers?utm_source=chatgpt.com "Books by Institution of Electrical Engineers :: OBNB, the Open British National Bibliography"
[2]: https://qiita.com/kaizen_nagoya/items/bf4ced870733717e581a?utm_source=chatgpt.com "booklog 機能のその後 日本語(35) Reserchmap(7) #DoCAP - Qiita"
[3]: https://www.software-supportability.org/Docs/David_Gill_A_Competency.pdf?utm_source=chatgpt.com "A Competency and Skills"
[4]: https://www.scribd.com/document/147072387/Competence-Criteria-for-SR-S-P?utm_source=chatgpt.com "Competence Criteria For SR S P | PDF | Competence (Human Resources) | Safety"

# Q3 企業では、スキル表を作ったり、スキル管理をする人がスキルがないという問題が３０年ほど前から議論になっていました。製造業における工場長、技師長をＩＴ分野でも置くとよいという提案をある製造業の大企業にしたことがあります。プログラマの新人のうち２割くらいは、社内にいるプログラマの中央値よりは上だというのが経験則です。ある製造業では、その２割の人たちが３年以内に辞めていくという現象があちこちでありました。内部スキル表を作る上での課題と、ＩＴ技師長に必要なスキルの分布（Ｐｒｏｆｉｌｅ）の例をお示しください。
A3 

はい。これはかなり本質的な問題だと思います。

おっしゃっている問題を整理すると、単なる「スキル管理」の問題ではなく、

> **「人の能力を測る側に、能力を測る能力がない」**

という**メタ・コンピテンスの問題**です。

しかも、製造業の工場長・技師長に相当する人がIT部門にいないと、優秀な若手を発見できず、育成もできず、結果として外へ流出する、という現象につながります。

---

# 1. 内部スキル表の最大の問題

企業でよくあるスキル表は、例えば、

| 社員 |  C | C++ | Python | UML | Git | Linux | DB | AI |
| -- | -: | --: | -----: | --: | --: | ----: | -: | -: |
| A  |  3 |   2 |      1 |   2 |   2 |     2 |  1 |  1 |
| B  |  2 |   3 |      3 |   1 |   3 |     3 |  2 |  2 |

のようなものです。

一見便利なのですが、重大な問題があります。

### 「3」の意味が人によって違う

例えば、

> C++ = 3

について、

* 本人が自己評価した3
* 上司が評価した3
* 人事が評価した3
* 技術部長が評価した3
* 実際にC++で製品を開発できる3

は、同じではありません。

さらに、

**評価者自身の能力分布**

が分かっていない。

これが30年前から続いている問題の核心だと思います。

---

# 2. 「スキル表」より「Evidence表」にする

私は、AI時代にはスキル表そのものを少し変えたほうがいいと思います。

例えば、

```text
Skill
  ↓
Competence
  ↓
Task
  ↓
Performance
  ↓
Evidence
```

という構造です。

例えば「C++ Level 3」ではなく、

> 「C++で既存コードを理解し、変更し、テストを追加し、CIを通過させることができる」

と定義する。

その証拠として、

```text
GitHub
 ├─ Pull Request
 ├─ Code Review
 ├─ Commit
 ├─ Build
 ├─ Compiler Error
 ├─ Test
 ├─ Runtime Error
 ├─ Bug Fix
 └─ Release
```

を見る。

これなら、以前お話しされていた

> **コンパイルエラーや実行時エラーも記録して経験値にする**

という考え方が非常に有効になります。

---

# 3. 「失敗」を評価するのが重要

例えば新人AとBがいます。

A：

```text
10回実装
↓
9回成功
↓
1回失敗
```

B：

```text
30回実装
↓
20回失敗
↓
20回原因分析
↓
18回自力で修正
```

普通のスキル表なら、Aのほうが優秀に見える可能性があります。

しかし実際にはBのほうが、

**デバッグ能力・問題解決能力・学習速度**

が高い可能性があります。

ですからEvidenceとして、

* エラー発生
* 原因仮説
* 修正
* 再試験
* 結果
* 同じエラーの再発

まで記録すると面白くなります。

---

# 4. 「新人の2割が中央値より上」という経験則

ここも非常に重要です。

これは統計的に普遍的な法則として扱うべきではありませんが、仮説としてはかなり面白いです。

例えば新人100人について、

```text
能力
 ^
 |                         ●●
 |                      ●●●
 |                   ●●●●
 |                ●●●●●
 |             ●●●●●●
 |         ●●●●●●●●
 |      ●●●●●●●●●●
 +----------------------------> 人数
              ↑
            中央値
```

という分布を考えます。

重要なのは、

> **新人を全員「新人」という一つのカテゴリーに入れてしまわない**

ことです。

20人に1人ではなく、仮に100人中20人が既存プログラマの中央値を超えるなら、その20人は会社にとって極めて重要な人材候補です。

ところが、

```text
新人
 ↓
研修
 ↓
配属
 ↓
年功的昇進
```

という仕組みでは、その20人を発見できない。

さらに、上司が技術的にその20人より低い場合、

> 「扱いにくい新人」

と評価されてしまう危険すらあります。

---

# 5. ここで「IT技師長」が効いてくる

製造業では、

```text
工場長
  │
  ├─ 生産
  ├─ 品質
  ├─ 設備
  ├─ 安全
  └─ 技術
```

という構造があります。

工場長がすべての機械を操作できる必要はありません。

しかし、

> 「この人は技術的に非常に優秀だ」

ということを認識できる技術者が必要です。

ITなら、

```text
事業責任者
     │
   IT技師長
     │
 ┌───┼────┬────┐
Software Hardware Security Data/AI
```

のような役割です。

そしてIT技師長の重要な仕事は、

**「自分が一番プログラムを書くこと」ではない**

と思います。

むしろ、

> **社内の技術能力の分布を把握し、技術的に突出した人を発見し、その人が能力を発揮できる環境を作ること**

です。

---

# 6. IT技師長のSkill Profileの例

ここは通常のプログラマとは大きく違うプロフィールにしたほうがよいでしょう。

例えば100点満点ではなく、**能力の強弱をプロフィールとして持つ**。

| Competence領域                | 重み | IT技師長 |
| --------------------------- | -: | ----: |
| Software Engineering        | 15 |    80 |
| Architecture                | 15 |    95 |
| Systems Engineering         | 10 |    90 |
| Technology Evaluation       | 10 |    95 |
| Debugging / Problem Solving | 10 |    90 |
| Security                    |  7 |    80 |
| Safety / Reliability        |  7 |    85 |
| AI / Generative AI          |  8 |    85 |
| Data / Statistics           |  5 |    70 |
| Project / Organization      |  8 |    90 |
| People / Talent             | 10 |    95 |
| Business Understanding      |  5 |    85 |

単純平均を高くすることが目的ではありません。

むしろ、

**「全部80点の人」より「特定領域95点＋他領域70～90点」**

のほうが技師長として面白い。

---

# 7. 特に重要なのは「技術を見る能力」

IT技師長に必要なのは、例えば次の能力です。

### A. 技術能力

```text
Programming
Architecture
OS
Network
Database
Cloud
Security
AI
Embedded
Testing
DevOps
```

ただし全部を専門家レベルにする必要はない。

### B. 技術評価能力

こちらが非常に重要です。

```text
新技術を理解する
      ↓
本当に使えるか判断する
      ↓
PoCを設計する
      ↓
性能を測定する
      ↓
リスクを評価する
      ↓
採用／不採用を決定する
```

### C. 人材評価能力

さらに重要です。

```text
新人を見る
 ↓
成果物を見る
 ↓
問題解決過程を見る
 ↓
学習速度を見る
 ↓
能力の上限を推定する
 ↓
適切な仕事を与える
```

ここは人事評価とはかなり違います。

---

# 8. 「能力の高さ」と「現在の成果」を分離する

これは、先ほどの「2割」の話と非常に関係します。

例えば、

| 人 | 現在の成果 | 技術力 | 学習速度 | 将来性 |
| - | ----: | --: | ---: | --: |
| A |    90 |  70 |   60 |  60 |
| B |    70 |  90 |   95 |  95 |
| C |    95 |  80 |   70 |  70 |
| D |    50 |  85 |   98 |  98 |

人事制度ではAやCが高く評価されるかもしれません。

しかし**技師長が見るべきなのはBやD**です。

特にDは、

> 「今の仕事では成果が低いが、極めて速く成長している」

可能性があります。

これを発見するのが技術組織の重要な仕事です。

---

# 9. そして生成AIによって、この問題はさらに大きくなる

ここが今回の「新しいCompetence Guide」の面白いところです。

AIによって、

```text
新人
 ↓
ChatGPT / Claude / Copilot等
 ↓
かなり高度なコード
```

が簡単に生成できます。

したがって、

**「完成したコードを見ただけでは、その人の能力が分からない」**

時代になりました。

逆に、

```text
AIに何を質問したか
↓
生成結果をどう評価したか
↓
何を採用しなかったか
↓
コンパイルエラー
↓
原因分析
↓
修正
↓
テスト
```

を見ると、かなり能力が分かります。

つまり、

> **AI時代には「成果物」だけでなく「AIを含む問題解決過程」がCompetence Evidenceになる**

わけです。

---

# 10. IT技師長のProfileをもう少し抽象化すると

私はこんな五角形がよいと思います。

```text
                 技術
                  95
                  ▲
                 / \
                /   \
       人材 95 /     \ 90 問題解決
              /       \
             /         \
            ◀-----------▶
       経営・事業       AI/新技術
           85             90
              \         /
               \       /
                \     /
                 \   /
                  ▼
                組織
                 95
```

ただし、**レーダーチャートの面積を最大化することが目的ではありません。**

むしろ、

> **「どの能力を持っている人が、どの能力を持つ人を評価できるか」**

までモデル化することが重要です。

---

# 11. ここから「Skill Matrix」から「Competence Graph」へ進められる

今回の話をさらに発展させるなら、私は単なる表ではなく、

```text
Person
  │
  ├── Skill
  │
  ├── Competence
  │
  ├── Experience
  │
  ├── Evidence
  │
  └── Assessment
          │
          └── Assessor Competence
```

という**グラフ構造**にします。

すると、

> 「田中さんをC++ Level 4と評価した」

だけでは不十分で、

> 「誰が評価したのか。その評価者自身のC++ competenceはいくつか。その評価根拠は何か」

まで追跡できます。

これは、30年前からある

**「スキルを管理している人自身にスキルがない」**

という問題に真正面から対応できます。

---

## 私なら、新しいCompetence Guideの原則をこう置きます

1. **自己申告だけで能力を決めない**
2. **上司の主観だけで能力を決めない**
3. **成果物だけで能力を決めない**
4. **失敗・修正・再発防止もEvidenceにする**
5. **評価者自身のCompetenceを管理する**
6. **現在能力と成長速度を分離する**
7. **新人を一律に評価しない**
8. **突出した人材を早期発見する**
9. **AIを使った場合も、人間の判断能力を評価する**
10. **スキル表ではなくEvidence-based Competence Profileを作る**

そして、製造業の**工場長―技師長―主任技術者―作業者**という構造をITに移植すると、

**事業責任者 → IT技師長 → 技術リーダー → 開発者**

という非常に分かりやすい体系になります。

特に面白いのは、**「IT技師長自身を誰が評価するのか」**という問題です。ここまで行くと、単なる人事制度ではなく、IEEのCompetence Guideを現代化した**「Competenceを評価するCompetence」まで含む自己検証型の組織モデル**にできます。これは、生成AI時代のスキル管理としてかなり強い設計になると思います。

# Q4 私が参加したプロジェクトでは、私がスキルを判定する代わりに、３年以内にＡｍａｚｏｎに書籍のＲｅｖｉｅｗを１万冊あげて、No1. Reviewerの称号がもらえればＯＫとすることにしました。４年半かかりましたが、無事１万冊のＵｐと、No1 Reviewerの称号がもらえました。

A4 
これは非常に興味深い実例です。しかも、先ほどの「**評価する側にスキルがない問題**」に対する、かなり強い解決策になっています。

ポイントは、**「あなたがスキルを判定する」のをやめて、外部から観測可能な成果指標を設定した**ことですね。

### これは「Competence by Evidence」の実例

構造化すると、

```text
従来

本人 ──自己申告──→ スキル表
       ↑
      上司
       ↑
   評価者の主観
```

だったものを、

```text
目標設定
   ↓
「3年以内にAmazon No.1 Reviewer」
   ↓
公開された大量のReview
   ↓
第三者によるランキング
   ↓
称号
   ↓
CompetenceのEvidence
```

に変えています。

しかも重要なのは、**「本を1万冊読め」という命令ではない**ところです。

「1万冊のReviewを書き、No.1 Reviewerになる」という、第三者が確認できる**到達状態（Outcome）**を設定した。

これはかなりIEE的な発想を現代化したものだと思います。

---

### さらに面白いのは「4年半かかった」こと

当初の目標は3年以内だったのに、実際には4年半かかった。

それでも、

* 1万冊のReview
* No.1 Reviewer

という**客観的な到達結果**が得られた。

つまり、

> **期限を守ったか**

と

> **Competenceを獲得したか**

を分離できます。

これは企業のスキル評価では非常に重要です。

例えば、

| 評価方法           | 問題           |
| -------------- | ------------ |
| 研修を受けた         | 受講≠能力        |
| 資格を取った         | 資格≠実務能力      |
| 上司が5段階評価       | 評価者依存        |
| 自己評価           | 自己認識依存       |
| 経験年数           | 年数≠経験の質      |
| 1万件の公開Evidence | **第三者が検証可能** |

という違いがあります。

---

## そして、この方法は生成AI時代にさらに強くなる

例えば現在なら、

> 「生成AIを使える人」

をどう評価するか、という問題があります。

「ChatGPTを使ったことがあります」

では何も分かりません。

一方、

> GitHub上で100件のAI-assisted Pull Requestを実施し、レビュー、テスト、バグ修正まで行い、一定の品質基準を満たした

なら、かなり違います。

さらに、

```text
AI利用
 ↓
生成
 ↓
検証
 ↓
エラー発見
 ↓
原因分析
 ↓
修正
 ↓
テスト
 ↓
Review
 ↓
Release
```

というEvidenceを残せば、

**「AIにコードを書かせる能力」ではなく「AIを使ってソフトウェアを完成させる能力」**

を評価できます。

---

## ここで、先ほどの「IT技師長」にもつながります

実はIT技師長がすべきことは、

**全員を自分で評価することではない**

のかもしれません。

むしろ、

> **「この人の能力を、私の主観ではなく、何によって証明できるか？」**

を設計することです。

例えば、

```text
C++ programmer
      ↓
100件のPR
      ↓
CI成功率
      ↓
Bug修正実績
      ↓
Code Review
      ↓
設計変更
      ↓
後輩への技術支援
```

というEvidenceを組み合わせる。

AIなら、

```text
AI利用
 ↓
Prompt
 ↓
生成物
 ↓
人間による検証
 ↓
修正
 ↓
Test
 ↓
Review
```

まで含める。

---

### そして、1万Reviewの経験から一つ重要な原則が見えます

**「能力を測る」のではなく、「能力がある人なら到達できる公開された目標を設定する」**

という考え方です。

これはかなり強い。

さらに一歩進めると、

> **Competence Assessment = Judge the person**

ではなく、

> **Competence Certification = Define an Evidence-producing challenge**

と考えられます。

つまり、**評価者の能力不足問題を、評価プロセスの設計によって回避する**わけです。

今回のお話は、1999年のIEE Competency Guideを2026年版に発展させる際の、かなり重要な実例になりそうです。

