# LEMP + Composer + Node.js + PHPMyAdminの環境作成

## 作った理由
laradockを使ってたが、無駄なコンテナ作りすぎでは?っと思ったため(素人目線)<br>
あと、手軽な学習環境が欲しい<br>
(laravel/uiでvueの導入まで行った↓)

```
composer require laravel/ui:^1.0 --dev
php artisan ui vue --auth
npm install && npm run dev
```

## 参考
[docker-composeでLaravelの開発環境を整える方法とその解説](https://www.membersedge.co.jp/blog/laravel-development-environment-with-docker-compose/)を基にプロジェクトの構成などを少しいじった。


## 変更点
- Laravelのプロジェクトを外側に出した。
- php.iniファイルの読み込み
- dockerを削除して、php, webの階層を変更、dbを追加(Laravel使うならマイグレーションファイルあるからdbいらないかも)
- 学習用なので、PHPMyAdminを追加
- vueを使いたいのでnode.js(v12)を追加

## 開始方法
- リポジトリをクローン

- `cd docker`

- `docker-compose up -d`

- `docker-compose exec app bash`

- `composer create-project --prefer-dist laravel/laravel=6.x[or 7.x] プロジェクト名`

- `root /var/www/html/[laravel-sample]/public;`の[]をLarevel生成時につけたプロジェクト名に変更

- `docker-compose down`で一度閉じる。

- `docker-compose up -d`した後 http://localhost:8000 にアクセス

- データベースの設定を書き換え

## port
※`docker-compose ps`でも調べられる。<br>
http://localhost:8000 -> laravelのページ<br>
http://localhost:8080 -> phpMyAdmin<br>

## DB設定
```env
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=sample
DB_USERNAME=user
DB_PASSWORD=password
```

## Laravel config設定
#### config/app.phpの設定変更

- 70行目: `'timezone' => 'Asia/Tokyo',`
- 83行目: `'locale' => 'ja',`

### config/database.phpの設定変更

絵文字使わないのでマルチバイトでなくて良い(mb4を消す)

- 55行目:`'charset' => 'utf8',`
- 56行目:`'collation' => 'utf8_unicode_ci',`

## 注意点
- Laravel6.xでは、`composer require laravel/ui:^1.0 --dev`を導入する。
  (2.0はlaravel7用)

## 変更点
- 2020/05/16<br>
  1. vueを使用するためにnode.jsを追加
  1. nginxにもlaravelプロジェクトをマウントしないとcssやjsが適用されないので修正
    ```docker
        web:
        image: nginx:1.15.6
        ports:
        - '8000:80'
        depends_on:
        - app
        volumes:
        - ./web/default.conf:/etc/nginx/conf.d/default.conf
        - ./../src:/var/www/html # ココ
    ```
