# 第 2 正規形

## 第 2 正規形とは？

- 全ての非キー属性が各候補キーに完全関数従属している関係のこと

## 第 2 正規形の定義

- 関係 R が第２正規形（the second normal from, 2NF）であるとは、次の 2 つの条件を満たすときを言う

  1. **R は第１正規形である**

  2. **R の全ての非キー属性は R の各候補キーに完全関数従属している**

### 非キー属性とは？

- どの候補キーにも当てはまらない属性

### 完全関数従属とは？

- 関数従属性 X → Y において、X の全ての真部分集合 X’ について、X’ → Y が成立しないこと

## 第２正規形にする目的は？

- 第２正規形になっていないことで発生する更新時異状を防ぐこと

- 第２正規形になっていないことで発生する更新時異状 = 部分関数従属性があることによる異状

## 第 2 正規形にする方法は？

- 各候補キーの一部に従属する部分関数従属性を排除していく