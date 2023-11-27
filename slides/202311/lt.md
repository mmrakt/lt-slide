---
marp: true
theme: gaia
style: |
  section h1, section h2, section h3 {
    font-weight: 500;
  }
  section.center {
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
  }
  section.horizonal-center {
      display: flex;
      flex-direction: column;
      justify-content: center;
  }

  section.normal {
    display: block;
  }
  section.text-center {
    text-align: center;
    vertical-align: middle;
  }
---

<!--
class: center invert
-->

## Astro は優秀だが万能ではない（かも）

---

<!--
class: invert
-->

## 背景

Astro を個人ブログや LP 案件で使用してみたのでその所感についてお話しします

---

<!--
class: horizonal-center invert
-->

## Astro について

> Astro は、コンテンツにフォーカスした高速な Web サイトを構築するためのオールインワン Web フレームワークです。

ref: https://docs.astro.build/ja/concepts/why-astro/

---

## Astro について

> Astro を選ぶ理由
>
> 1.  コンテンツ重視
> 2.  サーバーファースト
> 3.  デフォルトで高速
> 4.  簡単に使える
> 5.  充実した機能と柔軟性

ref: https://docs.astro.build/ja/concepts/why-astro/

---

## Astro について

- コンテンツ重視
  - 複雑な UI/UX を実現するための"アプリケーション"ではなく、"コンテンツ"を重視する web サイト向け
  - ブログ、ポートフォリオ、コーポレートサイト、LP, ...
- サーバーファースト
  - CSR ではなく SSR を優先、かつ SPA ではなく MPA
  - [View Transition API](https://docs.astro.build/ja/guides/view-transitions/) によって SPA っぽい挙動もできる（SPA モードと呼称）

---

## [アイランドアーキテクチャ](https://jasonformat.com/islands-architecture/)

Astro が採用する web アーキテクチャパターン

> 「Astro アイランド」とは、HTML の静的なページ上にあるインタラクティブな UI コンポーネントを指します。1 つのページに複数のアイランドが存在でき、アイランドは常に孤立して表示されます。静的で非インタラクティブな HTML の海に浮かぶ島（アイランド）とお考えください。

---

## アイランドアーキテクチャ

→ 静的な部分をサーバー側で事前に用意し、必要に応じてインタラクティブな UI を埋め込むことで、並列ロード（≒ パーシャルハイドレーション）が可能

→ コンポーネントの読み込みの優先度を設けて、優先度の高いものから表示させることができる

---

## Astro の Pros/Cons

前提

- 筆者は Vanilla JS や jQuery よりも、Vue.js や React に触れていた期間が長いためモダンな UI ライブラリ（以下 React を仮定）贔屓になってる気がします
- 一意見として捉えてください

---

## Astro の Pros/Cons

### Pros

- 特定のフレームワークに依存しないので、導入ハードルが比較的低い
- 素の マークアップ の感覚で書けるので、React に馴染みのない人でも取っ付きやすい
- メンテナンスとエコシステムの成長速度が早い
  - 2 週間おきのマイナーバージョンアップ
  - デフォルトのインテグレーションも充実しており自作もできる

---

## Astro の Pros/Cons

### Cons

- 逆に React に慣れてると色々と歯痒い点がある
- 他の UI ライブラリと共存させるにしても連携が取り辛い点がある

---

## 辛み：クライアントサイドでの処理

- jsx のようにイベントハンドラを直接記述できないため、都度 DOM の API を呼ぶ必要がある
  - `addEventListener()`, `querySelector()`...
- 状態管理の仕組みもデフォルトでは備わっていない
  - [公式が推奨している Nano Stores](https://docs.astro.build/ja/core-concepts/sharing-state/#why-nano-stores)という軽量な状態管理ライブラリがあるのでそれである程度カバーは可能

---

## 辛み：他の UI ライブラリとの共存

- コンポーネントの依存関係
  - `.astro`ファイルから`.jsx`や`.vue`のコンポーネントを呼ぶことはできるが、その逆は出来ない
  - 他の UI ライブラリのコンポーネントを使う場合はできる限りツリーの末端で使う考慮が必要
  - LP 案件では最終的に`.tsx`ファイルが 22、`.astro`ファイルが 3 個という内訳になった（）

---

## 辛み：他の UI ライブラリとの共存

- 画像の取り扱い
  - Astro には組み込みで画像最適化用の`<Image />`や`<Picture />`コンポーネントがあるが、他の UI ライブラリのコンポーネントではそれは当然使えない
  - Astro と React での画像の扱い方の差異の対処に結構苦労した

---

## EOF
