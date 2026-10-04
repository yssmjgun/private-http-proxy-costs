# private http proxy: what dedicated IPs actually cost, how to set one up, and when HTTP beats SOCKS5

Search "private http proxy" and you get two piles of results that barely talk to each other. One pile is networking theory — the HTTP Definitive Guide treats private HTTP proxies as an oddity, small proxies running on the client machine to extend browser behaviour. The other pile is proxy shops selling "private proxies" without ever explaining what's private about them. If you searched this term, you're probably trying to answer a more practical question: do I need an IP that only I use, or is a rotating shared pool enough for the job in front of me?

That question has a real answer, and it mostly comes down to session state, IP reputation and how you're billed. 9Proxy is one of the vendors selling into this space, and its package structure happens to illustrate the trade-offs clearly — so it's a useful worked example, not a detour.

## What "private" means once you take it out of the textbook

A private proxy is one where a single client is the only user of that IP at any given moment. Requests go through the proxy, the proxy's IP replaces yours, and nobody else is sharing that address while you're on it. That's the whole idea.

Two things decide whether a proxy is genuinely private in a useful way:

- **Exclusivity.** You rent the IP, not a slice of a pool. Nobody else's browsing habits land on your reputation score.
- **History.** What that IP did before you got it. A dedicated IP that spent last month in a credential-stuffing botnet is worse than a shared one from a clean range.

That second point is why "residential" keeps coming up in this category. IP reputation systems grade residential addresses differently from datacenter ranges, and a residential IP from the same city as your target tends to see the local version of a page rather than a geo-redirected or throttled one.

9Proxy's IP-based residential plans sit closest to the classic rented-private-IP model: you buy a fixed number of IPs, bind each one to a port, and there's no traffic meter — bandwidth is unlimited while that IP is assigned to you. Scrape a hundred pages or ten thousand through it and the bill doesn't move. That combination is the reason people pick per-IP billing for long runs where page weight is out of their control.

## HTTP vs HTTPS vs SOCKS5, minus the hand-waving

This is where most guides get vague, and it matters more than the branding.

An **HTTP proxy** speaks HTTP on your behalf and tunnels HTTPS through a CONNECT request. It only handles TCP; UDP-based traffic doesn't go through it. For browsers, scrapers, HTTP clients and API polling, that covers almost everything you'll do.

An **HTTPS proxy** is the same thing with TLS between you and the proxy server, so the hop to the proxy isn't in plaintext.

**SOCKS5** sits lower in the stack. It forwards arbitrary TCP traffic and, depending on implementation, UDP, which is why it's the fallback for applications with no proxy support at all.

| Protocol | What it carries | Where it fits | What to watch |
| --- | --- | --- | --- |
| HTTP | TCP only, HTTPS via CONNECT | Browsers, scrapers, HTTP clients, rank trackers | No UDP, and don't put it on public networks unencrypted |
| HTTPS | Same as HTTP, TLS to the proxy | Anywhere credentials or traffic shouldn't cross the network in the clear | Slightly more setup in some tools |
| SOCKS5 | TCP, plus UDP in many implementations | Apps with no native proxy settings, automation stacks, anti-detect browsers | Fewer HTTP-level controls |

