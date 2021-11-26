# svelteをdockerで動かす

ローカルで任意の名前のディレクトリを作っておく。
```sh
mkdir project-name
```

コンテナを立ち上げた後

```sh
npx degit sveltejs/template project-name && cd project-name
```

# TypeScriptを使用する
```sh
node scripts/setupTypeScript.js
```

# ローカルPCのlocalhostとdockerのlocalhostをつなげる
まず、ローカルPCのlocalhostとdockerのlocalhostをつなげるように```package.json```を書き換える。
```
sed -i s/--no-clear/build\ --host\ 0.0.0.0/ package.json
```
ローカルPC(Mac)から行う場合は以下のコマンドを実行する。
```sh
sed -i '' 's/--no-clear/build\ --host\ 0.0.0.0/' package.json
```
次に以下のコマンドを実行する。
```sh
npm install
```

```sh
npm run dev
```
