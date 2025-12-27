# Ruby Getting Started

A barebones [Ruby on Rails](https://rubyonrails.org/) app with MySQL database, which can be run locally with Docker or deployed to Heroku.

## Technology Stack

- Ruby 3.4
- Rails 8.0.3
- MySQL 8.0
- Docker & Docker Compose

This application supports the tutorials for both the [Cedar and Fir generations](https://devcenter.heroku.com/articles/generations) of the Heroku platform. You can check them out here:

- [Getting Started on Heroku with Ruby](https://devcenter.heroku.com/articles/getting-started-with-ruby)
- [Getting Started on Heroku Fir with Ruby](https://devcenter.heroku.com/articles/getting-started-with-ruby-fir)

## Running Locally

### With Docker (Recommended)

This setup uses Docker Compose to run Rails with MySQL 8.0.

Prerequisites:
- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

Steps:

1. Clone the repository:
```bash
$ git clone https://github.com/heroku/ruby-getting-started
$ cd ruby-getting-started
```

2. Start the containers:
```bash
$ docker compose up --build
```

3. In another terminal, create and migrate the database:
```bash
$ docker compose exec app bundle exec rake db:create db:migrate
```

Your app should now be running on [localhost:3000](http://localhost:3000/).

#### Useful Docker Commands

Stop the containers:
```bash
$ docker compose down
```

Stop and remove volumes (removes all database data):
```bash
$ docker compose down -v
```

View logs:
```bash
$ docker compose logs app
$ docker compose logs db
```

Access Rails console:
```bash
$ docker compose exec app bundle exec rails console
```

### Without Docker

Prerequisites:
- [Ruby 3.4+](https://guides.railsgirls.com/install)
- [MySQL 8.0](https://dev.mysql.com/downloads/mysql/)
- [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli)

Steps:

1. Clone the repository:
```bash
$ git clone https://github.com/heroku/ruby-getting-started
$ cd ruby-getting-started
```

2. Install dependencies:
```bash
$ bundle install
```

3. Configure database connection (set environment variables):
```bash
$ export DB_HOST=localhost
$ export DB_USERNAME=root
$ export DB_PASSWORD=your_mysql_password
```

4. Create and migrate the database:
```bash
$ bundle exec rake db:create db:migrate
```

5. Start the server:
```bash
$ heroku local
```

Your app should now be running on [localhost:5006](http://localhost:5006/).

## Deploying to Heroku

Using resources for this example app counts towards your usage. [Delete your app](https://devcenter.heroku.com/articles/heroku-cli-commands#heroku-apps-destroy) and [database](https://devcenter.heroku.com/articles/heroku-postgresql#removing-the-add-on) as soon as you are done experimenting to control costs.

Ensure you're in the correct directory:

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

### Deploy on Heroku [Cedar](https://devcenter.heroku.com/articles/generations#cedar)

By default, apps use Eco dynos if you are subscribed to Eco. Otherwise, it defaults to Basic dynos. The Eco dynos plan is shared across all Eco dynos in your account and is recommended if you plan on deploying many small apps to Heroku. Learn more about our low-cost plans [here](https://blog.heroku.com/new-low-cost-plans).

Eligible students can apply for platform credits through our new [Heroku for GitHub Students program](https://blog.heroku.com/github-student-developer-program).

```text
$ heroku create
$ git push heroku main
$ heroku open
```

### Deploy on Heroku [Fir](https://devcenter.heroku.com/articles/generations#fir)

By default, apps on [Fir](https://devcenter.heroku.com/articles/generations#fir) use 1X-Classic dynos. To create an app on [Fir](https://devcenter.heroku.com/articles/generations#fir) you'll need to
[create a private space](https://devcenter.heroku.com/articles/working-with-private-spaces#create-a-private-space)
first.

```text
$ heroku create --space <space-name>
$ git push heroku main
$ heroku ps:wait
$ heroku open
```

## Documentation

For more information about using Ruby on Heroku, see these Dev Center articles:

- [Getting Started on Heroku with Ruby](https://devcenter.heroku.com/articles/getting-started-with-ruby)
- [Getting Started on Heroku Fir with Ruby](https://devcenter.heroku.com/articles/getting-started-with-ruby-fir)
- [Ruby articles on the Heroku Dev Center](https://devcenter.heroku.com/categories/ruby)
