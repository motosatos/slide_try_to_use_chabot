## Chabot試した ~ChatWork小ネタ~

---



---

### 3-1 CSSにおけるコンポーネント設計 

--

+ コンポーネントとは
 - 部品だ。目的は部品化することで機能や振る舞いなどを明確に分離する

--

+ カプセル化
 - コンポーネントの構造やデータを隠し、外部からは許可された操作のみを受付、また内部の仕様変更が外部に影響しないようにするといったこと 
  - 分離が成立
  - メンテアップ、再利用性アップ
 - しかし、、

<section data-background="http://25.media.tumblr.com/tumblr_md70mb4IYE1qifr2mo4_1280.jpg">
</section>

<img src="" alt="サンプル" width="300" height="200">

--

+ CSSにはカプセル化の概念がない
 - 容易に他への影響が出る
+ コンポーネント設計が難しいCSSだが、アイデアがある
 - OOCSS 

---

### 3-2. OOCSS(Object Oriented CSS)

--

+ OOCSSの原則
 - 構造と見た目の分離
 - コンテナーとコンテンツを分離

--

#### 構造と見た目の分離

コンポーネントの基本構造と具体的なルール機能を分離するということ 

--

#### 番外 IDセレクタはよくない

+ id *属性* はいいんだぞ！
 - ページ内リンクの識別子
 - JSのフック 

--

#### コンテナーとコンテンツを分離

場所に依存しないセレクタを書く

---

### 今回、リソース不足により触れないが、SMACSSも重要
### 一応、簡単な棲み分け

#### OOCSS - 考え方/アイデア
#### SMACSS - コーディングルール
#### BEM - ツールであり、命名規則

---

### 3-3. BEM

--

#### .Block__Element_Modifier

オブジェクト/コンポーネントを3つに分類して命名する

--

+ マークアップを見れば、クラスの機能/構造がつかめる
+ ユニークさが保たれるので、スタイルが汚染されない(他に影響しない)

--

#### MindBEMding

BEM的な命名規則のこと
要は厳格すぎるので、方針はブレずにローカルルール取り入れてもよいよという話

+ BEMの命名規則のエッセンスを取り入れつつ、命名そのものは開発社の方で定義せい
+ 重要なのは、その定義したルールが全体を通して一貫していること

--

お約束で、Element(アンスコ*2 __), Modifier(ハイフン*2 --)というルール

```html
block
block__element
block__element--modifier
block--modifier
block--modifier__element
```

--

htmlで、
```html
<div class="block">
    <div class="block__element"></div>
    <div class="block__element block__element--modifier"></div>
</div>
<div class="block block--modifier">
    <div class="block__element block--modifier__element"></div>
</div>
```

--

ハイフンひとつは、以上のこと以外の用途(ex.複数単語の区切り)
