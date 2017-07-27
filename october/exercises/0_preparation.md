### Preparation

```
# install
composer create-project october/october myoctober dev-master
cd myoctober

echo 7.0.12 > .php-version

# using git to learn when and which files change as we do stuff
git init
add .
commit -m '[:baby:] Initial commit'
```

Now set up database config using a .env file. Create a file called `.env`. And add your database stuff to it like this:

```
DB_HOST=localhost
DB_DATABASE=myoctober_dev
DB_USERNAME=root
DB_PASSWORD=
DB_PORT=13306
```

Then change the appropriate DB driver in `config/database.php` so it looks something like this:

```
        'mysql' => [
            'driver'    => 'mysql',
            'host'      => env('DB_HOST', 'localhost'),
            'database'  => env('DB_DATABASE', 'database'),
            'username'  => env('DB_USERNAME', 'root'),
            'password'  => env('DB_PASSWORD', ''),
            'port'      => env('DB_PORT', ''),
            'charset'   => 'utf8',
            'collation' => 'utf8_unicode_ci',
            'prefix'    => '',
        ],
```

Then continue:

```
# save our changes
cp .env .env.example
git add .
git commit -m 'setting DB confs'
```

## Get the site running

Now push your repo to your remote, then set up a boxen project pointed at your project. Run boxen to get your project Nginx/PHP config in place. Then you should be able to see the site at:

myproject.dev

#### Quick/hackstart

Without using Boxen, you could use the embedded PHP server to access your DEV site, but the project won't be ready for other developers to pick it up. If you go this route, please set up a Boxen project at some point so other developers can pick up your work :bow:.

```
# make DB and tables and stuff
echo "CREATE DATABASE myoctober_dev DEFAULT CHARACTER SET utf8" | mysql -u root -p --port=13306
php artisan october:up

# Serve using the built-in server (Boxen is preferred)
php artisan serve
```

You could then check out the [frontend](http://localhost:8000/) and [backend](http://localhost:8000/backend) (creds are `admin`/`admin`).
