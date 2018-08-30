開発:

```
docker-compose build
docker-compose run --rm --user appuser app composer install
docker-compose run --rm --user node node yarn install
docker-compose run --rm --user node node yarn dev

docker-compose up
```

本番用のコンテナ作成&コンテナレジストリへのpush:

```
docker-compose -f docker-compose.prod.yml build
docker-compose -f docker-compose.prod.yml push
```
