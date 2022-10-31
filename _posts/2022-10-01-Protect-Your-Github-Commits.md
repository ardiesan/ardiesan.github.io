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



