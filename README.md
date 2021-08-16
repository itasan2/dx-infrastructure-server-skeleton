# Laravel 向け Docker 環境

# ローカル環境で開発環境用に起動する

```
docker-compose -f docker-compose-develop.yml --env-file=.env.develop up
```

http://localhost にアクセスしLaravelのTOPページが表示されればOK

ECSで起動する場合、上記 `docker-compose` で作成されたimageをECRへPushし、タスク定義で指定する
この時 .env.develop で指定されている環境変数やポートマッピングはタスク定義に定義する必要がある