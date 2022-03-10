# Redwood

### CLI: Planetscale and MySQL
Follow this guide:
- https://docs.planetscale.com/concepts/planetscale-environment-setup#macos-instructions

Be sure to login via `pscale auth login`

> **Note**
> We will use MySQL for local testing (optional), in which case we recommend installing mysql, which includes the mysql-client. For example on MacOS, to install mysql run `brew install mysql`
>
> Brew creates a `root` user (no password) and defaults to localhost. We'll use those for local dev and config. E.g. access msql shell via `msql -uroot`
## Setup
### Installation and Config
This project uses Yarn 3, dbAuth, and SQLite. You need to:
1. install packages
2. apply migrations to the dev DB
3. create a local dbAuth secret
4. create a local MySQL test DB (optional)
    - the test runner does not need a hosted DB; just use MySQL locally


Follow these steps:

```terminal
yarn install
echo "SESSION_SECRET=$(yarn --silent rw g secret --raw)" >> .env
mysql -uroot
mysql> create database planetscale_test;
```

>  **Note**
> The Test DB connection string for DB `planetscale_test` is already set in .env.defauls `TEST_DATABASE_URL`

### Planetscale DB Setup
For this example, we are using Planetscale for local dev as well as deployment. We are also using Prisma Migrate, which requires a Shadow DB branch. Reference:
- [Planetscale Prisma quick start](https://docs.planetscale.com/tutorials/automatic-prisma-migrations#quick-introduction-to-prisma's-db-push)
- [Prisma Planetscale quick start](https://www.prisma.io/docs/guides/database/using-prisma-with-planetscale)
- [Prisma Schema Reference](https://www.prisma.io/docs/reference/api-reference/prisma-schema-reference)

**Create the DB and Branches:**
Via the Planetscale Dashboard or CLI:
1. create a Planetscale DB
2. create two new branches from `main`: *shadow* and *development*

For the *shadow* and *development* branches, we need to get a connection string from the Dashboard:
1. Make sure you choose the "Connect with Prisma" option. And get a new user:password.
2. Copy the connection strings into `.env`
3. For the *shadow* branch, change the env var from `DATABASE_URL` to `SHADOW_DATABASE_URL`

Now we can apply the Prisma Schema to the *development* branch and seed the DB:
```terminal
yarn redwood prisma migrate dev
```

### Fire it up
Once everything is working, you're ready for local dev 🚀

```terminal
yarn rw dev
```

Your browser should open automatically to `http://localhost:8910` to see the web app. Lambda functions run on `http://localhost:8911` and are also proxied to `http://localhost:8910/.redwood/functions/*`.

### Deployment
Deployment with Planetscale is similar to a git pull and merge workflow. First, create a request to merge *development* into main. This can be done via the Dashboard or CLI:
```
pscale deploy-request create <database> development
```

Then approve and deploy via the Dashboard or CLI:
```
pscale deploy-request deploy <database> <deploy-request-number>
```

####

## Resources
- [Tutorial](https://redwoodjs.com/tutorial/welcome-to-redwood): getting started and complete overview guide.
- [Docs](https://redwoodjs.com/docs/introduction): using the Redwood Router, handling assets and files, list of command-line tools, and more.
- [Redwood Community](https://community.redwoodjs.com): get help, share tips and tricks, and collaborate on everything about RedwoodJS.