9Proxy supports HTTP, HTTPS and SOCKS5 on the same credentials, so switching protocol is a dropdown change rather than a new purchase. If you want to see how the packages are split across those protocols, 👉 [check the current 9Proxy package line-up](https://bit.ly/9-Proxy).

## Who actually needs an exclusive IP

Residential proxies show up in more places than the scraping blogs admit:

- Price monitoring and competitor product data, where a blocked IP means a gap in the dataset
- SERP and rank tracking from a specific city rather than a generic location
- Ad verification — checking that placements actually render for real users in real markets
- Security research and WAF testing, where you need a clean residential baseline before you can claim anything about how a target behaves
- OSINT and reconnaissance, where your IP is part of your operational profile
- Multi-account work, where each account needs an identity that survives a login

For a one-off check of what a page looks like in another country, a shared rotating pool is fine and cheaper. Private IPs start earning their price when state has to survive: logged-in sessions, carts, accounts, and scrapes long enough that you can't afford to restart every time an address goes down.

## The billing model decides more than the provider does

There are two ways to pay, and picking the wrong one is the most common way to waste money here.

**Per IP, unlimited bandwidth.** You buy N dedicated IPs. Page weight stops mattering. Best when sessions need to persist, throughput is high, or you genuinely can't predict how many megabytes a job will pull.

**Per GB.** You buy traffic and generate endpoints freely, rotating as often as you like. Best when each request is small and rotation is the point — API polling, geo checks, lightweight scraping.

9Proxy sells both, plus bundles that mix them. One thing worth knowing before you compare prices across the internet: **9Proxy changed pricing on IP-based and bundle packages on 1 June 2026, while GB packages stayed the same.** That's why half the price lists you'll find still show older, lower figures — and why some Chinese-language write-ups quote $20 for 100 IPs where the current page says $24.

The company's headline "from $0.015/IP and $0.68/GB" is real but only reachable at the top of the range: $0.68/GB is the 10,000 GB package, and per-IP costs only fall into the $0.018–0.024 band at the 100,000–500,000 IP tiers.

## Every 9Proxy package, as currently listed

| Package | What you get | Price | Effective rate | Validity | Order |
| --- | --- | --- | --- | --- | --- |
| IP 100 | 100 residential IPs, unlimited bandwidth | $24 | $0.24/IP | Until IPs are used up | [Get the 100 IP package](https://bit.ly/9-Proxy) |
| IP 500 | 500 residential IPs, unlimited bandwidth | $72 | $0.144/IP | Until IPs are used up | [Get the 500 IP package](https://bit.ly/9-Proxy) |
| IP 1,000 + 500 bonus | 1,500 IPs total | $126 | $0.084/IP | Until IPs are used up | [Get the 1,500 IP package](https://bit.ly/9-Proxy) |
| IP 2,500 | 2,500 residential IPs | $210 | $0.084/IP | Until IPs are used up | [Get the 2,500 IP package](https://bit.ly/9-Proxy) |
| IP 5,000 | 5,000 residential IPs | $360 | $0.072/IP | Until IPs are used up | [Get the 5,000 IP package](https://bit.ly/9-Proxy) |
| IP 15,000 | 15,000 residential IPs | $720 | $0.048/IP | Until IPs are used up | [Get the 15,000 IP package](https://bit.ly/9-Proxy) |
| IP 25,000 | 25,000 residential IPs | $863 | ~$0.035/IP | Until IPs are used up | [Get the 25,000 IP package](https://bit.ly/9-Proxy) |
| IP 50,000 | 50,000 residential IPs | $1,438 | ~$0.029/IP | Until IPs are used up | [Get the 50,000 IP package](https://bit.ly/9-Proxy) |
| Business IP 100,000 | 100,000 residential IPs | $2,300 | $0.023/IP | Until IPs are used up | [Get the 100,000 IP package](https://bit.ly/9-Proxy) |
| Business IP 200,000 | 200,000 residential IPs | $4,140 | $0.021/IP | Until IPs are used up | [Get the 200,000 IP package](https://bit.ly/9-Proxy) |
| Business IP 500,000 | 500,000 residential IPs | $8,625 | $0.018/IP | Until IPs are used up | [Get the 500,000 IP package](https://bit.ly/9-Proxy) |
| GB 5 | 5 GB of rotating residential traffic | $15 | $3.00/GB | 180 days | [Get the 5 GB package](https://bit.ly/9-Proxy) |
| GB 50 + 5 bonus | 55 GB of traffic | $105 | $2.10/GB | 180 days | [Get the 55 GB package](https://bit.ly/9-Proxy) |
| GB 100 | 100 GB | $150 | $1.50/GB | 180 days | [Get the 100 GB package](https://bit.ly/9-Proxy) |
| GB 200 | 200 GB | $200 | $1.00/GB | 180 days | [Get the 200 GB package](https://bit.ly/9-Proxy) |
| GB 1,000 | 1,000 GB | $800 | $0.80/GB | 180 days | [Get the 1,000 GB package](https://bit.ly/9-Proxy) |
| GB 2,000 | 2,000 GB | $1,500 | $0.75/GB | 180 days | [Get the 2,000 GB package](https://bit.ly/9-Proxy) |
| Enterprise GB 3,000 | 3,000 GB | $2,160 | $0.72/GB | No expiry | [Get the 3,000 GB enterprise package](https://bit.ly/9-Proxy) |
| Enterprise GB 6,000 | 6,000 GB | $4,200 | $0.70/GB | No expiry | [Get the 6,000 GB enterprise package](https://bit.ly/9-Proxy) |
| Enterprise GB 10,000 | 10,000 GB | $6,800 | $0.68/GB | No expiry | [Get the 10,000 GB enterprise package](https://bit.ly/9-Proxy) |
| Bundle Starter | 100 IPs + 5 GB | $30 | — | 180 days on the traffic | [Get the Starter bundle](https://bit.ly/9-Proxy) |
| Bundle Popular | 1,500 IPs + 50 GB | $180 | — | 180 days on the traffic | [Get the Popular bundle](https://bit.ly/9-Proxy) |
| Bundle Pro | 5,000 IPs + 500 GB | $720 | — | 180 days on the traffic | [Get the Pro bundle](https://bit.ly/9-Proxy) |

Two details in that table are easy to miss. IP-based packages don't expire on a clock — the balance stays until the IPs are consumed. GB packages, except the enterprise tiers, run on a 180-day validity window, which suits project-based work better than a monthly subscription you have to keep feeding.

## Setup: two paths, and they're genuinely different

**If you bought IP-based**, the workflow runs through the desktop client. You filter the pool by country, state, city, ZIP or ISP, right-click the IP you want, and forward it to a local port. From then on that IP is yours on, say, `127.0.0.1:6000`, and any tool pointed at that port exits through it. Proxy authentication is optional. Individual IPs stay alive somewhere between a few hours and roughly a day, and the app can auto-replace one that drops.

Because the port forwarding happens locally, this path works for software that has no proxy settings at all.

**If you bought GB-based**, nothing needs installing. You create a sub-account in the dashboard and generate endpoints with a structured username:


<subaccount>-country-US-st-<state>-city-<city>-isp-<isp>-ssid-<session>-sst-<session_time>


Those fields control targeting and session behaviour: country, state, city, ISP, a session ID for stickiness, and a session duration. Authentication is either username and password or an IP whitelist. Rotation mode hands you a fresh IP automatically; sticky mode holds one for the session window you set.

From there it plugs into whatever you already use — HTTP or SOCKS5 in Chrome or Firefox extensions for spot checks, anti-detect browsers like Hidemyacc, BitBrowser or ixBrowser for profile-per-account work, Proxifier to force an application without native proxy support through a chosen endpoint, or the API when the pipeline should manage sessions by itself. The setup guides for each of these are published openly, which is worth checking before you assume your stack is supported.

## Limits, caveats, and the parts marketing pages skip

- **Coverage is 90+ countries, not 195.** US, UK, Europe and Southeast Asia are well represented. Check your target geography before committing to a volume package.
- **There's no self-serve free trial on the site.** Trial access has historically been arranged through the vendor's community presence rather than a button in the dashboard. If you need to test first, that's an extra step.
- **Per-IP plans don't rotate by themselves.** The address is fixed to your port for its lifetime. Rotation on IP plans is handled through a separate auto-rotation proxy on selected ports.
- **Refund friction shows up in user reviews.** Geekflare's review notes that Trustpilot complaints cluster around people who bought a plan that didn't fit their use case and couldn't recover the spend — not around connection quality.
- **Third-party test numbers exist, and they're decent rather than flawless.** Geekflare's own testing reported 293 of 300 requests completing successfully (97.7%), two hard blocks, and five CAPTCHA challenges that all came from one IP range — resolved by rotating away from it.

Claims like 99.95% uptime, offline-IP replacement within 60 seconds, and 20–30% savings from IP reuse come from 9Proxy's own materials, so treat them as vendor statements rather than measured results.

One more thing about timing: 9Proxy runs rotating promotions rather than one permanent discount. In April 2026 the campaign issued a personal 9% coupon automatically after a first GB order of the month, valid until 30 June 2026 and limited to GB purchases. The Lunar New Year promotion that year applied 8% to regular IP and GB plans with code `LNY2026` and ended on 23 February. Neither is live now — the pattern is what matters. 👉 [See what's currently running on the 9Proxy sign-up page](https://bit.ly/9-Proxy) before paying list price.

## Picking a package without overthinking it

- **Testing one workflow, a handful of accounts:** 100 IPs at $24. Unlimited bandwidth means the experiment can't spiral in cost.
- **Steady scraping with heavy pages:** 1,500 IPs at $126, or 5,000 at $360 if the job is concurrent. Pay-per-IP, ignore the meter.
- **High-rotation, low-payload work:** 5 GB at $15 to start. If requests are small and IP turnover is the strategy, per-GB is cheaper.
- **Both needs at once:** the bundles exist for exactly this — Starter at $30 covers a trial that mixes sticky sessions with rotating traffic.
- **Continuous pipelines that never stop:** enterprise GB tiers, because the traffic doesn't expire and you stop planning around a 180-day deadline.

## Short answers

**Is a private HTTP proxy the same as a VPN?** No. A VPN tunnels all your device traffic to one exit point. A private proxy gives you an IP you can direct specific tools, profiles or scripts through — which is why people run fifty of them at once and only one VPN.

**Does a private proxy make me anonymous?** It removes your real IP from the request and prevents other users from sharing the address. It doesn't hide what you send over an unencrypted connection.

**HTTP or SOCKS5 for browser automation?** HTTP/HTTPS covers standard HTTP clients and most automation frameworks. Reach for SOCKS5 when an application has no proxy support or needs UDP.

**Can I test before buying?** Not through a self-serve trial. Buy the smallest package that proves the workflow — the entry tiers are priced so that this is a reasonable option rather than a gamble.
