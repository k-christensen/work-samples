# Deploy Tokens

These tokens are scoped in such a way that they only have the power to manage a single application. These are a much more narrow scope than the auth token, which controls every app over every organization you are in, and the org token, which controls every app within one of your specified orgs. These are great for continuous app deployment [through github actions](https://fly.io/docs/app-guides/continuous-deployment-with-github-actions/) or any other CD pipeline of your choosing.

## Create

There are two ways to generate a deploy token. Either through the dashboard or on the command line. We'll show you how to do both.

### Dashboard:

The URL will look like: `https://fly.io/apps/[your app here]/tokens`

To navigate there from the main dashboard page, click on your app. Then, on the left side bar, scroll down to tokens. This will take you to the page with the aforementioned URL. From there, all you do is click new token, then you will be allowed to optionally give your token a name and an expiration date. After you click create deploy token, you will see it under your list of tokens.

### Command Line:

To create a deploy token, simply run the following command:

```
fly tokens create deploy [flags]
```

The optional flags you can have include the following:

```
  -a, --app string        Application name
  -c, --config string     Path to application configuration file
  -x, --expiry duration   The duration that the token will be valid (default 175200h0m0s)
  -h, --help              help for deploy
  -j, --json              JSON output
  -n, --name string       Token name (default "flyctl deploy token")

```
So, if I wanted to create a new token and name it "shiny happy token", I would run the following command:

```
fly tokens create deploy -n "shiny happy token"
```

## Manage

Just as you can create tokens in both the dashboard and command line, you can manage them in both places as well.

### Dashboard

Once you are at the tokens page for your specific app (see [directions above](#dashboard)), you will see a list of all the deploy tokens currently in existence. If you want to delete one, there is a red button on the far right of each entry that says revoke. Once you confirm that you want to revoke this token, it will be deleted from the list and it will no longer work.

### Command Line

Note: this process is very similar to what is outlined in the [org token page](org_token.md#command-line-1)

To view your tokens, run the following command:

```
fly tokens list [flags]
```

The optional flags include the following:

```
  -a, --app string      Application name
  -c, --config string   Path to application configuration file
  -h, --help            help for list
  -s, --scope string    either 'app' or 'org' (default "app")

```

If you simply run `fly tokens list`, the output will look something like this:

```
ID               NAME             	CREATED BY              EXPIRES AT                    	
ID_number_3	 test deploy token	your_email@email.com	2124-04-23 05:39:39 +0000 UTC	
```

One notable flag is the scope string. Since we're talking about deploy tokens, the default flagless behavior of this command will only show you the deploy tokens for the app you are currently working on. Running the command with no flags and running the command with the `-s app` flag will give you the same output. However, if you run the following:

```
fly tokens list -s org
```

You will be asked to pick an organization. After you confirm your organization list, you will see your deploy tokens **and** all other tokens associated with that organization. For example, the output could look something like this:

```
? Select Organization: Personal
ID               NAME             	CREATED BY              EXPIRES AT                    
ID_number_1	 test org token   	your_email@email.com	2124-04-23 05:39:01 +0000 UTC	
ID_number_2	 litefs_cloud_poll	your_email@email.com	2024-05-17 06:38:26 +0000 UTC	
ID_number_3	 test deploy token	your_email@email.com	2124-04-23 05:39:39 +0000 UTC	
```

If you want to revoke any of these tokens, you will run the following command, where IDs are the ID numbers of the tokens you want to revoke:

`fly tokens revoke [IDs]`

For example, if I want to revoke the test deploy token, I would run the following command, after obtaining that ID number from the `fly tokens list` command:

`fly tokens revoke ID_number_3`


## Relevant links:

- [Reference doc about deploy tokens](https://fly.io/docs/reference/deploy-tokens/)
- [Fresh produce post about deploy tokens](https://community.fly.io/t/deploy-tokens/11895)
- [Flyctl documentation for token creation](https://fly.io/docs/flyctl/tokens-create-deploy/)
- [Flyctl documentation about token listing](https://fly.io/docs/flyctl/tokens-list/)
- [Flyctl documentation about token revocation](https://fly.io/docs/flyctl/tokens-revoke/)