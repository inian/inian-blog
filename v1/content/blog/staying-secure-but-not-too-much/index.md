---
title: Staying secure, but not too much
date: "2018-11-17T22:40:32.169Z"
description: How to stay secure online without giving up too much of your convenience
---

I am not too hardcore when it comes to computer security. I do value convenience and try to balance it with online security all the time. For example, I like some of the recommendations that Google gives me from time to time.

It all comes down to who you trust with your security – you have to trust someone and I rather trust companies which actively focus a lot on security, rather than companies which have security layered on as an afterthought.

Here are some of the tools I use day to day to stay a bit more secure online.

- [Brave](https://brave.com/) with different profiles for social accounts, normal web browsing, development and super critical stuff (banking, your production AWS account, etc). After their recent 0.55 release which is based on the Chromium frontend, all my tiny gripes with them got resolved and it is my primary browser now.
- [UBlock Origin](https://github.com/gorhill/uBlock) and [HTTPS Everywhere](https://www.eff.org/https-everywhere) when I am not using Brave
- [1Password](https://1password.com/) for managing all my passwords. Using a password manager makes it a high value target for attackers and puts all your eggs in the same basket but I would rather trust a company with security in their DNA than trust the hundreds of online services I end up using to get their security right. I don’t use the browser extension though, I just use the native app. I have worked on web security a bit and I think the attack surface is too high and there are too many things that can go wrong when using the extension. I used to be on Lastpass but since they got acquired by LogMeIn, they absolutely failed to fix long standing UX issues which made the experience unbearable after a point. 1Password on the other hand is a absolute pleasure to use.
- [Authy](https://authy.com/) – My go to 2FA application. SMS 2FA is terribly insecure, but still better than nothing. There might be a risk of using both 2FA and password manager from the same company. But since the TOTP protocols don’t involve the server after the initial key exchange, I don’t think the 2FA keys can be compromised even if Authy servers are hacked. I also like Authy's backup of 2FA keys to multiple devices, so that even if my mobile device is lost, I don't lose access to my 2FA tokens. I know some hardcore security people are cringing as they read this, but hey this entire blog post was about balancing security and convenience and I think this is a reasonable risk to take. I also like that Authy has a decent application for the Apple watch.
- [Yubikey](https://www.yubico.com/) - I use Yubikey as a 2FA only where websites let me add multiple of second factors authentication. So I use a combination of Authy and Yubikey for these websites. For example, AWS doesn't let you add multiple 2FA to the root account, which is a shame - so I only use Authy there. You can buy multiple hardware based tokens and secure them but it seems like too much of a hassle. Hardware based tokens make it very convenient to add 2FA without opening your authenticator application each time and it also helps you against phishing attacks which is nice.
- [NordVPN](https://nordvpn.com/) – I used to use Tunnelbear before but a recent convert to NordVPN because Tunnelbear had way fewer servers compared to other VPN offerings (security vs convenience again at play). NordVPN seems to do decently well on the security aspects I care about.
- [1.1.1.1](https://1.1.1.1/) or 8.8.8.8 – I trust Cloudflare over my local ISP for my DNS resolver on both my computer and mobile.
- [Telegram](https://web.telegram.org/) – When it comes to messaging apps, these are the most important things for me (from descending to ascending)

  - Are my friends on it?
  - Desktop support
  - Does it allow to export my chat messages? (In case I want to switch phones / operating systems)
  - End to end encryption

Signal fails the first and the third points for me, so using Telegram and Whatsapp mostly these days.

Will keep updating this post as my thoughts around this stuff change and as newer technologies come out 🙂
