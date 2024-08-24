# Litefs-cloud Tokens

These tokens are specifically for accessing the data in a LiteFS-cloud cluster. These are created at the organization level.

TODO: I should say more than a couple sentences, but I'm not really sure what else to say here. These feel like such a specific use case, so it's not like with a deploy token where there were multiple possible applications.

## Create

On the command line, you're going to run the following command:

```
fly tokens create litefs-cloud [flags]
```

You can add the following flags:

> Note that the cluster name is mandatory. You cannot create this token without a litefs-cloud cluster.

```
  -c, --cluster string    Cluster name
  -x, --expiry duration   The duration that the token will be valid (default 175200h0m0s)
  -h, --help              help for litefs-cloud
  -j, --json              JSON output
  -n, --name string       Token name (default "LiteFS Cloud token")
  -o, --org string        The target Fly.io organization

```

TODO: I didn't have the time to go through the whole process of diving more into litefs-cloud and creating a cluster. If I had more time, I'd dive into this more so I could actually give an example I could test.

## Manage

TODO: Actually test to see if everything I'm saying in this section is accurate. I don't see any reason why it shouldn't, since it's just a specialized org-level token, but this could be wrong since, given my TODO above, I didn't have time to test this. Also, I'm not including a dashboard section here, as I didn't have time to test this myself, and I'm less confident about how the dashboard behaves with litefs-cloud tokens.

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

If you want to see the tokens you've set at the organizational level, you have to specify in your command that the scope should be at the org level. Therefore, if you run the following:

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

For example, if I want to revoke the litefs_cloud_poll token, I would run the following command, after obtaining that ID number from the `fly tokens list` command:

`fly tokens revoke ID_number_2`


## Relevant links:

- [Relevant blog post about LiteFS-cloud](https://fly.io/blog/litefs-cloud/)
- [Existing documentation about LiteFS](https://fly.io/docs/litefs/)
- [Flyctl documentation for token creation](https://fly.io/docs/flyctl/tokens-create-litefs-cloud/)
- [Flyctl documentation about token listing](https://fly.io/docs/flyctl/tokens-list/)
- [Flyctl documentation about token revocation](https://fly.io/docs/flyctl/tokens-revoke/)