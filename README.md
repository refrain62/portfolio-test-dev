# React コードを隠したまま GitHub Pages でサイトを公開する の写経
https://zenn.dev/numagotatu/articles/2024-01-07-deploy-github-pages


## 先にプロジェクトフォルダ作成
```
protfolio-test-dev
protfolio-test-prod
```

## 開発側にプロジェクト作成
```
$ cd protprotfolio-test-devo
$ npm create vite@latest . -- --template react-swc-ts
```

## nodeのバージョン固定
github actions の workflow を使う場合は、package.jsonから node バージョンを参照するため、volta pinコマンドを実施しておく必要がある
```
$ volta pin node@20.10.0
```

# ルートのパスについて設定ファイルの修正
vite.config.ts
```
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react-swc'

// https://vitejs.dev/config/
export default defineConfig({
 plugins: [react()],
 base: '/portfolio-prod/'
})
```

ここまでで dev側の準備完了

## Github actionの設定
.github/workflows ディレクトリは以下に配置
```
mkdir -p .github/workflows
cd ./github/workflows
touch deploy-prod.yaml
```

## Personal Access Tokenの作成とRepository secretの設定
- Personal Access Token(classic)をrepo権限で作成。トークンの値は控えておきましょう。Personal Access Token の作成方法がわからないという方はこちらの記事のトークンの発行を参考にして下さい。

- 開発用リポジトリのSettingsタブ -> Secrets and variables -> Actions -> Repository secrets -> New repository secret にPERSONAL_ACCESS_TOKENの名前で作成。1.で作成した Personal Access Tokenの値を入れて保存します。



# React + TypeScript + Vite

This template provides a minimal setup to get React working in Vite with HMR and some ESLint rules.

Currently, two official plugins are available:

- [@vitejs/plugin-react](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react/README.md) uses [Babel](https://babeljs.io/) for Fast Refresh
- [@vitejs/plugin-react-swc](https://github.com/vitejs/vite-plugin-react-swc) uses [SWC](https://swc.rs/) for Fast Refresh

## Expanding the ESLint configuration

If you are developing a production application, we recommend updating the configuration to enable type aware lint rules:

- Configure the top-level `parserOptions` property like this:

```js
export default {
  // other rules...
  parserOptions: {
    ecmaVersion: 'latest',
    sourceType: 'module',
    project: ['./tsconfig.json', './tsconfig.node.json'],
    tsconfigRootDir: __dirname,
  },
}
```

- Replace `plugin:@typescript-eslint/recommended` to `plugin:@typescript-eslint/recommended-type-checked` or `plugin:@typescript-eslint/strict-type-checked`
- Optionally add `plugin:@typescript-eslint/stylistic-type-checked`
- Install [eslint-plugin-react](https://github.com/jsx-eslint/eslint-plugin-react) and add `plugin:react/recommended` & `plugin:react/jsx-runtime` to the `extends` list
