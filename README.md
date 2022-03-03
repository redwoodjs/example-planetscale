# Redwood
### Setup
This project uses Yarn 3, dbAuth, and SQLite. You need to:
1. install packages
2. apply migrations to the dev DB
3. create a local dbAuth secret


Follow these steps to get set up:

```terminal
yarn install
yarn redwood prisma migrate dev
echo "SESSION_SECRET=$(yarn --silent rw g secret --raw)" >> .env
```

### Fire it up

```terminal
yarn rw dev
```

Your browser should open automatically to `http://localhost:8910` to see the web app. Lambda functions run on `http://localhost:8911` and are also proxied to `http://localhost:8910/.redwood/functions/*`.
## Resources
- [Tutorial](https://redwoodjs.com/tutorial/welcome-to-redwood): getting started and complete overview guide.
- [Docs](https://redwoodjs.com/docs/introduction): using the Redwood Router, handling assets and files, list of command-line tools, and more.
- [Redwood Community](https://community.redwoodjs.com): get help, share tips and tricks, and collaborate on everything about RedwoodJS.


