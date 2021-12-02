# SvelteをDockerで動かす
Svelteは優秀なwebフレームワークです(主観)。
[https://svelte.jp/:embed:cite]

今回はDockerを使うのですが、ローカルを汚したくない人がいるみたいなので、その人向けです。

## ローカルでやること
ローカルで任意の名前のディレクトリを作っておきます。
```sh
mkdir project
```

```Dockerfile```と```docker-compose.yml```を作ります。

### Dockerfile
```Dockerfile
FROM node:lts-alpine

RUN apk add git

RUN mkdir project

WORKDIR /project
```
### docker-compose.yml
```yml
version: "3"
services: 
  web:
    build: .
    tty: true
    volumes: 
      - ./project/:/project/
    ports: 
      - "8080:5000"
```

ディレクトリの階層構造はこのようにします。
```sh
.
├── Dockerfile
├── docker-compose.yml
└── project
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

## コンテナ内で実行するコマンドたち
deditでtemplateを持ってきます。
```sh
npx degit sveltejs/template project && cd project
```

### TypeScriptを使用するために
```sh
node scripts/setupTypeScript.js
```

### ローカルPCのlocalhostとdockerのlocalhostをつなげる(って理解であってるかな)
まず、ローカルPCのlocalhostとdockerのlocalhostをつなげるように、以下のコマンドを実行して```package.json```を書き換えます。
```sh
sed -i s/--no-clear/build\ --host\ 0.0.0.0/ package.json
```
ローカルPC(Mac)でコンテナ外から行う場合は以下のコマンドを実行します。
```sh
sed -i '' 's/--no-clear/build\ --host\ 0.0.0.0/' package.json
```
次に以下のコマンドを実行します。
これは```package.json```に書いてあるものを入れてくれます。
```sh
npm install
```
準備は整いました。多分。
## svelte appを起動する
以下のコマンドを実行しよう。
```sh
npm run dev
```

こんなふうにきたら、おめでとうございます。
```sh
  Your application is ready~! 🚀

  - Local:      http://0.0.0.0:5000
```

```docker-compose.yml```で指定したport、

```sh
ports: 
  - "8080:5000"
```

今回は```http://localhost:8080/```にChromeとかでアクセスすれば```HELLO WORLD!```
