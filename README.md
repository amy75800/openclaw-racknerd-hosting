# openclaw hosting: how to run it on RackNerd for $32.49 a year — RAM sizing, Docker setup, and the real monthly cost most reviews skip

OpenClaw hosting gets simple fast once you look at what the software actually does. The gateway that runs on your server is a Node.js process. In the standard cloud-model setup, where you plug in a Claude, GPT, or Gemini API key, the heavy AI work happens on Anthropic's, OpenAI's, or Google's servers, and your machine just routes messages. That's why a $32.49/year VPS from RackNerd — the cheapest annual bill among the eight hosts compared in the research for this piece — can genuinely run it, and why paying more for a "bigger" host usually buys convenience rather than capability.

The trade is real, though. RackNerd has no one-click OpenClaw template, so you'll install Docker and wire things up yourself. If that sounds acceptable, this guide covers what OpenClaw actually needs from a server, which RackNerd plan fits, what the manual setup involves, and the cost line that matters more than hosting: your model API bill.

> **Quick take:** if you're comfortable with SSH and want the lowest total bill, RackNerd's 3.5 GB annual plan at **$32.49/year** (about $2.71/month) is the cheapest verified route to self-hosted OpenClaw. If you never want to see a terminal, managed hosts like MyClaw (from $9/month) or OpenClaw Cloud ($19/month, with a free model included) handle everything for you. 👉 [Check RackNerd's current annual VPS pricing](https://bit.ly/my-rn)

## What OpenClaw actually needs from a host

For the cloud-model setup — which fits roughly 90% of the people self-hosting OpenClaw — the requirements are modest: 2 GB of RAM as a bare minimum, 4 GB as a comfortable floor, and around 50 GB of fast NVMe storage. The gateway barely touches CPU, and months of chat history amount to 100–500 MB on disk.

The RAM details are where cheap plans get sorted. Most tutorials list 2 vCPUs and 2 GB as the minimum, but some setup guides warn that 2 GB can crash during onboarding. That's why the recurring advice lands at 4 GB for one always-on agent with cloud models, and 8 GB once you add browser-based skills, since a headless browser eats memory. Storage speed matters as much as capacity, because OpenClaw keeps memory and context on disk — NVMe beats older drives here in a way that a spec sheet doesn't show.

Runtime requirements are specific. The gateway runs on Linux (or macOS), so you'll want Ubuntu or AlmaLinux with root access. Node 24 is the recommended runtime, though Node 22.16 and later still work, and the installer sets up a systemd service so the agent restarts with the server. One thing that will never work: the shared cPanel web hosting you may already own. It has no root access and can't keep a long-running Node process alive.

There's one scenario where these numbers change completely. Running a local model through Ollama means the model weights load into your RAM — 16 GB minimum, ideally a GPU. A cheap annual VPS is the wrong tool for that job, whichever brand you pick.

## RackNerd plans for OpenClaw: the two tiers that matter

RackNerd's appeal is the bill. Annual pricing puts it below most competitors' monthly rate, and two tiers came out of the June 2026 pricing check worth comparing:

| Plan | RAM | Price | Billing cycle | OpenClaw setup | Get it |
| --- | --- | --- | --- | --- | --- |
| 3.5 GB plan | 3.5 GB | $32.49/yr (≈ $2.71/mo) | Annual, paid up front | Manual (Docker) | [ Order the 3.5 GB plan](https://bit.ly/my-rn) |
| 4 GB plan | 4 GB | $59.99/yr (≈ $5.00/mo) | Annual, paid up front | Manual (Docker) | [ Order the 4 GB plan](https://bit.ly/my-rn) |

Two things to know before picking between them. First, the 3.5 GB tier clears the 2 GB minimum that crashes for some people during onboarding, and it sits just under the "comfortable" 4 GB mark — fine for one or two messaging channels. Second, RackNerd rotates promotional configurations, and both verified prices are annual terms paid up front, so confirm the exact specs and the renewal rate on the order page before paying. 👉 [See today's annual RackNerd deals](https://bit.ly/my-rn)

The missing feature deserves equal billing: there's no one-click OpenClaw template here. That's the entire reason the bill is this low, and the next section explains what you're actually signing up for.

## Setting up OpenClaw on a RackNerd VPS: what "manual Docker" really means

It means roughly 2 to 6 hours the first time, per the comparison data on self-hosting setups, instead of a 60-second managed deploy. If you've ever installed anything on a Linux box, the steps are ordinary:

1. **Order the VPS and pick the OS.** Ubuntu or AlmaLinux, with root access.
2. **Install Docker.** This is the standard self-host path for OpenClaw in cloud-model mode.
3. **Deploy the gateway.** If you skip Docker and install Node directly, use Node 24 (Node 22.16+ also works). Either way, the installer sets up a systemd service, so the agent comes back on its own after a reboot.
4. **Connect a messaging channel.** WhatsApp, Telegram, Discord, Slack, Signal, and iMessage are all supported — you message the agent like a contact, and it browses the web, runs commands, and manages files on your box.
5. **Add your model API key.** This is cloud-model mode, and it's where your real monthly cost lives (more on that below).
6. **Close the doors.** Firewall, SSH keys, and one hard rule about the dashboard port, covered in the security section.

None of these steps is difficult; they're just steps. Hostinger's one-click template and the managed hosts exist precisely because people get stuck somewhere in the middle of this list. Paying $6.49/month or $9+/month essentially buys back those hours, plus someone else's patching and backups. If you enjoy this kind of work even slightly, the paid version adds little.

## What OpenClaw hosting really costs per month

The VPS is the cheapest part of the bill. Here's the annual math across the verified options:

- **RackNerd 3.5 GB:** $32.49/year, about $2.71/month
- **RackNerd 4 GB:** $59.99/year, about $5.00/month
- **Hostinger KVM 1 (4 GB):** $6.49/month on a 24-month term — $77.88 the first year, and renewals run 1.7–1.9× the launch price, so year two lands around $132–148
- **Contabo (8 GB):** about $6.40/month, roughly $76.80/year
- **MyClaw Lite (managed):** $9/month, $108/year
- **Oracle Cloud Always Free:** $0

Look at the spread: the gap between the cheapest and the priciest DIY option is roughly $75–115 a year. Then look at the line item every host leaves out. Managed and DIY routes alike use BYOK — bring your own API keys — and typical model-provider bills run $20–60 a month. One comparison documented users hitting **$3,600 in a single month** from uncontrolled agent loops, which is over a century of RackNerd's annual plan spent in thirty days. Set spending limits on your API account before you connect the key, not after.

The practical conclusion: hosting choice moves a small number. Pick based on how much operations you want to own and how much you trust yourself with the security checklist. If the annual bill is what's driving you, 👉 [lock in RackNerd's annual rate](https://bit.ly/my-rn) while the current promo pricing holds.

## When RackNerd is the wrong pick for OpenClaw

Three situations call for something else, and it's better to know them before ordering.

**Local models.** If you want Ollama running on the same box so your prompts never leave your server, you need 16 GB of RAM at minimum and ideally a GPU. An 8-billion-parameter model at Q4 quantization needs around 8 GB on its own, before the gateway. No budget annual VPS covers this.

**Browser-heavy agents.** Setup guides that use browser-based skills treat 8 GB as the realistic floor. The 4 GB RackNerd tier helps, but the honest fit is an 8 GB machine — Hostinger's KVM 2, for example, ships 2 vCPUs, 8 GB RAM, and 100 GB NVMe for $8.99/month, and Contabo's 8 GB plan costs about $6.40/month.

**Zero tolerance for terminals.** If SSH sounds like a chore rather than a Tuesday evening, managed hosting is worth the multiplier. MyClaw's Lite plan runs $9/month with auto-updates and daily backups, Agent37 offers shared-container hosting at $0.99/month for experimenting, OpenClaw Cloud sits at $19/month with a free model included, and xCloud — the most established managed host — carries the largest community plus a marketplace of 200+ community-built skills. All of them deploy in minutes and take the security defaults off your plate.

## Lock down the server before it goes live

Self-hosting moves the entire security checklist to you, and this is where the cheap DIY route bites people. Researchers found more than 42,000 OpenClaw instances left exposed to the public internet. The single most important rule: never expose OpenClaw's dashboard port to the public internet. Not "limit it," not "password-protect it" — keep it off the open web entirely.

Beyond that, the standard hardening set is short: a firewall (UFW is the usual choice), SSH key-only authentication, fail2ban, and regular updates. Those four items are literally what managed hosts like RunClaw advertise as selling points, which tells you how much they matter. Add an API spending limit to the list as well, for the reasons in the cost section. Backups are the easy part — at 100–500 MB for months of chat history, even a manual copy is trivial.

## Frequently asked questions

**Is 3.5 GB of RAM enough for OpenClaw?**
Yes, for the cloud-model setup with one or two messaging channels. It clears the 2 GB minimum that some guides warn can crash during onboarding, though it sits just under the 4 GB comfortable floor. Browser skills and multiple agents push you toward 8 GB.

**Does RackNerd have a one-click OpenClaw installer?**
No. Deployment is manual Docker, which is the trade for the $32.49/year price. Hostinger is the budget host with a dedicated one-click template if you want the shortcut.

**How much does the AI part cost?**
Separate from hosting. Every route here uses BYOK, so you pay Anthropic, OpenAI, or Google directly — typically $20–60/month depending on usage. Hosts don't mark up inference; they just never see that bill.

**Can I host OpenClaw for free?**
Yes, for testing. Oracle Cloud's Always Free ARM tier includes 12 GB of RAM and runs OpenClaw, though setup on ARM is manual and free-tier allowances change without much warning.

**RackNerd or managed hosting — which one?**
Do the time math honestly. Managed saves you 2–6 hours of setup plus ongoing patching for roughly $75–130 more per year. If those hours are worth less than that to you, self-host on RackNerd; if not, pay for managed.

## The short version before you order

Match the plan to your situation:

- **Lowest possible bill, comfortable with a Docker guide** → RackNerd's 3.5 GB annual plan at $32.49/year, or the 4 GB tier at $59.99/year if you want headroom.
- **Browser skills or several agents** → an 8 GB machine; Contabo at about $6.40/month or Hostinger KVM 2 at $8.99/month fit (mind Hostinger's 1.7–1.9× renewals).
- **Never wants to see a terminal** → managed hosting from $9/month, or Oracle's free tier while you're still deciding whether an always-on agent fits your life.
- **Local models** → none of the above; you need 16 GB+ and a GPU conversation.

Before paying, do two things: confirm the current promo configuration and renewal rate on the order page, since RackNerd rotates its deals, and set a spending limit on your API account before the key ever touches the agent. Then 👉 [order RackNerd's annual plan and have OpenClaw running this weekend](https://bit.ly/my-rn).
