---
title: Programming Principles
author: vwkd
index: 1
tags:
  - meta
---

## Purpose of code

- "Any fool can write programs that a computer can understand. Good programers write programs that humans can understand." - Martin Fowler
- "Programs must communicate clearly to people not the computer." - Douglas Crockford
- "The real purpose of code is to communicate ideas to other people." - Kyle Simpson
- "We don't spend our time typing, that's not where our time goes. It's looking into the abyss, that's where our time goes." - Douglas Crockford
- "Don't be clever just to save a couple of characters. Be more explicit about it in the places where explicitness is more communicative.` - Kyle Simpson
- use comments to explain the _why_ not the _what_, sometimes the _how_ if it's too confusing, because can see _what_ is done but doesn't know _why_ it's done
- "If you made what you though was progress, it is okay to abandon that, if it turns out to be in the wrong place" - [Rich Harris](https://youtu.be/BzX4aTRPzno?t=710)
- "Whenever there is a divergence between what your brain thinks is happening and what is actually happening in the computer, that's where bugs enter the code." - Kyle Simpson
- "Stop prioritizing what the developer thinks, start prioritizing what the person using it wants." - Kyle Simpson
  - We should stop building apps for developers but for the person that uses it.
  - Don't build app according to layers of technology but layers of experience



## Focus of code

- YAGNI "You ain't gonna need it!": don't build things that you forsee you need in favor of things that you need right now.
- Second System Syndrome: be mindful about over-engineering your system when you feel confident because you aquired experience



## Empowerability (E12Y)

(Kyle Simpson)
- Don't build for the developer, the user, the customer, etc. Build for the _human being_ that is going to use it.
- Don't build app in layers of technology, progressive enhancement, e.g. load first CSS then JS, hide loading using placeholders like different fonts, images, spinners, etc. Build app in layers of user experience.
- Don't build app according to what device supports, what runtime supports (e.g. Browser), etc. Build app for the user experience, e.g. what if bandwith is expensive, power is not available for one week, hand is shaky while running, bad lighting conditions, etc.
- Don't build app with pixel-perfect consistent design. Build app with adaptive design to individual person



## Naming

- Use [Semantic Versioning](https://semver.org/) as version identifier, deterministic version pinning, separate commit to update dependencies
- "Version numbers signal the intent of the release, but don’t guarantee anything." - [Joel Parker Henderson - Versioning](https://github.com/joelparkerhenderson/versioning)
- "Semver is like traffic lights. Cars may run them, but it doesn't mean they are useless." - [Joel Parker Henderson - Versioning](https://github.com/joelparkerhenderson/versioning)
- "In Web Development the Hamster Wheel of Backwards Incompatibility has become more of a Hamster Centrifuge." -  Steve Losh
- "Every backwards-incompatible change costs the world an amount of time." - Steve Losh on [Volatile Software](https://stevelosh.com/blog/2012/04/volatile-software/)
- Use [Conventional Commits](https://www.conventionalcommits.org/) to name commit messages
- Use [BEM](http://getbem.com/naming) naming convention to name DOM element class names



## Features

- "Every asynchronous operation should have cancellation built in to it." - Kyle Simpson



## Resources

- [Joel Spolsky - The Joel Test: 12 Steps to Better Code](https://www.joelonsoftware.com/2000/08/09/the-joel-test-12-steps-to-better-code/)