# Evoxt Frankfurt: how fast is the DE-CIX node, what does $2.99/month actually include, and who should skip it? (Full plan table, measured latency, port 25 limit)

A Frankfurt VM from Evoxt starts at $2.99/month for 1 vCPU, 512 MB RAM, 5 GB NVMe and 500 GB of transfer on a 1 Gbps port, and every tier, including the cheapest one, ships with a weekly offsite backup. The node peers at DE-CIX, and a test box running there in November 2025 measured 0.90 ms to a Frankfurt speedtest node, 8.84 ms to Amsterdam, 16.80 ms to London, with roughly 900 Mbps up and down on each of those runs.

That covers the two questions most people type into a search bar. The rest of this page handles the ones that decide the purchase: the full eleven-plan price list, why two testers on the same $2.99 tier got single-core scores 60% apart, the blocked port that rules out self-hosted mail, China routing that works on Unicom and fails on Telecom, and the cases where a German incumbent is still the smarter buy.

> **Quick pick.** A blog, a proxy endpoint, a small API or a monitoring box fits in VM-0.5 at $2.99/month with room to spare. If you need 2 GB of RAM for Postgres, a build agent or a handful of containers, VM-1 at $5.99/month is the next stop worth making. Very few readers of this page need anything above VM-2.
> 👉 [Compare the Frankfurt plans and start a deployment](https://bit.ly/EvoXt)

## Every plan on the Frankfurt list

All eleven plans run on the same platform: KVM virtualisation, NVMe storage, a 1 Gbps port, one IPv4 plus a /64 IPv6 block, weekly offsite backups, and a Layer 3 firewall you manage from the control panel. Prices are the monthly rates published on Evoxt's pricing page. Frankfurt belongs to the Standard network tier, which carries the largest transfer allowance of the three tiers Evoxt sells.

| Plan | vCPU | RAM | Storage / Transfer | Price (USD/mo) | Order |
| --- | --- | --- | --- | --- | --- |
| VM-0.5 | 1 | 512 MB | 5 GB / 500 GB | $2.99 | [ Deploy](https://bit.ly/EvoXt) |
| VM-0.75 | 1 | 1 GB | 10 GB / 750 GB | $4.99 | [ Deploy](https://bit.ly/EvoXt) |
| VM-1 | 1 | 2 GB | 20 GB / 1 TB | $5.99 | [ Deploy](https://bit.ly/EvoXt) |
| VM-1.5 | 2 | 2 GB | 20 GB / 1.5 TB | $6.95 | [ Deploy](https://bit.ly/EvoXt) |
| VM-2 | 2 | 4 GB | 30 GB / 2 TB | $11.99 | [ Deploy](https://bit.ly/EvoXt) |
| VM-3 | 4 | 4 GB | 30 GB / 3 TB | $14.99 | [ Deploy](https://bit.ly/EvoXt) |
| VM-4 | 4 | 8 GB | 60 GB / 4 TB | $23.99 | [ Deploy](https://bit.ly/EvoXt) |
| VM-6 | 8 | 8 GB | 60 GB / 5 TB | $29.99 | [ Deploy](https://bit.ly/EvoXt) |
| VM-8 | 8 | 16 GB | 80 GB / 6 TB | $47.99 | [ Deploy](https://bit.ly/EvoXt) |
| VM-12 | 16 | 16 GB | 80 GB / 8 TB | $60.95 | [ Deploy](https://bit.ly/EvoXt) |
| VM-16 | 16 | 32 GB | 100 GB / 10 TB | $95.99 | [ Deploy](https://bit.ly/EvoXt) |

Two things in that table matter more than the spec columns.

The transfer figure counts inbound and outbound together. VM-0.5's 500 GB is not 500 GB of egress; uploads and downloads share the same bucket. For a web front end serving static files, that is fine. For a box that pulls large datasets down from other providers every night, do the arithmetic before you assume the cheap plan covers it. Evoxt does not charge overage fees on top, which is the one mercy here.

The same plan names cost the same money in Hong Kong, Osaka and Malaysia Premium, but carry less transfer: VM-1 gets 500 GB there against Frankfurt's 1 TB, and VM-16 drops from 10 TB to 5 TB. If throughput per dollar is what you are shopping for, Frankfurt is the strongest of the three tiers by a wide margin.

### Does prepaying help?

Evoxt lists two prepaid terms, 5% off for six months and 10% off for twelve, with terms up to three years offered. Here is what that does to the four plans most people consider:

| Plan | Monthly | 6 months (−5%) | 12 months (−10%) |
| --- | --- | --- | --- |
| VM-0.5 | $2.99 | $2.84/mo | $2.69/mo |
| VM-1 | $5.99 | $5.69/mo | $5.39/mo |
| VM-2 | $11.99 | $11.39/mo | $10.79/mo |
| VM-4 | $23.99 | $22.79/mo | $21.59/mo |

On VM-0.5 the annual prepay saves about $3.60 across the year. That is a rounding error against the risk of committing twelve months to a provider you have not tried yet, so start monthly and switch the term on your next invoice if the box behaves. The discount gets meaningful at VM-4 and above, where the twelve-month term saves nearly $29 a year.

## What DE-CIX buys you, in milliseconds

DE-CIX is one of the largest internet exchanges in the world, and published figures put its annual traffic near 48 exabytes across several hundred connected networks. Evoxt peers there and buys transit from multiple Tier 1 providers, so Frankfurt-bound traffic from most of Europe stays inside a handful of hops.

Measured from a $2.99 VM-0.5 in November 2025:

| Destination | Latency | Download | Upload |
| --- | --- | --- | --- |
| Frankfurt, DE | 0.90 ms | 918 Mbps | 923 Mbps |
| Amsterdam, NL | 8.84 ms | 940 Mbps | 930 Mbps |
| London, UK | 16.80 ms | 882 Mbps | 886 Mbps |
| Paris, FR | 17.48 ms | 823 Mbps | 909 Mbps |
| Warsaw, PL | 27.68 ms | 919 Mbps | 905 Mbps |
| Bucharest, RO | 38.34 ms | 935 Mbps | 904 Mbps |
| New York, US | 80.06 ms | 936 Mbps | 867 Mbps |
| Los Angeles, US | 141.28 ms | 915 Mbps | 472 Mbps |
| Singapore, SG | 176.59 ms | 941 Mbps | 400 Mbps |
| Tokyo, JP | 263.14 ms | 909 Mbps | 306 Mbps |

An iPerf3 run against a local Frankfurt node gave 915/917 Mbps at 1.293 ms, so the 1 Gbps port is real and not a shared best case. The global average across all speedtest targets came out at 866 Mbps down and 678 Mbps up.

One caveat on residency. The IP range 23.27.124.0/24 is announced by AS149440 Evoxt Sdn. Bhd. and geolocates to Frankfurt am Main with data center usage type, so your data physically sits in Germany. The company itself is Malaysian. That distinction matters if your compliance paperwork asks who the processor is, not just where the disk is. Ask support for a data processing agreement before you sign anything that requires one.

## The benchmark numbers disagree, and that is the useful part

Evoxt's marketing leads with 6.0 GHz Turbo, and the site notes that the comparison figures behind it were collected in 2020 and may change. Real hardware reports something lower. Two tests of the cheapest tier, run within days of each other in November 2025, produced this:

| Test | CPU | Geekbench 5 single / multi | Notes |
| --- | --- | --- | --- |
| Tester A (Nov 14) | AMD EPYC-Milan @ 3593 MHz | 1130 / 1179 | 5 GB disk too small for the full benchmark suite |
| Tester B | AMD EPYC Genoa @ 4.19 GHz | 1818 / 1798 | Sysbench single-core 5901; memory 70,976 / 40,431 MB/s |

Same plan, same price, 60% apart on single-thread work. Host CPU generation and neighbour load explain most of it, which means the number you get is partly luck of the draw on deployment day. Budget for something between 1,100 and 1,800 on Geekbench 5 single-core rather than the turbo figure on the homepage.

For a step up, a third-party write-up of VM-1 reports Geekbench 6 single-core above 1,300 and average disk throughput around 44,522 IOPS, graded A. Absolute IOPS numbers from any single test deserve skepticism, but nothing in the three sets of results suggests the storage is the weak link.

Streaming and AI endpoints resolved cleanly on an IPv4 Frankfurt address in the November test: Netflix (German catalogue), Disney+, Amazon Prime, TikTok and ChatGPT all returned positives. That is a snapshot, not a promise, since these results move with IP reputation. The address itself looked clean, with a fraud score of 3 and no abuse listings.

The 5 GB disk on VM-0.5 is the practical constraint nobody mentions. Tester A's benchmark suite crashed outright because there was not enough room for its temp files. Debian plus Docker plus logs will fit if you are disciplined, but a Nextcloud install, a Postgres dataset or a Node build cache will not.

## The limits that decide whether you buy

**Port 25 is blocked on Frankfurt IPs.** You cannot run a self-hosted mail server and expect outbound mail to land; the documented workaround is a transactional email API such as Postmark or SES. If outbound mail is central to your project, stop reading and pick a provider that permits it, because no amount of tuning gets around a filtered port.

**China Telecom users should look elsewhere.** Routes are standard international BGP via Tata with no CN2 or GIA optimisation. China Unicom gets a clean AS4837 direct path at around 164 ms, and premium 9929 test nodes came in at 131 ms from Beijing and 168 ms from Shanghai. Telecom traffic detours through the US on Cogent at roughly 249 ms, and a Shanghai CN2 node averaged under 3 Mbps download in testing. Mobile sits in between at about 236 ms via Singapore and Marseille. The all-probe average lands near 218 ms.

**Support is competent on routine tickets and thin on edge cases.** VPSBenchmarks ranked Evoxt second in its "Best VPS under $25" list for 2025 and third in its December 2025 "Best VPS" roundup, with decent marks for deployment consistency. Trustpilot, on a small sample, averages four stars. A recent Reddit thread documents a refund dispute over a Headscale/Tailscale coordination setup that did not go smoothly.

**One independent reviewer did not like it at all.** A November 2025 test of the $2.99 box concluded: "A European VPS without many highlights. It is not very competitive in Europe, has average value for money, and is not really worth buying." Read that in context. The reviewer tested the lowest tier, on one machine, on one day, and explicitly flagged the results as time-limited. It is still the most useful counterweight to the affiliate write-ups, and it is right about one thing: if you only care about price per gigabyte of RAM inside Germany, this is not the winning offer.

If none of those four items touches your workload, the platform underneath is solid. Deployment takes about 2.5 minutes for Linux and roughly 15 for Windows or one-click apps, with credentials mailed to you. The control panel includes live monitoring, browser VNC, firewall rules, IP swap and reassignment, VM cloning, sub-accounts split across admin, technical, billing and support roles, and a full REST API. Changing region or IP after deployment is free through a support ticket. The published SLA is 99.99%, which works out to about 4 minutes 19 seconds of permitted downtime in a 30-day month.

👉 [Deploy a Frankfurt VM and check the panel yourself](https://bit.ly/EvoXt)

## Add-ons, payment, and what a realistic monthly bill looks like

Evoxt sells upgrades à la carte rather than forcing a jump to the next plan:

- Extra IPv4 address: $3/month, up to 16 per account
- Extra vCore: $3/month
- Extra RAM: $2/month per GB
- Extra transfer: $3/TB on Standard, $12/TB on Premium, $24/TB on Premium Plus
- Paid backup plans: priced by VM storage size, adding daily snapshots, manual backups and five rotation copies on top of the included weekly copy

Do the maths on a concrete case. VM-1 with 2 GB, 1 TB of transfer and one core costs $5.99. Add 1 GB of RAM and another terabyte of transfer and you are at $10.99. VM-2 costs $11.99 and gives you 4 GB, 2 TB and two cores for a dollar more. So the add-on path wins when you need one dimension nudged, and the plan jump wins as soon as you need cores and transfer together.

Payment methods cover credit and debit cards, PayPal, Bitcoin (no fee, roughly an hour to confirm), USDT on Tron (no fee), Alipay, and account credit. The site also lists Litecoin and Ethereum. Registration typically asks for a name and little else.

On discounts at checkout: there is a promo code field in the order flow, one code per order, and affiliate codes generally do not stack with other offers. I am not going to hand you a code and a discounted price, because the ones circulating in forums and affiliate posts for this brand contradict each other and none of them could be confirmed against Evoxt's own checkout. The prepaid terms above are the only discount structure I could verify.

Refund terms deserve the same honesty. Plan-tracking data compiled from Evoxt's pages lists a 7-day window for new customers, counted from first deployment. I could not confirm that wording on Evoxt's own site, so treat seven days as the number to verify with support before you pay, and do not buy an annual term on the assumption that a longer window exists.

## Evoxt against the German incumbents

| Provider | Entry point | Where it wins | Trade-off |
| --- | --- | --- | --- |
| Hetzner | CX/CPX shared vCPU from about €3.49 | German company, local support, aggressive pricing | Shared vCPU, peak single-thread below a dedicated high-clock core, roughly 2.4–3.5 GHz |
| Netcup | Similar low-price bracket | Cheap European capacity | Comparable position to Hetzner on price, less differentiated on hardware |
| Contabo | Low price for large RAM and disk figures | Paper specs per dollar | Reliability and support reputation is mixed, with instability complaints in community threads |
| OVHcloud | Multiple European locations, anti-DDoS and game-optimised VPS lines | DDoS handling, location spread | Tiered pricing is harder to compare across lines |
| DigitalOcean, AWS, Azure, Google Cloud | Frankfurt regions on all four | Managed services, ecosystem, compliance tooling | Cloud-tier pricing with bandwidth metered separately, so egress bills grow fast |

The split is straightforward. If your project lives entirely in Germany, sells to German customers and price is the deciding variable, Hetzner is the rational default and Evoxt will not beat it on cost per gigabyte. If you want a high-clock core for a single-threaded workload, free weekly offsite backups on a $2.99 plan, the same provider in a dozen regions as you expand, and the option to pay in Bitcoin, Evoxt's Frankfurt node covers all of that without a bandwidth surcharge.

## Questions that come up before ordering

**What latency should I expect in Europe?** Under 1 ms inside Frankfurt, 8–9 ms to Amsterdam, 16–17 ms to London and Paris, 27–38 ms across Central and Eastern Europe. Those are measured figures from a Frankfurt box, not estimates.

**Can the $2.99 plan run a real website?** Yes for a static site, a small WordPress install with a caching layer, a WireGuard endpoint or a monitoring agent. No for anything with a database that grows, or for a workload that needs more than a few gigabytes of local disk.

**Can I send email from it?** Not from port 25 on a Frankfurt IP. Use a transactional email API for outbound mail and keep the VPS for everything else.

**Does it unlock streaming and AI services?** A November 2025 test on a Frankfurt IPv4 address unlocked Netflix, Disney+, Prime, TikTok and ChatGPT. IP reputation changes, so treat this as a recent snapshot.

**Will the price change when I renew?** The pricing page lists one monthly rate per plan, and prepaid terms are published as discounts off that rate. I found nothing confirming or denying a renewal increase either way, so confirm the renewal rate in the checkout summary before you commit to a long term.

**Is it fast from China?** Unicom, yes: AS4837 direct at around 164 ms, better on premium 9929 routes. Telecom, no: roughly 249 ms routed through the US, with poor throughput on CN2 test nodes.

## Pick by scenario

A personal blog, a proxy node, a cron-driven script or a small API: VM-0.5, $2.99/month, and upgrade later if disk pressure appears.

A 2 GB workload such as a small Postgres instance, a CI runner or a handful of Docker services: VM-1 at $5.99, with an extra gigabyte of RAM at $2/month if you outgrow it before you outgrow the transfer allowance.

A database or application where single-thread speed drives response times: skip the entry tier and take VM-2 or VM-3, since the two-core plans give you both clock and headroom.

Anything that sends mail, serves China Telecom users, or needs more than a few gigabytes of fast local disk: none of these plans, regardless of price.

Before you pay, confirm two things in writing with support: that port 25 is still blocked on the Frankfurt range your order lands on, and what the refund window actually is for a new account. Then deploy monthly first, test the box for a week, and move to the twelve-month term only once the latency and the disk have proven themselves.

👉 [Start with VM-0.5 in Frankfurt and upgrade later if you need to](https://bit.ly/EvoXt)
