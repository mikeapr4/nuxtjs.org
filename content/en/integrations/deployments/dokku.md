---
template: guide
title: Dokku
description: How to deploy a Nuxt application on Dokku?
target: Server
category: deployment
logo:
  light: "/img/partners/dark/dokku.png"
  dark: "/img/partners/light/dokku.png"
---
# Deploy Nuxt on Dokku

How to deploy a Nuxt application on Dokku?

---

We recommend to read [Dokku documentation for the setup](http://dokku.viewdocs.io/dokku/getting-started/installation/) and [Deploying a Node.js Application on Digital Ocean using Dokku](http://jakeklassen.com/post/deploying-a-node-app-on-digital-ocean-using-dokku/).

For the example, we will call our Nuxt application `my-nuxt-app`.

We need to tell Dokku to install the `devDependencies` of the project (to be able to launch `npm run build`):

```bash
// on Dokku Server
dokku config:set my-nuxt-app NPM_CONFIG_PRODUCTION=false YARN_PRODUCTION=false
```

Also, we want our application to listen on the host `0.0.0.0` and run in production mode:

```bash
// on Dokku Server
dokku config:set my-nuxt-app HOST=0.0.0.0 NODE_ENV=production
```

You should see these 3 lines when you type `dokku config my-nuxt-app`

![nuxt config vars Dokku](https://i.imgur.com/9FNsaoQ.png)

Then, we tell Dokku to launch `npm run build` via the `scripts.dokku.predeploy` script in our project `app.json`:

`create a file name app.json in our project root folder`

```js
{
  "scripts": {
    "dokku": {
      "predeploy": "npm run build"
    }
  }
}
```

To launch the application we run `npm run start` using the [Procfile](http://dokku.viewdocs.io/dokku/deployment/methods/dockerfiles/#procfiles-and-multiple-processes):

```
web: npm run start
```

Finally, we can push our app on Dokku with:

```bash
// commit your change before push.
git remote add dokku dokku@yourServer:my-nuxt-app
git push dokku master
```

Voilà! Our Nuxt application is now hosted on Dokku!
