# Machine-exec Tokens

These tokens will allow you to execute commands on a single machine. Note that if you do not specify which command, or range of commands, can be run, all commands will be allowed. In the interest of scoping your tokens as narrowly as possible, it's preferable to specify exactly which commands are allowed.

## Create

On the command line, you're going to run the following command:

```
fly tokens create machine-exec [command...] [flags]
```

You can add the following optional flags:

```
  -a, --app string               Application name
  -C, --command strings          An allowed command with arguments. This command must match exactly
  -p, --command-prefix strings   An allowed command with arguments. This command must match the prefix of a command
  -c, --config string            Path to application configuration file
  -x, --expiry duration          The duration that the token will be valid (default 175200h0m0s)
  -h, --help                     help for machine-exec
  -j, --json                     JSON output
  -n, --name string              Token name (default "flyctl machine-exec token")
```
TODO: I had trouble finding examples of how this is used, so I'm a bit shaky on what is meant by commands that can be used. I'd probably understand this better if I could have some use cases.

For example, if I wanted to create a machine-exec token named shiny happy machine-exec token that expires in 48 hours, I would run the following command:

```
fly tokens create machine-exec -n "shiny happy machine-exec token" -x 48h
```
TODO: make an example that doesn't allow all commands

## Manage

### Dashboard

Because these tokens execute commands on a single machine, and an app is a collection of machines and volumes, these tokens exist at the app level. Therefore, the way you manage these is similar to how you manage a deploy token (see post [here](deploy_token.md#dashboard-1)). You will click on the app that this token applies to, scroll down the left sidebar, and click on tokens. The URL will look like `https://fly.io/apps/[your app here]/tokens`. There, you'll see a table with a list of tokens for this app. On the far right for each token's entry, there is a button where you can revoke a token.

### Command line
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
ID               NAME             	        CREATED BY              EXPIRES AT                    	
ID_number_3	 test deploy token	        your_email@email.com	2124-04-23 05:39:39 +0000 UTC
ID_number_5      shiny happy machine-exec token	your_email@email.com	2024-05-20 05:06:17 +0000 UTC	
```

While that command will display the token you need, as the machine-exec token exists at the app level, if you want to see a list of all tokens, including those at the organization level along with those at the app level, you will want to run the following command:

```
fly tokens list -s org
```

You will be asked to pick an organization. After you confirm your organization list, you will see your deploy tokens **and** all other tokens associated with that organization. For example, the output could look something like this:

```
? Select Organization: Personal
ID               NAME             	        CREATED BY              EXPIRES AT                    
ID_number_1	 test org token   	        your_email@email.com	2124-04-23 05:39:01 +0000 UTC	
ID_number_2	 litefs_cloud_poll	        your_email@email.com	2024-05-17 06:38:26 +0000 UTC	
ID_number_3	 test deploy token	        your_email@email.com	2124-04-23 05:39:39 +0000 UTC
ID_number_5      shiny happy machine-exec token	your_email@email.com	2024-05-20 05:06:17 +0000 UTC	
```

If you want to revoke any of these tokens, you will run the following command, where IDs are the ID numbers of the tokens you want to revoke:

`fly tokens revoke [IDs]`

For example, if I want to revoke the shiny happy machine-exec token we created before, I would run the following command, after obtaining that ID number from the `fly tokens list` command:

`fly tokens revoke ID_number_5`



## Relevant Links:

- [Flyctl documentation for token creation](https://fly.io/docs/flyctl/tokens-create-machine-exec/)
- [Flyctl documentation about token listing](https://fly.io/docs/flyctl/tokens-list/)
- [Flyctl documentation about token revocation](https://fly.io/docs/flyctl/tokens-revoke/)