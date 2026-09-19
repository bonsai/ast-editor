# ast-editor

**ast-editor は、プログラム世界を AST として観察・編集するための境界層です。**

このリポジトリは、もともと C++ などのプログラムを syntax tree / program model として扱う Bonsai を基礎にしています。現在の World Type System では、これを「プログラム世界を型付き構造として扱うための編集器」と位置づけます。

## World Type System における位置

世界には型があります。

```
World
│
├── Entity       何があるか
├── State        どうなっているか
├── Operation    何ができるか
├── Relation     どう結びつくか
├── Type         何として扱うか
└── Boundary     どこまでを一つの世界・概念として扱うか
        │
        └── Software World
              │
              └── Language
                    │
                    └── AST
                          │
                          └── ast-editor
```

AST は World そのものではありません。

**AST は、Language によって記述された Software World を、型付きの木構造として表現したものです。**

したがって ast-editor の役割は、単にコードを書く GUI ではなく、

> **プログラムという世界を、AST という構造で見る・調べる・操作するための Boundary**

です。

## Dictionary / Type / Ontology / System / Boundary

World の基本要素との対応は次のようになります。

| World | ast-editor での位置 |
|---|---|
| **Dictionary** | ノードや属性の名前・値を扱う |
| **Type** | Variable、Function、Call、Class などのプログラム要素の型 |
| **Ontology** | プログラム要素が何であり、何と関係するかという意味体系 |
| **System** | Entity と Operation の関係からなるプログラム構造 |
| **Boundary** | Source Code ↔ AST ↔ Program Model の変換・操作境界 |

ここで重要なのは、**AST と Ontology を同一視しないこと**です。

Ontology は意味と関係を定義します。
AST は、その意味体系に対応する構造を Language の構文から生成します。

```
World Ontology
      ↓
Language Type System
      ↓
Source Code
      ↓ parse
     AST
      ↓
Program Model
      ↓
analysis / edit / transform
      ↓
Program World
```

## AST と World

例えば、

```cpp
x = f(a)
```

という記述を AST として見ると、

```
Assignment
├── Variable(x)
└── Call
    ├── Function(f)
    └── Argument(a)
```

となります。

World の語彙に戻すと、

```
Entity:
  x
  f
  a

Operation:
  assign
  call

Relation:
  x ← result(f(a))

State:
  x = result(f(a))
```

となります。

つまり AST は、**Entity・Operation・Relation を持つ Software World の構造的な観測結果**として読むことができます。

## ast-editor の役割

ast-editor は次の循環を支える層です。

```
observe
   ↓
parse
   ↓
AST
   ↓
query
   ↓
understand
   ↓
edit / transform
   ↓
program
   ↓
observe
```

特に重要なのは **query と manipulation** です。

AST を固定された解析結果として見るのではなく、プログラム世界に対して、

- 何があるか
- 何と何が関係するか
- どの Entity がどの Operation を持つか
- どの State を生むか
- どこが Boundary か

を探索し、必要なら構造を変更します。

## BonsaiCode との関係

`BonsaiCode` と `ast-editor` は、World Type System では次の位置になります。

```
bonsai/type
    │
    │ World の型を定義
    ↓
Software World
    │
    ├── Language
    │
    ├── AST
    │
    └── Program Model
             │
             ↓
        ast-editor
```

したがって、`ast-editor` は `type` の代替ではありません。

**type が World の型を扱い、ast-editor はその型体系を Software World に適用して、AST / Program Model を観察・操作する層です。**

## 一言で定義する

> **ast-editor = Software World を AST / Program Model として見るための Boundary**

そして、

> **AST = Language が Software World を構造化して表現したもの**

です。

この位置づけによって、AST は単なる「コード解析技術」ではなく、World Type System における **Software World の観測・操作モデル**として扱えます。
