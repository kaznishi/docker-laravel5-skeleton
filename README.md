開発:

```
docker-compose build
docker-compose run --rm --user appuser app composer install
docker-compose run --rm --user node node yarn install
docker-compose run --rm --user node node yarn dev

docker-compose up
```
