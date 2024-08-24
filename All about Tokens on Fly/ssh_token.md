# SSH Tokens

These tokens give the bearer SSH access to a specified app.

TODO: Again, I don't know what else to put here, but I feel like we need more than a sentence. I think I'd want to look into what specific use cases for this look like so I could fill this out a bit more.

## Create

To generate a ssh token, run the following command:

`fly tokens create ssh [flags]`

Possible flags include the following

```
  -a, --app string        Application name
  -c, --config string     Path to application configuration file
  -x, --expiry duration   The duration that the token will be valid (default 175200h0m0s)
  -h, --help              help for ssh
  -j, --json              JSON output
  -n, --name string       Token name (default "flyctl ssh token")

```

For example, if you wanted to create a token called "shiny happy ssh token" for the app my-app and save it as "my-app.token.ssh", you would run the following:

```
fly tokens create ssh -a my-app -n "shiny happy ssh token" > my-app.token.ssh
```
This will create a file in the directory you are working in called my-app.token.ssh.

## Manage

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
ID               NAME             	  CREATED BY              EXPIRES AT                    	
ID_number_3	 test deploy token	  your_email@email.com	2124-04-23 05:39:39 +0000 UTC
ID_number_6	 shiny happy ssh token   your_email@email.com	2044-05-13 05:31:24 +0000 UTC	
```

While that command will display the token you need, as the SSH token exists at the app level, if you want to see a list of all tokens, including those at the organization level along with those at the app level, you will want to run the following command:

```
fly tokens list -s org
```

You will be asked to pick an organization. After you confirm your organization list, you will see your deploy tokens **and** all other tokens associated with that organization. For example, the output could look something like this:

```
? Select Organization: Personal
ID                  NAME             	    CREATED BY                  EXPIRES AT                    
ID_number_1	    test org token   	    your_email@email.com	2124-04-23 05:39:01 +0000 UTC	
ID_number_2	    litefs_cloud_poll	    your_email@email.com	2024-05-17 06:38:26 +0000 UTC	
ID_number_3	    test deploy token	    your_email@email.com	2124-04-23 05:39:39 +0000 UTC
ID_number_6         shiny happy ssh token   your_email@email.com	2044-05-12 07:34:34 +0000 UTC	
```

If you want to revoke any of these tokens, you will run the following command, where IDs are the ID numbers of the tokens you want to revoke:

`fly tokens revoke [IDs]`

For example, if I want to revoke ssh token created above, I would run the following command, after obtaining that ID number from the `fly tokens list` command:

`fly tokens revoke ID_number_6`

## Relevant links:

- [Fresh Produce post](https://community.fly.io/t/ssh-only-token/19217)
- [Flyctl documenation for token creation](https://fly.io/docs/flyctl/tokens-create-ssh/)
- [Flyctl documentation about token listing](https://fly.io/docs/flyctl/tokens-list/)
- [Flyctl documentation about token revocation](https://fly.io/docs/flyctl/tokens-revoke/)