---
layout: post
title: "Protect Your Github Commits"
author: ardie
last_modified: 2022-10-31 16:18:28
tags:
- mindset
- git
- security
- wares
---

Did you know that anyone who knows the email you used in github could make a `push`
as if they were you?
<!-- more -->

No, this is NOT a security flaw in github. This is the very design of git. Git is meant 
to be decentralized with no central repository to speak of, but habits are hard to break
and control is something that enterprises want over its assets. And centralizing a 
decentralized system was born on top of git. Git's original design was such that we only pull 
from and push to trusted peers. And trusted peers are easy to get to agree to follow conventions.

## TLDR;

The official git documentation provides a comprehensive [sign your work](https://git-scm.com/book/en/v2/Git-Tools-Signing-Your-Work)
article in using GPG.

## Why Sign Your Work

Why Sign? Because anyone can make a commit as you if they know one or two of the emails you use to contribute work for either private or public git managed source codes.

A malicious individual can submit code under your name and email, contributing poor quality or malicious code.

## Pre-Setup and Assumption

This article assumes you are on MacOS and is familiar with executing commands via Terminal.app 

If you are on Linux, WSL, Android, or BusyBox, installation of:

* Git Credential Manager (GCM)
* GPG
* pinentry 

is out of scope of this article.


### Install Requirements


```
brew install git git-credential-manager gpg pinentry-mac 
```


## GPG Key Generation & Github Setup

Signing a commit will require an SSH key or GPG key. We will use GPG key in this process.

```
gpg --full-generate-key
```

Choose the defaults or `ECC (sign and encrypt)` and `Curve 255419`. I strongly suggest setting an expiry date. End of the year, if you will.


Fill in the needed information and make sure to use your github provided private email address. You can find that at [https://github.com/settings/emails](https://github.com/settings/emails) with the following pattern `@users.noreply.github.com`

Make sure that the checkbox `Keep my email addresses private` together with `Block command line pushes that expose my email` is checked.

This is also a good time to re-configure your Git to no longer use your private email. Repositories can be scanned and is a good source of valid emails for spamming and phishing. 

```
git config user.email {github_provided_username}@users.noreply.github.com 
```

Now that that is configured, time to export the generated keys so we can tell GitHub which keys proves who we are. First step is to get the key id, specifically the short key.

```
gpg --list-keys --keyid-format short
```

The shortkey is usually after `pub  ed25519/`, copy the short key and execute the command below:

```
gpg --export --armor {short-key}
```

Copy the text in the terminal or use `pbcopy` to send exported data to clipboard

```
gpg --export --armor {short-key} | pbcopy
```

Open [https://github.com/settings/keys](https://github.com/settings/keys) on a browser and click `New GPG key` button. Paste the key.


Configure git to use a signingkey.

```
git config user.signingkey {short-key}
```

## Git Signing Application

Find the path of the gpg application using `which`

```
which gpg
```

Copy the return path and configure git to use that as the `gpg.program`. Assuming the value is `/opt/homebrew/bin/gpg`


```
git config gpg.program /opt/homebrew/bin/gpg
```

or use backtick to use a command output as a command argument like:

```
git config gpg.program `which gpg`
```

## Signing New Commits

To sign new commits, simply add `-S` to your `git commit` like so

```
git commit -S -m "This commit will be signed with the configured signing key"
```

## Auto-Sign New Commits

Finally, setup git to always sign commits.

```
git config commit.gpgsign true
```

With the `commit.gpgsign` configured to true, there will be no need to add `-S` to every commits made.



## Multi-Project, Multi-Email, Multi-Keys/Machine

If you work with the above scenarios, you can use the `--local` option when executing `git config` for every local checkout you have.
