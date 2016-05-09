# Heroku Buildpack for npm Authentication

This is a Heroku buildpack that enables authenticated npm operations
within a Heroku dyno.

It detects an `NPM_AUTH_TOKEN` environment variable and creates a `.npmrc` file.

It is the soul sister of the [GitHub Buildpack](https://github.com/zeke/github-buildpack)

## Usage

Use this one-liner to read your [npm auth token](http://blog.npmjs.org/post/118393368555/deploying-with-npm-private-modules):

```sh
cat ~/.npmrc | head -1 | sed 's/.*=//g'
```

Save the token in your Heroku app config:

```sh
heroku config:set GITHUB_AUTH_TOKEN=YOUR_TOKEN_HERE
```

Configure your app to use this buildpack:

```sh
heroku buildpacks:add --index 1 https://github.com/zeke/npm-buildpack
```

The next time you push your app to Heroku, this buildpack will create a
`.npmrc` file containing your npm token in the base directory of the app:

```sh
heroku run bash
cat .npmrc
//registry.npmjs.org/:_authToken=00000000-0000-0000-0000-000000000000
```

Now you can perform authenticated npm operations on the dyno, including
`npm publish`!
