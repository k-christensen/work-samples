# An Overview of Access Tokens

Think of tokens like comic con passes (if you're technical enough that you're reading this, I imagine you're familiar enough with nerd culture to get the gist). If you're running the entire con, then it would make sense for you to have an all access pass for every day that it runs. However, not every attendee gets an all-access full weekend pass (as much as they all may want one). Some will get full access, but only for one specific day. Some will have access to the main floor, but they won't be able to go to a meet and greet with their favorite actor.

In the same way, different kinds of access tokens are made so that you can grant different levels of access to your organization and apps. Theoretically, you could use your auth token for every use case. However, a central tenant in secure developing is the [principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege), which is the idea that any user or account should only have the permission to do what they absolutely need to do. For example, if a user only needs read access to a database, they shouldn't have the ability to create new entries in that database. This is why we have other kinds of tokens, so you can develop your apps as securely as possible and keep your more powerful tokens, and by extension your apps and users, as safe as possible.

## Auth Tokens

[To read more, go here](auth_token.md)

At the highest level, there is your auth token. This gives access to all your apps and all your organizations. This is your full weekend all access pass. Obviously, as this is the most powerful token, you want to use it as little as possible.

### Use cases:

TODO: Obviously, any time you need a token, you *can* use this token, but where are some scenarios where this is the *only* token you can functionally use? Ben's post about deploy tokens (https://community.fly.io/t/deploy-tokens/11895) does say that this is the token that gets used to "create, configure, deploy and manage all of your org’s apps and all their resources," but that feels too vague for here.

TODO: figure out if this is actually what they're called/what we're calling them to be consistent across docs and not confuse readers. Also I feel like I should have a bit more here for the use case besides "times where you need to control everything across every org"

## Org Deploy Tokens

[To read more, go here](org_token.md)

These tokens give you permission to manage multiple applications over a single organization. This would be like if you had a one day pass, but you had tickets for every meet and greet for that day.

### Use Cases:

- CI tokens that are empowered to create/manipulate different test apps within an organization
- CI Tokens that only have access to one staging/production/testing “environment” orgs
- A token for the machine API that can be used to orchestrate machines for running user workloads within a specific org

(From linked [fresh produce post](https://community.fly.io/t/org-scoped-tokens/13194))

TODO: I took this directly from the fresh produce post, not sure if this is the best way to cite it?

## Deploy Tokens

[To read more, go here](deploy_token.md)

These tokens give you permissions to manage a single application and its resources. This would be like if you had a ticket for one day to go to the meet and greets for the actors on a single show. 

### Use Cases:

- Deploying code for a single app
- Automating deployment in a CD pipeline as seen in [this article](https://fly.io/docs/app-guides/continuous-deployment-with-github-actions/)

TODO: This was from Ben's fresh produce post about deploy tokens. I feel like there are more concrete use cases I could add here. "Deploying code for a single app" is true, but we can do better.

## Machine-exec Tokens

[To read more, go here](machine-exec_token.md)

These tokens allow you to carry out a command, or commands, on a single machine inside an app. This would be like if you had a one day ticket with the ability to go to one meet and greet for a single show.

### Use Cases:

TODO: I wasn't able to find a whole lot about this token other than what I posted above. I'd probably ask around and see when you need to focus on a single machine inside an app. I talk about a possible use case in [my questions doc](questions.md#machine-exec-use-cases)

## Readonly Tokens

[To read more, go here](readonly_token.md)

This token allows the bearer to have readonly access only. That means they are able to view data, but they can't take any state-changing actions, like updates, additions, or deletions. Note that these readonly tokens are limited to a single org and its resources. This is like if you have a comic con ticket, but you're only allowed to look around the main hall and could not go to any meet and greets. No embarrassing celebrity interactions for you!

### Use Cases:

- Setting up a log shipper ([see this community post](https://community.fly.io/t/security-questions/18757)) or any other instance where you only want to process app data (such as logs)
- Any instance where you want the bearer to be able to view app data or app output, but you want to ensure the bearer does not have the ability to modify any data. Given that, this would be the most secure out of all the tokens, but the tradeoff is that there is no action it can take besides viewing data.

TODO: While I'm happy with that one community post addition because it shows a practical usage, again I feel like the second bullet point is a little broad. Another instance where I'd ask around and see what some other concrete uses are

## Litefs-cloud Tokens

[To read more, go here](litefs-cloud_token.md)

This token is limited to a accessing the contents of a single LiteFS Cloud cluster. This would be like if for some reason, the comic con had a library connected to it, and you had a key to that library. Look, I know that one was a stretch, please don't roll your eyes too hard.

TODO: Beef this up with answers to questions asked here

### Use Cases:

TODO: I don't know what else to say other than "you need to access data inside a LiteFS Cloud cluster." This feels like such a heavily scoped token that there really is only one use for it, but I could also be missing something. I didn't have a ton of time to dig into the links I posted in the litefs-cloud token page, but that would be where I'd look.

## SSH Tokens

[To read more, go here](ssh_token.md)

An SSH token will provide the bearer with SSH only access to a specified app. I'm not going to try to use the comic con metaphor here.

TODO: Another one where I feel like I should dive in a bit more. I suppose there is the ssh_token article, but I think given my TODO below, if I had a better sense of when people want to ssh into their app, I might be able to write a bit more about it here.

### Use Cases

TODO: Ask around and figure out what some tangible use cases are for ssh'ing into an app

## How it works

We have [a blog post](https://fly.io/blog/macaroons-escalated-quickly/) that dives into detail how macaroons, the type of tokens we use, are developed and used.

TODO: see my [questions about attenuation](questions.md#attenuation), also think of how to do a shorter version of this blog post so I'm not sending the dev to a post that they might not want to commit the time to reading, also I'd read through this to get a sense as to what other people have to say about macaroons and what other perspectives are/what questions other people care about answering https://news.ycombinator.com/item?id=39204314

Basically, I haven't digested enough about the inner workings of this to confidently describe how this works, but this is something I would want to add.
