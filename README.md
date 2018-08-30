開発:

```
# 初回
docker-compose build
docker-compose run --rm --user appuser app composer install
docker-compose run --rm --user node node yarn install
docker-compose run --rm --user node node yarn dev

# アプリケーション立ち上げ
docker-compose up
```

本番用のコンテナ作成&コンテナレジストリへのpush:

```
docker-compose -f docker-compose.prod.yml build
docker-compose -f docker-compose.prod.yml push
```
