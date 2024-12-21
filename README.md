# Dockerize Laravel App

A simplified Docker Compose workflow that sets up a Laravel network of containers for local Laravel development with Adminer & PGAdmin.

## Usage

To get started, make sure you have [Docker installed](https://docs.docker.com/docker-for-mac/install/) on your system, and then clone this repo.

Next, navigate in your terminal to the directory you cloned this, and spin up the containers for the web server by running `docker compose up -d --build`.

After that completes, follow the steps from the [src/README.md](src/README.md) file to get your Laravel project added in (or create a new blank Laravel app).

**Note**: Your Postgres database host name should be `postgres`, **note** `localhost`. The username and database should both be `homestead` with a password of `secret`.

The following are built for our web server, with their exposed ports detailed:

-   **nginx** - `:80`
-   **postgres** - `:5432`
-   **php** - `:9000`
-   **redis** - `:6379`
-   **adminer** - `:8080`
-   **pgadmin** - `:8090`

Three additional containers are included that handle Composer, NPM, and Artisan commands _without_ having to have these platforms installed on your local computer. Use the following command examples from your project root, modifying them to fit your particular use case.

-   `docker compose run --rm --user $(1000 -u):$(1000 -g) composer install`
-   `docker compose run --rm --user $(1000 -u):$(1000 -g) npm run dev`
-   `docker compose run --rm --user $(1000 -u):$(1000 -g) artisan migrate`

## Makefile

There is a `makefile` which can help you to run every docker or artisan command easily. If you're not familiar with [GNU Makefile](https://www.gnu.org/software/make/manual/make.html) it's ok and you can still use this repository (even you can delete `makefile`), but with `makefile` you can manage different commands easier and better! Before using a `makefile` just install it from [GNU Makefile](https://www.gnu.org/software/make/manual/make.html) and run `make` command in repository root directory and you will see a help result to use it. some of `make` command example to simplify workflow:

```
# run docker compose up -d
make up

# run docker compose down --volumes
make down-volumes

# run migrations
make migrate

# run tinker
make tinker

# run artisan commands
make art db:seed
```

## Docker exec container

Access container as interactive shell and see output:

```
docker exec -it <container id> sh
```

Tip: You may use /bin/bash or just bash so after installing bash, you should inspect your image to understand CMD part and change current
option to whatever you want. For this purpose run:

```
docker inspect [imageID]
```

## Usage in Production

Tip: Don't forget to install and configure opcache

While I originally created this template for local development, it's robust enough to be used in basic Laravel application deployments. The biggest recommendation would be to ensure that HTTPS is enabled by making additions to the `nginx/default.conf` file and utilizing something like [Let's Encrypt](https://hub.docker.com/r/linuxserver/letsencrypt) to produce an SSL certificate.

## Compiling Assets

This configuration should be able to compile assets with both [laravel mix](https://laravel-mix.com/) and [vite](https://vitejs.dev/). In order to get started, you first need to add ` --host 0.0.0.0` after the end of your relevant dev command in `package.json`. So for example, with a Laravel project using Vite, you should see:

```json
"scripts": {
  "dev": "vite --host 0.0.0.0",
  "build": "vite build"
},
```

Then, run the following commands to install your dependencies and start the dev server:

-   `docker compose run --rm npm install`
-   `docker compose run --rm --service-ports npm run dev`

Want to build for production? Simply run `docker compose run --rm npm run build`.
