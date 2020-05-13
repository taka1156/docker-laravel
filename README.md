# LEMP + Composer + PHPMyAdminの環境作成

## 作った理由
laradockを使ってたが、無駄なコンテナ作りすぎでは?っと思ったため(素人目線)<br>
あと、手軽な学習環境が欲しい

## 参考
[docker-composeでLaravelの開発環境を整える方法とその解説](https://www.membersedge.co.jp/blog/laravel-development-environment-with-docker-compose/)を基にプロジェクトの構成などを少しいじった。

## 変更点
- Laravelのプロジェクトを外側に出した。
- php.iniファイルの読み込み
- dockerを削除して、php, webの階層を変更、dbを追加(Laravel使うならマイグレーションファイルあるからdbいらないかも)
- 学習用なので、PHPMyAdminを追加

## 開始方法
- リポジトリをクローン

- srcを空にする

- `cd laravel_docker`

- `docker-compose up -d`

- `docker-compose exec app bash`

- `composer create-project --prefer-dist laravel/laravel=6.x[or 7.x] プロジェクト名`

- `root /var/www/html/[laravel-sample]/public;`の[]をLarevel生成時につけたプロジェクト名に変更

- `docker-compose down`で一度閉じる。

- `docker-compose up -d`した後 http://localhost:8000 にアクセス

- データベースの設定を書き換え

## port
※`docker-compose ps`でも調べられる。
localhost:8000 -> laravelのページ
localhost:8080 -> phpMyAdmin

## DB設定
```env
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=sample
DB_USERNAME=user
DB_PASSWORD=password
```

## 反省点
- neginx周りをもう少し勉強
- docker周りをカンで触ってることが多いので何かしらにまとめる。
