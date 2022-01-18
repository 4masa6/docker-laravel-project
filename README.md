## 概要

Docker環境構築
PHP8・Laravel8・Nginx・MySQL
- Laravel8
- DB接続
- LaravelBreeze
- 言語設定
- Debugbar
- 日本語化

## 手順
### プロジェクトの立ち上げ

```bash
# プロジェクトをダウンロード
git pull https://github.com/4masa6/docker-laravel-project.git

cd docker-laravel-project

# /.env作成
cp .env.example .env
# === /.envの編集 ===
# 後述の/src/.env の設定と合わせる
PMA_ARBITRARY=1
PMA_HOST=db
PMA_USER=root
PMA_PASSWORD=password
# ==================

# ビルド
docker-compose build --no-cache

# コンテナ起動
docker-compose up -d

# コンテナに入る
docker-compose exec php /bin/bash


# srcディレクトリに移動し、.envファイルを作成
cd src
cp .env.example .env
# composerインストール
composer install
# APP_KEY作成
php artisan key:generate
```

URL
`http://127.0.0.1:8000/`

### DBとの接続

- /src/.envファイルを修正
```plain:/src/.env
# 前述の/src/.env の設定と合わせる
DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=<データベース名（snake_case）>
DB_USERNAME=root
DB_PASSWORD=password
```

- phpMyAdminに接続 `http://127.0.0.1:8086/`
- 上部「DB」タブより、下記SQLを実行
```sql
CREATE DATABASE <データベース名（snake_case）> DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci
```

- phpコンテナに入り、マイグレーションを実行
```bash
# コンテナに入っていなければ
docker-compose exec php /bin/bash
cd src
# マイグレーション実行
php artisan migrate
```