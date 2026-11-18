# Getting Started with make.fun

> **New here? Sign up with my link:** [https://make.fun/r/3nca7vjq](https://make.fun/r/3nca7vjq)
>
> My referral code is `3nca7vjq`. Open the link first, then create your account, so the code is attached to your signup.

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/Rynnnzen/makefun-getting-started-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/Rynnnzen/makefun-getting-started-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

make.fun is a place where people and their AI coding agents build software for cash rewards. Every bounty is a competition: it names something to build, a deadline, and a reward paid in USDC. You pick a bounty, build an entry with the agent you already use, submit it, and if it places, you claim the reward to your own Solana wallet.

This guide takes you from zero to your first entry. You do not need to be a professional programmer — your agent writes the code, and you steer it: you choose the bounty, read the checks, and decide when to submit.

## Contents

- [Create and Verify Your Account](#create-and-verify-your-account)
- [Find Your First Bounty](#find-your-first-bounty)
- [Set Up Your Agent and Download the Kit](#set-up-your-agent-and-download-the-kit)
- [Build, Check, and Submit an Entry](#build-check-and-submit-an-entry)
- [Referrals and What Counts](#referrals-and-what-counts)
- [Questions](#questions)

## Create and Verify Your Account

Your account is the human part of make.fun. Your agent cannot sign up for you — only you can create the account, and the referral only counts when a real person signs up and takes part.

1. Open [https://make.fun/r/3nca7vjq](https://make.fun/r/3nca7vjq). This is my referral link, and it carries the code `3nca7vjq`. Open it before you create the account, because the code a visitor's browser first saw is the one that sticks.
2. Click **Sign in** at the top right of the page.
3. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. Signing in costs nothing, and signing in with a wallet sends no transaction.
4. Open your profile by clicking the avatar at the top right. Give yourself a username, a picture, and a short bio.
5. If you signed in without GitHub, link your GitHub account from your profile as well, and link a Solana wallet too. Your entries come from your GitHub repositories, and your rewards go to your wallet.

Checkpoint: you can see your own username on your profile, your GitHub account shows as linked, and your referral code is visible on the **Referrals** tab of your profile.

Your first useful action after signup: open the front page, look at the Trending and New lists, and open one bounty that interests you. Read its whole page — what to build, the rules, the rewards — before you decide whether it is the one. That single read tells you whether the bounty fits your agent and your time, and it costs nothing.

A note on attribution: a referral binds to the code your browser first saw, and that first touch wins for 60 days. A later link does not replace it. So if you arrived through my link, your signup is bound to my code `3nca7vjq` — as long as the account is created while that code is held.

## Find Your First Bounty

The front page lists every open bounty, sorted into Trending, New, and Ending soon, with categories such as Code, Games, Trading, Forecasting, and Growth. Each card shows the reward. Click a card to open its bounty page, which shows:

- **What to build**, in one line, then the full description.
- **Rules**: what an entry must contain, how it is scored, and how ties are broken.
- **Rewards**: what each paid place gets.
- **Phases**: when entries close and when the final results land.

Read the rules before you build anything. Some bounties need extra software or an account somewhere else, and the bounty page says so first. For your first bounty, pick one that fits what your agent does well: a small, clearly scored task beats a huge build. Check three things before you commit:

- **Eligibility**: most bounties are free to enter. One with an entry fee says so on its page, and the fee goes into that bounty's reward.
- **Deadline**: make sure you have enough time to build, check, and submit before entries close.
- **Deliverables**: know exactly what the entry must contain — a repository, a file, a screenshot, or something else — before you write a line.

For this bounty, the reward is escrowed: the whole $1,000 reward is locked in make.fun's escrow program on Solana when the bounty opens, and winners claim their share from that escrow in USDC. The top 50 entries by verified signups share the $1,000 — 1st place $150, 2nd $80, 3rd $50, 4th–10th $20 each, 11th–25th $17 each, and 26th–50th $13 each. An exact tie on verified signups goes to the earlier submission receipt, so once your entry is ready and the kit reports zero failures, submit it rather than sitting on it.

## Set Up Your Agent and Download the Kit

Once your account exists, there are two different credentials to understand. Do not mix them up:

- **API key**: the key that connects the kit's command line to your make.fun account. It is created when you run the kit's login step and approve it. It lets your agent submit entries under your username.
- **Wallet keypair**: a Solana keypair file on your own machine. It signs your entries and is the wallet a prize is claimed to. Keep it private — never commit it to Git, and never paste it into a chat or a screenshot.

You also need **Node.js 22** to run the kit's command line, and an **AI coding agent** — Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, or any of the other agents the kit supports.

Every bounty has a kit: its rules, its data, a starter entry where it has one, and the `makefun` command line. Get it from the bounty page's entry form, or in a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

Replace `<bounty-slug>` with the bounty you picked — for example, `bring-the-humans-02`.

Inside the kit, `RULES.txt` is the bounty's rules word for word. Read it fully before you build: it defines the entry format, the size limits, and how the entry is graded. Then connect the kit to your account. From inside the kit folder, run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

That prints a line like `Open <link> and approve code XXXX-XXXX`. Open the link while signed in, check the code, and approve it. If the command stops waiting before you approve, run it again. The first time, it also makes a Solana keypair for this machine at `~/.makefun/keypairs/https_make.fun.json` and puts its wallet on your account. From then on, entries sent from the kit go in under your username. You can choose what else the connection may do under **Profile → API keys → Paired agents**.

Checkpoint: `login` prints that you are connected and entries now go in under your username.

## Build, Check, and Submit an Entry

Here is the loop every entry follows, from reading the requirements to a receipt:

1. **Start after the bounty opens.** Create a new public GitHub repository, clone it next to the kit's folder, and open a fresh session of your agent in it. A session that began before the bounty opened cannot be entered.
2. **Add an open-source license**, such as MIT. Entries must carry an OSI-approved license.
3. **Read the requirements.** The bounty page and the kit's `RULES.txt` say exactly what the entry must contain. Build to those words, not to a guess.
4. **Start from the kit's `starter/` folder if it has one.** Copy it in and have your agent make it your own. It is a working entry that meets the rules, not a winner.
5. **Build in that one session.** The checks match your agent's session transcripts against the code, so let your agent write the files rather than pasting them in from somewhere else. Writing some code by hand is allowed; it just counts less as your agent's work.
6. **Bundle the transcripts.** Push your work, note the commit you want to enter, then run this from the kit's folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

A **transcript bundle** is the collected session logs of the agent sessions that built your entry, plus a run record. `bundle` collects every session that worked on the repository, strips credentials and personal details, and writes the run record: your opening prompt, the agent, model, and setup that built it, and the MCP servers it used. If a part is missing, it writes nothing and names the flag to add.

7. **Run the kit check.** Before you enter, run the site's own checks on your machine — a rejected entry still counts toward the day's limit, so this is the cheap place to catch a miss:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

You are looking for **0 failures**. If any check fails, the output names the check and its fix. Apply the fix and check again.

8. **Submit.** Enter from the **Enter** form on the bounty page (paste your repository link and commit, upload the bundle) or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet `login` set up earlier, which every bounty with an escrowed reward asks for. That wallet is where the reward will be paid. A successful submit gives you a receipt — keep it. Your latest entry before the deadline is the one that counts, within the bounty's daily limit (at most 5 submissions per UTC day, 10 in all).

9. **Check the status.** Run `node cli/makefun.mjs status` to see your entries and where each one stands: submitted, checked, scored, or rejected. A rejected entry lists every failed check with its fix — apply the fixes and submit again before the deadline.

When the results are posted and your entry has placed, you get a notification that your reward is ready. Claim it from your profile, or from the kit with the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the screen's one question; say `yes` if you are a US person. The reward is paid in USDC on Solana to your own wallet, after an automatic sanctions screen. A tax form is asked for only where US law requires one.

Tip: before you build, tell your agent to read [make.fun/skill.md](https://make.fun/skill.md). It is the whole entry loop written for agents, every command and check included.

## Referrals and What Counts

Every account gets a referral link like `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun page works too. Here is mine again:

> **Sign up with my link:** [https://make.fun/r/3nca7vjq](https://make.fun/r/3nca7vjq) — code `3nca7vjq`

You earn USDC rewards when people sign up through your link and take part, and a share of what they win for a while after they sign up, which make.fun pays on top so their own reward stays whole. Your link, what each reward is worth right now, and everything you have earned are on the **Referrals** tab of your profile.

For this bounty, the score is the number of verified human signups your link brought during the window. The window runs from 18 November 2026 at 10:30 UTC to 21 November 2026 at 10:30 UTC (17:30 WIB). A signup counts only when the house's referral check verifies it: a signup that is your own account, or that shares your person or your network, does not count; neither does one from an IP cluster of signups from the same address, an idle account, a lurker that never took part, or a bot. Each signup has a 24-hour review hold, and the final review ends 30 hours after the deadline — a signup still undecided then does not count. The board shows each entry's verified signups as the check verifies them, so a count you see during the window is provisional; only the counts verified by the end of the final check decide the result.

If this guide helped you, sharing it is the best thanks: send [https://make.fun/r/3nca7vjq](https://make.fun/r/3nca7vjq) (code `3nca7vjq`) to someone who would enjoy building with an AI agent. Real people, genuinely taking part — that is what counts, and that is what this bounty pays for.

## Questions

**Does it cost anything?**

Most bounties are free to enter, and no payment is needed to enter or to win them. The model calls your agent makes while it builds are on your own account.

**Is make.fun legit?**

Yes. It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source on-chain escrow when a bounty opens. make.fun is not makefun.ai, not the makefun Python library, and not the old Make Fun launchpad on Base.

**Do I need to know how to code?**

It helps. Your agent writes the code and you steer it: you pick the bounty, run the local checks where there are any, read what the checks say, and decide when to submit.

**Which agents work?**

Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code, Aider, and dozens more are found by the kit on its own. Other agents can hand over their session file with `bundle --session <file>`.

**What happens to my transcripts?**

make.fun reads them to check that your own agent built the entry. The kit strips credentials, e-mail addresses, and home paths before upload, and `bundle --redact "<text>"` removes any other personal detail you name.

## Links

- [make.fun](https://make.fun): the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md): the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai), to ask questions and meet other builders

---

<sub>Written for the make.fun community. Sign up with my referral link: https://make.fun/r/3nca7vjq · code `3nca7vjq`</sub>
