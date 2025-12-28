# Ruby Getting Started

MySQL データベースを使用したシンプルな [Ruby on Rails](https://rubyonrails.org/) アプリケーションです。Docker でローカル実行、または Heroku にデプロイできます。

## 技術スタック

- Ruby 3.4
- Rails 8.0.3
- MySQL 8.0
- Docker & Docker Compose
- Datadog APM & Monitoring

このアプリケーションは、Heroku プラットフォームの [Cedar および Fir 世代](https://devcenter.heroku.com/articles/generations) の両方のチュートリアルをサポートしています。以下を参照してください:

- [Getting Started on Heroku with Ruby](https://devcenter.heroku.com/articles/getting-started-with-ruby)
- [Getting Started on Heroku Fir with Ruby](https://devcenter.heroku.com/articles/getting-started-with-ruby-fir)

## ローカルでの実行

### Docker を使用する場合（推奨）

このセットアップでは、Docker Compose を使用して Rails と MySQL 8.0 を実行します。

前提条件:
- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

手順:

1. リポジトリをクローンします:
```bash
$ git clone https://github.com/heroku/ruby-getting-started
$ cd ruby-getting-started
```

2. 環境変数をセットアップします:
```bash
$ cp .env.sample .env
```

`.env` を編集して Datadog API キーを設定します:
```
DD_API_KEY=your_datadog_api_key_here
```

Datadog API キーの取得方法:
- [Datadog](https://app.datadoghq.com/) にログイン
- Organization Settings → API Keys に移動
- 既存のキーをコピーするか、新しいキーを作成

3. コンテナを起動します:
```bash
$ docker compose up --build
```

4. 別のターミナルで、データベースを作成してマイグレーションを実行します:
```bash
$ docker compose exec app bundle exec rake db:create db:migrate
```

アプリケーションが [localhost:3000](http://localhost:3000/) で起動します。

Datadog エージェントが自動的にメトリクス、トレース、ログの収集を開始します。

#### 便利な Docker コマンド

コンテナを停止:
```bash
$ docker compose down
```

コンテナを停止してボリュームを削除（すべてのデータベースデータを削除）:
```bash
$ docker compose down -v
```

ログを表示:
```bash
$ docker compose logs app
$ docker compose logs db
$ docker compose logs datadog
```

Rails コンソールにアクセス:
```bash
$ docker compose exec app bundle exec rails console
```

Datadog エージェントのステータスを確認:
```bash
$ docker compose exec datadog agent status
```

### Docker を使用しない場合

前提条件:
- [Ruby 3.4+](https://guides.railsgirls.com/install)
- [MySQL 8.0](https://dev.mysql.com/downloads/mysql/)
- [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli)

手順:

1. リポジトリをクローンします:
```bash
$ git clone https://github.com/heroku/ruby-getting-started
$ cd ruby-getting-started
```

2. 依存関係をインストールします:
```bash
$ bundle install
```

3. データベース接続を設定します（環境変数を設定）:
```bash
$ export DB_HOST=localhost
$ export DB_USERNAME=root
$ export DB_PASSWORD=your_mysql_password
```

4. データベースを作成してマイグレーションを実行します:
```bash
$ bundle exec rake db:create db:migrate
```

5. サーバーを起動します:
```bash
$ heroku local
```

アプリケーションが [localhost:5006](http://localhost:5006/) で起動します。

## Datadog 連携

このアプリケーションには、APM（Application Performance Monitoring）、メトリクス収集、分散トレーシングのための Datadog が含まれています。

### 機能

- **APM & 分散トレーシング**: アプリケーションパフォーマンスの監視とサービス間のリクエストトレース
- **インフラストラクチャ監視**: Docker コンテナのメトリクス、CPU、メモリ使用量の追跡
- **ログ管理**: すべてのコンテナからの集中ログ管理
- **カスタムメトリクス**: DogStatsD を使用したカスタムメトリクスの送信

### 設定

Datadog エージェントは `compose.yaml` の環境変数で設定されています:

- `DD_API_KEY`: Datadog API キー（`.env` ファイルで設定）
- `DD_SITE`: Datadog サイト（アジア太平洋リージョンは `ap1.datadoghq.com`）
- `DD_APM_ENABLED`: APM と分散トレーシングを有効化
- `DD_DOGSTATSD_NON_LOCAL_TRAFFIC`: 他のコンテナからのメトリクス受信を許可
- `DD_APM_NON_LOCAL_TRAFFIC`: 他のコンテナからのトレース受信を許可

### Datadog でのデータ表示

アプリケーション起動後、Datadog ダッシュボードでメトリクスとトレースを確認できます:

1. [Datadog](https://app.datadoghq.com/) にログイン
2. 以下に移動:
   - **APM → Services**: アプリケーショントレース
   - **Infrastructure → Containers**: Docker メトリクス
   - **Logs**: 集中ログ

### セキュリティに関する注意

⚠️ **重要**: Datadog API キーをバージョン管理にコミットしないでください。

- `.env` ファイルは `.gitignore` に含まれています
- 必要な環境変数のテンプレートとして `.env.sample` を使用してください
- 機密情報には必ず環境変数を使用してください

## Heroku へのデプロイ

このサンプルアプリのリソース使用は利用量にカウントされます。実験が終わったらすぐに[アプリを削除](https://devcenter.heroku.com/articles/heroku-cli-commands#heroku-apps-destroy)し、[データベースを削除](https://devcenter.heroku.com/articles/heroku-postgresql#removing-the-add-on)してコストを管理してください。

正しいディレクトリにいることを確認してください:

```console
$ ls -1
app
app.json
bin
config
config.ru
db
Gemfile
Gemfile.lock
lib
log
Procfile
public
Rakefile
README.md
test
tmp
vendor
```

### Heroku [Cedar](https://devcenter.heroku.com/articles/generations#cedar) へのデプロイ

デフォルトでは、Eco プランに登録している場合は Eco dyno を使用します。そうでない場合は、Basic dyno がデフォルトになります。Eco dyno プランはアカウント内のすべての Eco dyno で共有され、多くの小規模アプリを Heroku にデプロイする予定がある場合に推奨されます。低コストプランの詳細については[こちら](https://blog.heroku.com/new-low-cost-plans)をご覧ください。

対象となる学生は、新しい [Heroku for GitHub Students プログラム](https://blog.heroku.com/github-student-developer-program)を通じてプラットフォームクレジットを申請できます。

```text
$ heroku create
$ git push heroku main
$ heroku open
```

### Heroku [Fir](https://devcenter.heroku.com/articles/generations#fir) へのデプロイ

デフォルトでは、[Fir](https://devcenter.heroku.com/articles/generations#fir) 上のアプリは 1X-Classic dyno を使用します。[Fir](https://devcenter.heroku.com/articles/generations#fir) にアプリを作成するには、まず[プライベートスペースを作成](https://devcenter.heroku.com/articles/working-with-private-spaces#create-a-private-space)する必要があります。

```text
$ heroku create --space <space-name>
$ git push heroku main
$ heroku ps:wait
$ heroku open
```

## ドキュメント

Heroku での Ruby の使用に関する詳細情報については、以下の Dev Center の記事を参照してください:

- [Getting Started on Heroku with Ruby](https://devcenter.heroku.com/articles/getting-started-with-ruby)
- [Getting Started on Heroku Fir with Ruby](https://devcenter.heroku.com/articles/getting-started-with-ruby-fir)
- [Ruby articles on the Heroku Dev Center](https://devcenter.heroku.com/categories/ruby)
