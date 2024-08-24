# Org Tokens

Org tokens are the happy medium between deploy tokens and auth tokens. They are more powerful than deploy tokens, as they can control all apps within a given organization while deploys can only control one, but they aren't as powerful as auth tokens as they're confined to their specified organization. For example if you wanted to have a token that:

- is empowered to create/manipulate different test apps within an organization
- only has access to one staging/production/testing “environment” orgs
- can be used in the machine API to orchestrate machines for running user workloads within a specific org

Then an org token may be right for you!

## Create
There are two ways to both create and manage your token. You can either use the dashboard or command line. We'll show you how to use both.

### Dashboard

The url will look like `https://fly.io/dashboard/[your name]/tokens`

To navigate to this page from the main dashboard page, just scroll down to "tokens" in the left sidebar. At the top, you will see fields to add an optional org token name and expiration date. Once you've filled in those fields (or not, I did say they were optional after all), you simply click "Create Organization Token", and you now have a new org token for you to use as you please.

### Command line

To create an org token, simply run the following command:

```
fly tokens create org [flags]
```

The optional flags you can have include the following:

```
  -x, --expiry duration   The duration that the token will be valid (default 175200h0m0s)
  -h, --help              help for org
  -j, --json              JSON output
  -n, --name string       Token name (default "Org deploy token")
  -o, --org string        The target Fly.io organization
```

If you run this command with no flags, you will be prompted to pick the organization this token will apply to. A note that the org flag will want the org slug, not what is listed when you go through the prompt to pick the org. If you don't know the org slug, or want to confirm, just run `fly orgs list` to see a list of your orgs and their slugs.

As an example, if I wanted to create a token in my personal organization called "shiny happy org token", I would run the following:

```
fly tokens create org -o "personal" -n "shiny happy org token" 
```

## Manage

Just as you can create tokens through both the dashboard and command line, you can manage them in both places as well.

### Dashboard

From the page discussed in the [create section](#dashboard), you will see all your org tokens listed. If you want to revoke a token, simply click the revoke button on the far right.

### Command Line

To view your tokens, run the following command

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

You'll notice here that there are no org tokens. This is because, as mentioned in the optional flags, the default scope is at the app level. That means, if you want to see all tokens within your organization, you must run the following:

```
fly tokens list -s org
```

After you run this, you will be asked to pick an organization. After you confirm your organization, you will see your deploy tokens **and** all other tokens associated with that organization. For example, the output could look something like this:

```
? Select Organization: Personal
ID               NAME             	CREATED BY              EXPIRES AT                    
ID_number_1	 test org token   	your_email@email.com	2124-04-23 05:39:01 +0000 UTC	
ID_number_2	 litefs_cloud_poll	your_email@email.com	2024-05-17 06:38:26 +0000 UTC	
ID_number_3	 test deploy token	your_email@email.com	2124-04-23 05:39:39 +0000 UTC	
```

If you want to revoke any of these tokens, you will run the following command, where IDs are the ID numbers of the tokens you want to revoke:

`fly tokens revoke [IDs]`

For example, if I want to revoke the test org token, I would run the following command, after obtaining that ID number from the `fly tokens list` command:

`fly tokens revoke ID_number_1`


## Related links:

- [Fresh Produce Post](https://community.fly.io/t/org-scoped-tokens/13194)
- [Flyctl documenation for token creation](https://fly.io/docs/flyctl/tokens-create-org/)
- [Flyctl documentation about token listing](https://fly.io/docs/flyctl/tokens-list/)
- [Flyctl documentation about token revocation](https://fly.io/docs/flyctl/tokens-revoke/)
- [Flyctl documentation about listing orgs](https://fly.io/docs/flyctl/orgs-list/)
