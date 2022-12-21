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


## Steps to Setup

Signing a commit will require an SSH key or GPG key. We will use GPG key in this process.

```
gpg --full-generate-key
```

Choose the defaults or `ECC (sign and encrypt)` and `Curve 255419`. I strongly suggest setting an expiry date. End of the year, if you will.


Fill in and the needed information and make sure to use your github provided private email address. You can find that at [https://github.com/settings/emails](https://github.com/settings/emails) with the following pattern `@users.noreply.github.com`

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

Open (https://github.com/settings/keys)[https://github.com/settings/keys] on a browser and click `New GPG key` button. Paste the key.


Configure local copy of repository to use the signingkey. If you only work with one Git GitHub account, you do not need to use the `--local` option.

```
git config --local user.signingkey {short-key}
```

Assuming you are on MacOS and is using Homebrew, you can find your gpg application using `which`

```
which gpg
```

Copy the return path and configure git to use that as the `gpg.program`. Assuming the value is `/opt/homebrew/bin/gpg`


```
git config --local gpg.program /opt/homebrew/bin/gpg
```

## Auto-sign

Finally, setup git to always sign commits.

```
git config commit.gpgsign true
```
