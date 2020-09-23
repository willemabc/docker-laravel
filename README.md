# docker-laravel

My recipe for a Docker workflow that sets up a LEMP network of containers for local Laravel development. It is heavily inspired by Andrew Schmelyun's article [The beauty of Docker for local Laravel development](https://dev.to/aschmelyun/the-beauty-of-docker-for-local-laravel-development-13c0) and his [aschmelyun/docker-compose-laravel](https://github.com/aschmelyun/docker-compose-laravel) repository.

After cloning this repository, navigate to the directory you cloned this to and run:

```
docker-compose up -d --build lnginx
```

Next, we'll grab a fresh copy of Laravel:

```
docker-compose run --rm lcomposer create-project laravel/laravel tmp
```

Laravel was created in the *tmp* directory. Remove the existing *src* directory and rename the *tmp* directory to *src*.

We need to make a few changes in Laravel's *.env* file:

```
DB_CONNECTION=mysql
DB_HOST=lmysql
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=laravel
DB_PASSWORD=laravel
```

Now we basically just have to finish the Laravel installation: update the Composer packages, install the Node/NPM packages and run the database migrations. This is done a little bit different than what you are used to:

```
// composer
docker-compose run --rm lcomposer update

// node/npm
docker-compose run --rm lnpm install
docker-compose run --rm lnpm run dev
```

Finally... access your brand new Laravel website in your browser at http://localhost!