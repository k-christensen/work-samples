# Access Tokens

Access tokens are the way in which a user is authenticated and their level of authorization is dictated by the kind of access token they have. There's the auth token, the output of `fly auth token`, which has the highest level of permissions. However, it's not very secure to use this all-powerful token everywhere. That's why we offer five other tokens with a more narrow scope of permissions. These other kinds of tokens are deploy tokens, litefs-cloud tokens, machine-exec tokens, readonly tokens, and ssh tokens. 

[Overview of Tokens](overview.md): As the title indicates, this is an overview of all the tokens you can use in the fly ecosystem. This is a great place to start if you know you need to use access tokens in your apps, but you're not sure what is best for your use case. 

Below are more detailed articles about the different kinds of tokens you can use. All of these articles will cover how to create and manage their respective token. They will also all have links to related community posts and documentation:

- [Auth tokens](auth_token.md)

- [Deploy Tokens](deploy_token.md)

- [Machine-exec Tokens](machine-exec_token.md)

- [Readonly Tokens](readonly.md)

- [Litefs-Cloud Tokens](litefs-cloud_token.md)

- [SSH Tokens](ssh_token.md) 