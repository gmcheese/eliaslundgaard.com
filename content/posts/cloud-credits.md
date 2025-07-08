---
categories: ["projects"]
date: 2025-07-08T10:59:00+01:00
tags: ["guides"]
title: "Cloud Credits: How to Get Them and What to Consider"
show-meta: false
toc: false
---

Anyone doing a startup these days are probably going to use the cloud in some extend. Whether you're partial to Azure, GCP or AWS (or some obscure neo-cloud), you'd probably rather spend your hard earned money on hires or LLM tokens than something as boring as data storage or VPSs.

A surefire way to save lots of money in the early days is to scoop up cloud credits. Most major cloud providers offers credits, partly to help you, mostly to lock you in. Getting them isn't just a form - it takes a bit of forward thinking and strategizing on how and when you apply from them.

At [Wisp Compute](https://wispcompute.com), we have received credits from multiple providers which has helped us support larger customers from day one. Highly recommend doing the same!

## Credit Tiers Overview

I'm going to focus on the AWS, Azure and GCP. The remaining providers probably employ the same strategies, so even if you're not a big on hyperscalers, you can continue reading.

Each provider structure their credits into tiers. The idea is that you should get credits that match your current stage, and probably also a security measure for the providers to avoid abuse (credits can be abused by spinning up GPUs and mining crypto on your account - so keep those keys safe!). The more traction you have, the more credits you can access.

The following tiers exists across providers:

| Cloud Provider | Tier                         | Max Credits            | Eligibility Highlights     |
| -------------- | ---------------------------- | ---------------------- | -------------------------- |
| **GCP**        | Start                        | \$2,000 over 1 year    | Early, pre-equity startups |
|                | Scale                        | \$100k 1-2 yrs         | VC/accelerator backed      |
|                | AI + Web3 Startup Tier       | \$350k over 2 yrs      | AI-focused startups        |
| **AWS**        | Activate Founders            | \$1,000                | Self-funded early startups |
|                | Activate Portfolio           | \$100,000              | VC/accelerator backed      |
|                | Provider Intermediate Tiers  | \$25k–\$100k           | Stage-based via providers  |
| **Azure**      | Startup Credit Offer         | \$1,000 for 90 days    | Early/unincorporated       |
|                | Founders Hub (unfunded)      | \$5,000                | Incorporated but no VC     |
|                | Investor Offer (VC-backed)   | \$100,000+             | VC/accelerator backed      |

## Getting Started

Before you even start applying, make sure that you've got:

* A working website explaining what you do.
* A general idea of what services you will use on the platform.
* (Ideally) an incorporated entity.

I found it useful to run a Free Tier account for a few weeks, to be able to show what I needed. Note that this does not count for Azure, where you're not allowed to have an existing account (silly).

Once you have that, you should apply for the startup programs: 

* [Google Cloud for Startups](https://cloud.google.com/startup)
* [AWS Activate](https://aws.amazon.com/activate/activate-landing/)
* [Microsoft for Startups](https://portal.startups.microsoft.com/signup)

From application to acceptance it took:
* GCP: 3 days
* AWS: 2 days (via [NVIDIA Inception](https://www.nvidia.com/en-us/startups/) which I highly recommend if you're in AI)
* Azure: 1 day

All in all, not a lot of work for lots of credits!

## Getting More Credits

It might feel tempting to max out your credits as quickly as possible, but that might now be the smartest thing to do long term. First of all, these credits will expire in 12-24 years. Although it is possible to negotiate an extension, you might be better off staying in a lower tier for as long as possible.

Secondly, the providers care about, and monitors, your growth. That means, that they will only unlock the next tier of credits if they can see a consistent and growing spend pattern on your account. For example, Google wants to see a monthly spend of $400 to consider moving you to the $100K tier (no reference here, I remember discussing this with a Googler but number might be off).

### Hidden Tiers

As with anything in life, connections do help. There are more tiers than the providers advertise. For example, we have been in a $20.000 tier on GCP, and I know that there is a $40.000 tier as well. The problem is that accessing these funds requires you to get to know the people in the respective programs, and negotiate with them. My point is - don't be afraid to ask! It's better to go to a $20K tier than $100K if you do not need it.

### Accelerators and Partnerships

Another hack is that participation in a verified accelerator can help unlock credits. For example, Denmark's [Innofounder](https://innovationsfonden.dk/en/p/innofounder) unlocks credits directly. Sadly there is no official list of verified accelerators, and which platforms recognizes them, so you may need to ask your program manager if they're affiliated.

## Bonus perks

In addition to credits, participation in the programs also gives you tons of other benefits. Some of the offers we've drawn advantage of are:

* Microsoft for Startups (Founder's hub)
    * Github Enterprise 1 year free
    * Office 365 1 year free
* Google for Startups
    * Google Workspace Business Plus 1 year free (this one you need to ask for)

There's also other nice offers, like LinkedIn Sales Navigator and Gemini Pro access, as well as many, many less useful offers that they will try to get you on.

## Gotchas

Now free cloud credits - and lots of them - that seems too good to be true! Well, they're good, but there are some minor inconveniences. First of all, the credits do not solve the general challenges with cloud. The support that you get with these offers is basically useless (Microsoft takes the cake for worst support), so you get the fish, but not the rod.

Another problem is that on some platforms, those credits are not "equal" to hard cash. For example, I have hade major challenges getting quota increases for GPUs on my Sponsored Azure Subscription as Microsoft prioritises "paying" customers. That basically renders the credits useless for an AI startup.

With that said, the credits are god-send at the early stages. Just watch out for vendor lock-in!