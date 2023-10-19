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

※今回の内容は個人の見解を多分に含んでいるので一意見として捉えていただけると幸いです m

---

<!--
class: invert
-->

## 背景

- 案件で Vue2 & Nuxt の LP を Astro（&React）にリプレイスしたので Astro を触ってみた所感について話します
- LP 自体は複雑なマークアップはない
- アニメーションとか動きが結構多め

---

## Astro について

> Astro は、コンテンツにフォーカスした高速な Web サイトを構築するためのオールインワン Web フレームワークです。

> 1.  コンテンツ重視
> 2.  サーバーファースト
> 3.  デフォルトで高速
> 4.  簡単に使える
> 5.  充実した機能と柔軟性

https://docs.astro.build/ja/concepts/why-astro/

---

- pros
  - 特定のフレームワークに依存しないので、乗り換えハードルが比較的低い
  - 素の HTML の感覚で記述できるので、モダンな UI ライブラリ（Vue, React 等）に馴染みのない人でも理解しやすい
- cons
  - 逆にモダンフロントエンドに慣れてると色々辛い、物足りない
  - 他の UI ライブラリ（Vue,React 等）と共存させるにしても連携が取り辛い

---

## 辛み：DOM 操作

- jsx のようにイベントハンドラを直接記述できず、毎回`addEventListener()`しなければならない
- 状態管理の仕組みもデフォルトでは備わっていない
  - [公式が推奨している Nano Stores](https://docs.astro.build/ja/core-concepts/sharing-state/#why-nano-stores)という軽量な状態管理ライブラリがあるのでそれである程度補完は可能

---

## 辛み：他の UI ライブラリとの共存

- コンポーネントの依存関係
  - `.astro`ファイルから`.jsx`や`.vue`のコンポーネントを呼ぶことはできるが、その逆は出来ない
  - 他の UI ライブラリのコンポーネントを使う場合はできる限り末端で使う考慮が必要
- 画像の最適化
  - Astro には組み込みの`<Image />`コンポーネントがあるが、他の UI ライブラリのコンポーネントではそれは当然使えない
  - Astro と React での画像の扱い方の差異の対処に結構苦労した

---

## 辛み：他の UI ライブラリとの共存

- ***

- 向き
  - pre-render で事足りる系
    - 動きの少ないブログ、ポートフォリオなど
- 不向き
  - がっつり DOM 操作する系
    - 動きの多い LP
  - web サービス

---

## 結論

- モダンフロントエンドに馴染みがない人でもとっつきやすい
  - 馴染んでいる人にはちょっと物足りない
- クライアントサイドで複雑な DOM 操作をしようとすると他の UI ライブラリ（とそのエコシステム）に頼りたくなる
-
