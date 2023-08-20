---
marp: true
theme: gaia
style: |
  section h1, section h2, section h3 {
    font-weight: 500;
  }
  section.center {
      display: flex;
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

# npm/yarn/pnpm ゆるふわ解説

---

<!--
class: invert
-->

## Goal

- Node のパッケージマネージャー（PM）の仕様差異をふんわり理解する
- 依存関係の仕組みについてやんわり理解する

---

## 各 PM 詳細

- npm
- yarn(v1, v2~)
- pnpm

---

## npm

- 言わずと知れた PM
- 2023/08/20 現在の latest version は v9.8.0
- 2020 年に Github が買収し、現在は MS 管理下にある
- 「npm is not an acronym」を再帰的に表したものであり、「Node Package Manager」の頭文字を表しているものではないらしい
  - https://github.com/npm/cli#faq-on-branding

---

## Yarn(v1, classic)

- 2016 年に当時の Facebook と Google を中心に開発
- 下記の特徴が革新的だった
  - 依存関係の木構造が普遍
  - ロックファイルによるバージョン固定
- 2020 年に本バージョン(v1)はメンテナンスモード対象となった
  - https://classic.yarnpkg.com/en/docs/install#mac-stable
  - 2023/08/20 現在の latest version は v3.6.2

---

## pnpm

- 2017 年に OSS として release
- pnpm = performant npm
- 2023/08/20 現在の latest version は v8.6.12
- ディスク使用量の効率化が主眼（後述）
  - hosting の代わりに symlink を活用

---

## Yarn (v2, Berry)

- 2020 年に yarn v2 が release
- Plug’n’Play (PnP)の導入（後述）
  - node_modules を作らず、代わりに依存関係のマッピング用のファイルを使用する

---

## 比較観点

- パフォーマンス
- ディスク効率性
- セキュリティ

---

## node_modules のディレクトリ構造

### 前提

- install 実行で node_modules/配下に依存パッケージが配置される
- Node はパッケージの探索時、パッケージが見つかるまで親ディレクトリの node_modules を再帰的に探しに行く

---

## npm

- 依存関係は木構造になっている
- 依存関係内の重複パッケージは hoisting（巻き上げ）によって flat に配置される（なるべく階層が浅くなるように保つ）
  - 重複パッケージの削除 = dedupe
- 同名パッケージでも異なるバージョンの場合は hoisting が出来ない

---

<!--
class: center invert
-->

<div style="display: flex; width: 100%; justify-content: space-between; gap: 20px; align-items: start;">
<div style="flex-direction: column; width: 50%;">
before

```
node_modules/
　 ├ A(v1.0.0)
　 │ └ node_modules/
　 │ 　 └ B(v1.0.0)
　 ├ C(v1.0.0)
　 │ └ node_modules/
　 │ 　 └ A(v1.0.0)
　 │ 　 　 └ node_modules/
　 │ 　 　 　 └ B(v1.0.0)
　 └ D(v1.0.0)
　 　 └ node_modules/
　 　 　 └ B(v2.0.0)
```

</div>

<div style="flex-direction: column;  width: 50%">

after

```
node_modules/
　 ├ A(v1.0.0)
　 ├ B(v1.0.0)
　 ├ C(v1.0.0)
　 └ D(v1.0.0)
　 　 └ node_modules/
　 　 　 └ B(v2.0.0)
```

</div>
</div>

---

<!--
class: invert
-->

## yarn classic

// TBD

---

## pnpm

- hositing の代わりに symbolic link を活用
  - install したパッケージは home の.pnpm_store 内に配置される
  - 木構造内の重複パッケージは symlink で使い回す

```
node_modules
     └── .pnpm
         ├── bar@1.0.0
         │   └── node_modules
         │       └── bar -> <store>/bar
         └── foo@1.0.0
             └── node_modules
                 ├── foo -> <store>/foo
                 └── bar -> ../../bar@1.0.0/node_modules/bar
```

---

## yarn berry

- Plug'n'Play(PnP)というインストール戦略を採用（node_modules と対比）
  - node_modules を作らず、代わりにモジュールパスのマッピング用の pnp.cjs を作成
  - 各パッケージを zip 化しプロジェクトの`.yarn/cache/`配下に配置
- 従来通り node_modules 管理にすることも可能
  - ref: https://sg.wantedly.com/companies/wantedly/post_articles/325643
