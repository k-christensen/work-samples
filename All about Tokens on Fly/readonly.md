# Readonly Tokens

Readonly tokens are similar to org tokens (see [article about org tokens](#org_tokens.md)) in that they are scoped at the organization level. However, they are unique in that they only have the permission to read data. They cannot add, update, or destroy anything in the organization they are scoped to. These are great for instances like viewing logs or any case where the bearer may not need writing and destroying permissions and you want to maintain the security of the organization as much as possible. While it's obviously not ideal to have any token compromised, this kind of token being compromised is a significantly less bad day than if any other token were compromised.

## Create

On the command line, you're going to run the following command:

```
fly tokens create readonly [flags]
```

You can add the following optional flags:

```
  -x, --expiry duration   The duration that the token will be valid (default 175200h0m0s)
      --from-existing     Use an existing token as the basis for the read-only token
  -h, --help              help for readonly
  -j, --json              JSON output
  -n, --name string       Token name (default "Read-only org token")
```

For example, if I wanted to create a readonly token named shiny happy readonly token that expires in 48 hours, I would run the following command:

```
fly tokens create readonly -n "shiny happy readonly token" -x 48h0m0s
```
TODO: Figure out how to use the `--from-existing` flag. I've tried using the name of the token I want to base this off of (see below), I've tried the ID of the token both in and out of quotes (as in `fly tokens create readonly -n "dragonfly readonly token" --from-existing zQgwgYqbO69nqf9qXZzvRQXnkKCmvamx9bzpxgAU23` and `fly tokens create readonly -n "dragonfly readonly token" --from-existing "zQgwgYqbO69nqf9qXZzvRQXnkKCmvamx9bzpxgAU23"`). The error I keep getting is
```
Error: pass a macaroon token (e.g. from `fly tokens deploy`) as the -t argument or in FLY_API_TOKEN
```
I've tried setting the deploy token as an environment variable and passing that as the -t arguement and that's not working, and I've tried passing the actual token (as in the output of fly tokens create deploy) as the -t argument. 

I'm leaving in the below paragraph because this is the prose that I would use, but this needs to be corrected so that it's doing the right thing

> If you need the scope of your readonly token to be more narrow than at an organizational level, you can create a token scoped at your preferred scope, then create a readonly token using the --from-existing flag. For example, if I wanted a readonly token that was scoped to a specific app, I would create a deploy token for that app, then created a readonly token based on that deploy token. So, if that deploy token were called "dragonfly deploy token," then I would run the following command:

```
fly tokens create readonly -n "dragonfly readonly token" --from-existing "dragonfly deploy token"
```

## Manage

### Dashboard
Note: This is similar to the process outlined in the [org token page](org_token.md#dashboard-1)

To get to the organization tokens page, scroll down to "tokens" in the left sidebar. The url will look like `https://fly.io/dashboard/[your name]/tokens`

Note: unlike the output of `fly tokens list -s org` (see the section below for details), this tokens dashboard will **not** list tokens scoped to a single app, like a deploy token.

To revoke a token, just hit the red revoke button on the far right for the unwanted token.

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

One notable flag is the scope string. Now, remember at the beginning how I said that this token exists at the org level? That's important here because the default scope of the command is the app level. Therefore, you're not going to see your readonly token unless you run the following command, which forces the token list to work at the org level.

TODO: when I figure out the `--from-existing` bit, see if the readonly token created from the existing deploy token shows up in that command

```
fly tokens list -s org
```

If you do not list your org's slug, you will be asked to pick an organization. After you confirm your organization, you will see your app level tokens **and** all other tokens associated with the selected organization. For example, the output could look something like this:

```
? Select Organization: Personal
ID               NAME             	        CREATED BY              EXPIRES AT                    
ID_number_1	 test org token   	        your_email@email.com	2124-04-23 05:39:01 +0000 UTC	
ID_number_2	 litefs_cloud_poll	        your_email@email.com	2024-05-17 06:38:26 +0000 UTC	
ID_number_3	 test deploy token	        your_email@email.com	2124-04-23 05:39:39 +0000 UTC
ID_number_4	 shiny happy readonly token   	your_email@email.com	2024-05-20 01:46:18 +0000 UTC	
```

If you want to revoke any of these tokens, you will run the following command, where IDs are the ID numbers of the tokens you want to revoke:

`fly tokens revoke [IDs]`

For example, if I want to revoke the readonly token we created before, I would run the following command, after obtaining that ID number from the `fly tokens list` command:

`fly tokens revoke ID_number_4`


## Relevant links:

- [Example of a readonly token in use](https://community.fly.io/t/security-questions/18757/5)
- [Flyctl documentation for token creation](https://fly.io/docs/flyctl/tokens-create-readonly/)
- [Flyctl documentation about token listing](https://fly.io/docs/flyctl/tokens-list/)
- [Flyctl documentation about token revocation](https://fly.io/docs/flyctl/tokens-revoke/)