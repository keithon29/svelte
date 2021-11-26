# svelteをdockerで動かす

## ローカルでやること
ローカルで任意の名前のディレクトリを作っておく。
```sh
mkdir project-name
```

```Dockerfile```と```docker-compose.yml```を作る。

ディレクトリの階層構造はこのようにする。
```sh
.
├── Dockerfile
├── docker-compose.yml
└── project-name
```
### コンテナを立ち上げて入る
docker-compose便利...
```sh
docker-compose up -d --build
```
webのところは、```docker-compose.yml```で指定した、service名。
```sh
docker-compose exec web sh
```

## コンテナを立ち上げた後実行するコマンドたち

```sh
npx degit sveltejs/template project-name && cd project-name
```

### TypeScriptを使用するために
```sh
node scripts/setupTypeScript.js
```

### ローカルPCのlocalhostとdockerのlocalhostをつなげる
まず、ローカルPCのlocalhostとdockerのlocalhostをつなげるように```package.json```を書き換える。
```
sed -i s/--no-clear/build\ --host\ 0.0.0.0/ package.json
```
ローカルPC(Mac)から行う場合は以下のコマンドを実行する。
```sh
sed -i '' 's/--no-clear/build\ --host\ 0.0.0.0/' package.json
```
次に以下のコマンドを実行する。
これは```package.json```に書いてあるものを入れてくれる。
```sh
npm install
```
準備は整った。多分。
## svelte appを起動する
以下のコマンドを実行しよう
```sh
npm run dev
```

```sh
  Your application is ready~! 🚀

  - Local:      http://0.0.0.0:5000
```
こんなふうにきたら、合格かな。

```docker-compose.yml```で指定したport、
```sh
ports: 
  - "8080:5000"
```

今回は8080で```http://localhost:8080/```にChromeとかでアクセスすれば```HELLO WORLD!```
