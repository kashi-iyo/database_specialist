# 正規化を要約

- 次の「伝票」関係（リレーション）を用いて正規化を行っていく。

  | 伝票番号 | 顧客名（顧客番号） | 商品番号     | 商品名                                      | 数量         |
  | -------- | ------------------ | ------------ | ------------------------------------------- | ------------ |
  | 1001     | ねこ商事（11）     | 2001<br>2002 | チョコレート<br>キャラメル                  | 5<br>10      |
  | 1002     | うさぎ開発（13）   | 2001         | チョコレート<br> ドーナツ<br>シュークリーム | 5<br>20<br>5 |
  | 1003     | くま工業（29）     | 2005         | プロテイン                                  | 30           |
  | 1004     | くま工業（29）     | 2005         | プロテイン                                  | 30           |

- 条件

  - 顧客は複数回注文するので、伝票は複数ある

  - 顧客番号に対応する顧客名の重複はない

  - 商品番号に対応する商品名の重複はない

## 第１正規形にする

- [第１正規形へ正規化する目的](#第１正規形へ正規化する目的)
- [第１正規形へ正規化する方法](#第１正規形へ正規化する方法)
- [ドメインから直積集合を排除する](#ドメインから直積集合を排除する)
- [ドメインから冪集合を排除する](#ドメインから冪集合を排除する)
- [候補キーを選択](#候補キーを選択)
- [主キーを選択](#主キーを選択)

### 第１正規形へ正規化する目的

すべてのドメインをシンプルにする

### 第１正規形へ正規化する方法

1. ドメインから **直積集合** を排除する

2. ドメインから **冪集合** を排除する

3. **候補キー** を決定する

4. **主キー** を決定する

### ドメインから直積集合を排除する

**顧客名（顧客番号）** が直積集合なので、こいつを分解して、次のような関係（リレーション）にする

| 伝票番号 | 顧客名     | 顧客番号 | 商品番号     | 商品名                                      | 数量         |
| -------- | ---------- | -------- | ------------ | ------------------------------------------- | ------------ |
| 1001     | ねこ商事   | 11       | 2001<br>2002 | チョコレート<br>キャラメル                  | 5<br>10      |
| 1002     | うさぎ開発 | 13       | 2001         | チョコレート<br> ドーナツ<br>シュークリーム | 5<br>20<br>5 |
| 1003     | くま工業   | 29       | 2005         | プロテイン                                  | 30           |
| 1004     | くま工業   | 29       | 2005         | プロテイン                                  | 30           |

### ドメインから冪集合を排除する

複数の値を持っているドメインは「商品番号」「商品名」「数量」なので、こいつらを分解して、次のような関係（リレーション）にする

| 伝票番号 | 顧客番号 | 顧客名     | 商品番号 | 商品名         | 数量 |
| -------- | -------- | ---------- | -------- | -------------- | ---- |
| 1001     | 11       | ねこ商事   | 2001     | チョコレート   | 5    |
| 1001     | 11       | ねこ商事   | 2002     | キャラメル     | 10   |
| 1002     | 13       | うさぎ開発 | 2001     | チョコレート   | 5    |
| 1002     | 13       | うさぎ開発 | 2003     | ドーナツ       | 20   |
| 1002     | 13       | うさぎ開発 | 2004     | シュークリーム | 5    |
| 1003     | 29       | くま工業   | 2005     | プロテイン     | 30   |
| 1004     | 29       | くま工業   | 2005     | プロテイン     | 30   |

### 候補キーを選択

- 条件

  - 顧客は複数回注文するので、伝票は複数ある

  - 顧客番号に対応する顧客名の重複はない

  - 商品番号に対応する商品名の重複はない

- **関係（リレーション）のタプルを一意に識別** できる属性

  - `{伝票番号, 顧客番号, 顧客名, 商品番号, 商品名, 数量}` の全ての属性

  - 値が全部同一のタプルがないので

- 属性の組のうち **極小** の属性

  - 1 つ目の「極小」を見つける

    1. {数量} を検査：無くなってもタプルは一意性を保てる

       ```
       {伝票番号, 顧客番号, 顧客名, 商品番号, 商品名}
       ```

    2. {商品名} を検査：「商品番号に対応する商品名の重複はない」かつ、この属性によってタプルは一意に識別可能なので、残す。また「極小」を求めるにあたり {商品番号} の方は不要なので除外する

       ```
       {伝票番号, 顧客番号, 顧客名, 商品名}
       ```

    3. {顧客名} を検査：「顧客番号に対応する顧客名の重複はない」という条件があるかつ、顧客データが無くなってもタプルの一意性は保てるので除外。また、{顧客番号} も除外できる

       ```
       {伝票番号, 商品名}
       ```

  - 2 つ目の「極小」はある？

    1. 1 回目の検査から {顧客番号}, {顧客名}, {数量} は無くてもタプルを一意に識別できるとわかったののと、{商品名} と {商品番号} を入れ替えできるので次のようにできる

       ```
       {伝票番号, 商品番号}
       ```

### 主キー選択

- 候補キーの中から以下の 2 つの制約に合致する属性を選択する

  1. 主キーはタプルの一意識別能力を備えていること

  2. 主キーを構成する属性は空値（NULL）を取らないこと

- 列挙された候補キーは `{伝票番号, 商品番号}`, `{伝票番号, 商品名}` 。この中からふさわしい属性を選ぶのだが、"伝票番号 " が良さそうだ

## 第２正規形にする

- [前提](#前提)
- [第２正規形へ正規化する目的](#第２正規形へ正規化する目的)
- [第２正規形へ正規化する方法](#第２正規形へ正規化する方法)
- [候補キーを列挙しとく](#候補キーを列挙しとく)
- [非キー属性を列挙する](#非キー属性を列挙する)
- [候補キーと非キー属性の間に完全関数従属の関係が成立しているかどうかを検査する](#候補キーと非キー属性の間に完全関数従属の関係が成立しているかどうかを検査する)
- [部分関数従属の排除](#部分関数従属の排除)
- [第２正規形をリレーションで表現](#第２正規形をリレーションで表現)

### 前提

- 第１正規形にした「伝票」関係（リレーション）を用いる

  | 伝票番号 | 顧客番号 | 顧客名     | 商品番号 | 商品名         | 数量 |
  | -------- | -------- | ---------- | -------- | -------------- | ---- |
  | 1001     | 11       | ねこ商事   | 2001     | チョコレート   | 5    |
  | 1001     | 11       | ねこ商事   | 2002     | キャラメル     | 10   |
  | 1002     | 13       | うさぎ開発 | 2001     | チョコレート   | 5    |
  | 1002     | 13       | うさぎ開発 | 2003     | ドーナツ       | 20   |
  | 1002     | 13       | うさぎ開発 | 2004     | シュークリーム | 5    |
  | 1003     | 29       | くま工業   | 2005     | プロテイン     | 30   |
  | 1004     | 29       | くま工業   | 2005     | プロテイン     | 30   |

### 第２正規形へ正規化する目的

- 全ての非キー属性が、各候補キーに完全関数従属している関係を作ること

### 第２正規形へ正規化する方法

- 各候補キーの一部に従属する部分関数従属性を排除していくこと

  - ここでいう「排除」とは、**部分関数従属な候補キーを主キーとし、非キー属性を別関係（リレーション）へ移動させること**

- 第２正規形へ正規化する手順

  1. 候補キーを列挙

  2. 非キー属性を列挙

  3. 候補キーと非キー属性の間に完全関数従属の関係が成立しているかどうかを検査する

     - これをやった結果、部分関数従属があるかどうかがわかる

  4. もしも部分関数従属がある場合、部分関数従属な関係にある **候補キーの一部** を主キーとした関係（リレーション）を新たに作り、関数従属している非キー属性をそちらの関係（リレーション）へ移動させる

### 候補キーを列挙しとく

- 第１正規形にて列挙した候補キーは次の通り

  - `{伝票番号, 商品番号}`

  - `{伝票番号, 商品名}`

### 非キー属性を列挙する

- 非キー属性 = どの候補キーにも当てはまらない属性

- 非キー属性：`{顧客番号, 顧客名}, {数量}`

### 候補キーと非キー属性の間に完全関数従属の関係が成立しているかどうかを検査する

#### 定義のおさらい

- 完全関数従属 = 関数従属性 X → Y において、X の全ての真部分集合 X’ について、X’ → Y が成立しないこと

- X ：候補キー

- X'：候補キーの各属性

- Y：非キー属性を当てはめる

#### 候補キー `{伝票番号, 商品番号}`、非キー属性 `{数量}` の完全関数従属が成立してるかどうかを検査する

- 前提

  - X：`{伝票番号, 商品番号}`

  - X'：`{伝票番号}`, `{商品番号}`

  - Y：`{数量}`

- `{伝票番号}` → `{数量}` は成立するか？

  - 伝票番号が決まっても、数量は決まらないので **成立しない**

- `{商品番号}` → `{数量}` は成立するか？

  - 商品番号が決まっても、数量は決まらないので **成立しない**

- 結果

  - どちらも成立していないので、完全関数従属している

- 備考

  - `{商品名}` は `{商品番号}` に対して一意に決まる属性なので、`{伝票番号, 商品名}` は検査しない

#### 候補キー `{伝票番号, 商品番号}`、非キー属性 `{顧客番号, 顧客名}` の完全関数従属が成立してるかどうかを検査する

- 前提

  - X：`{伝票番号, 商品番号}`

  - X'：`{伝票番号}`, `{商品番号}`

  - Y：`{顧客番号, 顧客名}`

- `{伝票番号}` → `{顧客番号, 顧客名}` は成立するか？

  - 伝票番号が決まると、顧客番号は決まる（また、顧客名は顧客番号が決まれば決まる）ので、 **成立する**

- `{商品番号}` → `{顧客番号, 顧客名}` は成立するか？

  - 商品番号が決まっても、顧客番号（顧客名）は決まらないので **成立しない**

- 結果

  - 一方だけ成立している状態なので、部分関数従属

### 部分関数従属の排除

- `{伝票番号}` → `{顧客番号, 顧客名}` を別関係（リレーション）へ移動させる

  - 主キーが `{伝票番号}`、非キー属性が `{顧客番号, 顧客名}` な関係（リレーション）を新たに作る

- 結果、次のような関係スキーマ 2 つが出来上がる

  - 伝票明細（伝票番号, 商品番号, 商品名, 数量）

  - 伝票（伝票番号, 顧客番号, 顧客名）

### 第２正規形をリレーションで表現

- 伝票明細リレーション

  | 伝票番号 | 商品番号 | 商品名         | 数量 |
  | -------- | -------- | -------------- | ---- |
  | 1001     | 2001     | チョコレート   | 5    |
  | 1001     | 2002     | キャラメル     | 10   |
  | 1002     | 2001     | チョコレート   | 5    |
  | 1002     | 2003     | ドーナツ       | 20   |
  | 1002     | 2004     | シュークリーム | 5    |
  | 1003     | 2005     | プロテイン     | 30   |
  | 1004     | 2005     | プロテイン     | 30   |

- 伝票リレーション

  | 伝票番号 | 顧客番号 | 顧客名     |
  | -------- | -------- | ---------- |
  | 1001     | 11       | ねこ商事   |
  | 1001     | 11       | ねこ商事   |
  | 1002     | 13       | うさぎ開発 |
  | 1002     | 13       | うさぎ開発 |
  | 1002     | 13       | うさぎ開発 |
  | 1003     | 29       | くま工業   |
  | 1004     | 29       | くま工業   |
