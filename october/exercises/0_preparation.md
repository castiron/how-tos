### Preparation

```

# install
composer create-project october/october myoctober dev-master
cd myoctober

# using git to learn when and which files change as we do stuff
git init
add .
commit -m 'firsties'
```

Now this part gets weird. October diverges some of the basic configuration setup that Laravel uses. Let's keep doing things the laravel way, for now. Create a file called `.env`. And add your database stuff to it like this:

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
add .
commit -m 'setting DB confs'

# make DB and tables and stuff
echo "CREATE DATABASE myoctober_dev" | mysql -u root -p --port=13306
php artisan october:up

# boom
php artisan serve
```

Now check out the [frontend](http://localhost:8000/) and [backend](http://localhost:8000/backend) (creds are `admin`/`admin`).
