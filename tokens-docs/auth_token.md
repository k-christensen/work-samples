# Auth Tokens

This is the one token to rule them all (as in, it's the most powerful token of all the tokens described in this document set). This has the power to control any app in any organization you're in. A lot of times, especially when you're getting started, this will probably be the token you use for authentication purposes because it has the best odds of working, and you probably haven't gotten to the point where you've created other tokens. However, the trade off is that if this token gets compromised, you're going to have an exceptionally bad day. That's why the other five kinds of tokens exist, to make your bad days a little less horrific.

## Create

### Dashboard

On the fly.io dashboard, if you navigate to https://fly.io/user/personal_access_tokens, you can name and create another custom personal access token. 

### Command Line

TODO: I don't want to say this for certain, but I don't think there is a way to create an auth token on command line. I know you can view it (see [manage's command line section](#command-line-1)) but I don't think you can create one like you can on the dashboard. But I would verify this before writing this definitively.

## Manage

### Dashboard:

Speaking of https://fly.io/user/personal_access_tokens, if you're on that page, you will see all the tokens that currently exist and are in use. If you want to delete them, you can simply click revoke next to that token and it will no longer work.


### Command Line:

When you begin using fly, you will have an auth token. On the command line, you can view the auth token in use by running the following:

```
fly auth token
```

This will spit out the string that is the auth token in use.

TODO: Is there a way to delete auth tokens in command line the way you can on the dash? I saw when I tried to run `fly tokens list` it didn't show any of these tokens, so I'm not sure if there's another command just for this kind of token, or if this just only exists in the dashboard.

## Relevant links

- [Flyctl documentation for viewing the token in use](https://fly.io/docs/flyctl/auth-token/)
- [Example of an auth token in use](https://fly.io/docs/machines/api/working-with-machines-api/#environment-setup)

