# React(JS) 環境構築の手順

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

### 1.開発環境の構築

create-react-app コマンドを実行

```
npx create-react-app プロジェクト名
```

React v18 から v17 へ変更を実施

npm or yarn の 以下のコマンドを実行

```
npm uninstall(yarn remove) react react-dom
npminstall(yarn add) react@17.0.2 react-dom@17.0.2
```

index.js を v17 用の表示へ変更

```javascript:index.js
import React from "react";
import ReactDOM from "react-dom";
import "./index.css";
import App from "./App";
import reportWebVitals from "./reportWebVitals";

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById("root")
);
// If you want to start measuring performance in your app, pass a funcLon
// to log results (for example: reportWebVitals(console.log))
// or send to an analyLcs endpoint. Learn more: hPps://bit.ly/CRA-vitals
reportWebVitals();
```

動作確認をして、通常通り表示されれば OK

```
npm run start (yarn start)
```

### 2.ESLint / Prettier の設定

ESLint のインストールするためコマンドを実施

```
npm init @eslint/config
# or
yarn create @eslint/config
```

上記を実行すると対話式で設定を進められる

ESLint をどのように使いますか？

```
? How would you like to use Eslint? …
  To check syntax  only (構文のチェック)
  To check syntax and find problems (構文のチェックと問題の発見)
> To check syntax, find problems, and enforce code style(構文のチェックと問題の発見、コードの適用)
```

どのタイプのモジュールを使いますか？

```
? What type of modules does your project use? …
> JavaScript modules (import/export)
  CommonJS (require/exports)
  None of these
```

どのフレームワークを使いますか？

```
? Which framework does your project use? …
> React
  Vue.js
  None of these
```

プロジェクトで Typescript を使いますか？

```
? Does your project use TypeScript
> No
  Yes
```

コードはどこで実行しますか？

```
? Where does your code run? …
> Browser
  Node
```

プロジェクトに使うスタイルはどのように定義しますか？

```
? How would you like to define a style for your project? …
> Use a popular style guide (人気のスタイルガイド)
  Answer questions about your style (スタイルを答える)
  Inspect your JavaScript file(s) ( Javascriptファイルを調べる )
```

どのスタイルガイドを選択するか？

```
? Which style guide do you want to follow? …
❯ Airbnb: https://github.com/airbnb/javascript
  Standard: https://github.com/standard/standard
  Google: https://github.com/google/eslint-config-google
  XO: https://github.com/xojs/eslint-config-xo
```

ESLint の設定ファイルはどの形式が良いか？

```
? What format do you want your config file to be in? …
❯ JavaScript
  YAML
  JSON
```

これらのパッケージを npm で今インストールしますか？

```
? Would you like to install them now with npm? › No / Yes
```

インストールが問題なく完了すると eslintrc.js が作成されます。

追加で必要なパッケージをインストールします。

「eslint-plugin-unused-imports」は利用していない import を自動で削除するためにインストール

以下のコマンドを実行する

```
npm i --save-dev eslint-plugin-unused-imports
yarn add -D eslint-plugin-unused-imports
```

Prettier のインストール

以下のコマンドを実行して

```
npm i --save-dev prettier eslint-config-prettier
yarn add -D prettier eslint-config-prettier
```

インストールしたら、`.prettierrc.js` というファイルを作成する

```javascript:.prettierrc.js
module.exports = {
  printWidth: 120, //1行の文字列を120文字にする
  singleQuote: true, //ダブルに代わりシングルクオーテーションを使う
  semi: true, //行末にセミコロンを追加
  trailingComma: 'es5', //複数行の場合は可能な限り末尾のカンマを表示
  tabWidth: 2, //タグのスペース２ デフォルトは2
  useTabs: false, //スペースをタブに代える デフォルトはfalse
}
```

ESLint の詳細設定を記載する

```javascript:.eslintrc.js
module.exports = {
  env: {
browser: true,
    es2021: true,
  },
  extends: [
    //'plugin:react/recommended', airbnbにほど設定されているので削除可能
    "airbnb",
    "prettier"// ESLintの設定に従ってフォーマット extends内の最後の行に記載すること
  ],
  parserOptions: {
    ecmaFeatures: {
      jsx: true,
    },
    ecmaVersion: 12, //latestから12に変更
    sourceType: "module",
  },
  plugins: ["react",
            "unused-imports" //追加 使っていないimportを自動で削除用
           ],
  // 以下を追加
  rules: {
    "no-use-before-define": "off",
    "import/prefer-default-export": "off",
    "unused-imports/no-unused-imports": "error",
    "unused-imports/no-unused-vars": [
      "warn",
      {
        vars: "all",
        varsIgnorePattern: "^_",
        args: "after-used",
        argsIgnorePattern: "^_",
      },
    ],
    "import/extensions": [
      "error",
      {
        js: "never",
        jsx: "never",
        ts: "never",
        tsx: "never",
      },
    ],
    "react/jsx-filename-extension": [
      "error",
      {
        extensions: ["js", ".jsx"],
      },
    ],
    "react/react-in-jsx-scope": "off",
  },
  settings: {
    "import/resolver": {
      node: {
        paths: ["src"],
        extensions: [".js", ".jsx"],
      },
    },
  },
  // 追加はここまで
};
```

特定のフォルダ、ファイルに ESLint を適用させない

```.eslintignore
build/
public/
**/node_modules/
*.config.js
.*lintrc.js
/*.*
```

ここまででコマンド上の操作は完了

VSCode 上で ESLint と Prettier の拡張をインストールする

インストールしたら、ワークスペースで有効化する
有効化できたら、VSCode の設定がいるに以下を追加する。

`command + ,` で設定画面が開かれる

設定を開いたら、ワークスペースをクリックし、Vscode の設定を行う

```json:settings.json
{
  "editor.defaultFormatter": "esbenp.prettier-vscode", //デフォルトのフォーマットはprettierを指定
  "editor.formatOnSave": true, //保存したときにフォーマットする
  "editor.codeActionsOnSave": { //ESLintに対応しているファイルはESLintでフォーマット
    "source.fixAll.eslint": true
  },
}
```

ファイルへ書き込みができたら、一度 `npm run start / yarn start` で React プロジェクトを開けるかを確認。  
Linter 側で引っかかるエラーがあれば、エラーを調べて対応します。

![fd82208e11e14a98214bfd491a64479e](https://user-images.githubusercontent.com/76837734/179395789-dfd58cca-c94b-4475-9930-82c948720512.png)

↑ が表示されたら、準備完了です！
