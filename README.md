<p align="center">
  <img src="assets/readme/hero.svg" alt="AI Job Search. Pip the courier bird carries an application along a flight path to its destination. The job search that runs on your machine." width="100%">
</p>

<p align="center">
  <a href="https://claude.com"><img src="https://shields.io" alt="Built on Claude Code" height="20"></a>
  &nbsp;
  <a href="LICENSE"><img src="https://shields.io" alt="MIT license" height="20"></a>
  &nbsp;
  <img src="https://shields.io" alt="India Job Market Fork" height="20">
</p>

<p align="center">
  An AI-powered job application framework built on <a href="https://claude.com">Claude Code</a>.<br>
  Forked and actively extended to optimize job discovery, ATS resume tailoring, and<br>
  application workflows for the Indian tech and hiring ecosystem.
</p>

<p align="center">
  <a href="#about-this-fork--roadmap"><b>Fork Roadmap</b></a>
  &nbsp;&nbsp;&nbsp;
  <a href="#quick-start"><b>Quick start</b></a>
  &nbsp;&nbsp;&nbsp;
  <a href="#commands"><b>Commands</b></a>
  &nbsp;&nbsp;&nbsp;
  <a href="#how-apply-works"><b>How /apply works</b></a>
  &nbsp;&nbsp;&nbsp;
  <a href="#customization"><b>Customization</b></a>
  &nbsp;&nbsp;&nbsp;
  <a href="SETUP.md"><b>Setup guide</b></a>
</p>

---

## About This Fork & Roadmap

This repository is an active fork of [`ai-job-search-dev/ai-job-search`](https://github.com) (originally created by [Mads Lorentzen](https://github.com) under the MIT License).

While the upstream project provides a strong local-first job application pipeline, I am actively updating and adapting this codebase to build a streamlined, high-accuracy job application system tailored for **Indian job seekers and global remote roles**:

### Planned & In-Progress Updates
- **Indian Job Board Integrations (`/add-portal`):** Building and configuring custom portal search skills for major Indian hiring platforms (such as Naukri, Wellfound, Instahyre, and LinkedIn India).
- **ATS & Resume Optimization for Indian Recruiters:** Customizing the `/apply` CV-tailoring prompts and LaTeX templates to align with notice-period conventions, CTC/compensation formatting, and skill-weighting used by Indian tech recruiters.
- **Multi-Model & Cost-Optimized Routing:** Adapting the agent workflow to support flexible API endpoints and lower-token evaluation passes during `/scrape` and `/rank`.
- **Automated Application Tracking:** Enhancing `/outcome` tracking to log application stages, recruiter follow-ups, and response rates across multiple portals.

> **Privacy Reminder:** Never commit personal resume data, phone numbers, or API keys to a public fork. Keep sensitive profile files listed in `.gitignore`.

---

> [!NOTE]
> This is an independent open-source project and is not affiliated with, endorsed by, sponsored by, or maintained by Anthropic. Anthropic and Claude Code are referenced only to describe the toolchain this workflow uses.
>
> This project has **no affiliated cryptocurrency, token, or paid sponsorship program**. Anything claiming otherwise is unauthorized and should be treated as a scam. The only ways to support the original project are the Ko-fi link below and contributing on GitHub.

## Does it actually work?

*From the original creator, Mads Lorentzen:*

I'm a geophysicist by training. When my position was cut in late 2025, I built this framework to run my own job search - the same `/scrape`, `/apply`, and `/interview` workflow in this repo, used weekly, on my own career. I was upfront about it with every employer I spoke to, and instead of counting against me, it usually sparked a genuine technical conversation.

<p align="center">
  <img src="assets/readme/letters.svg" alt="69 envelopes for 69 tailored applications. 20 of them light up as first interviews, and one is sealed as the signed contract." width="100%">
</p>

Sixty-nine tailored applications, twenty first interviews, and one signed contract later, I started as an AI engineer in June 2026. People kept asking whether this actually works. It got me hired. Now it's yours.

*The longer version, including the full application funnel, is on [LinkedIn](https://linkedin.com).*

<p align="center">
  <i>Did this save you a Sunday of cover-letter writing? Consider a coffee.<br>
  Did it land you the job? Maybe two.</i> ☕
</p>

<p align="center">
  <a href="https://ko-fi.com">
    <img src="https://ko-fi.com" alt="Buy me a coffee at ko-fi.com" height="40">
  </a>
</p>

## What this is

A structured workflow that turns Claude Code into a full-stack job application assistant. Six commands carry you from an empty profile to a tracked result:

<p align="center">
  <img src="assets/readme/route.svg" alt="The workflow in order: /setup builds your profile, /scrape finds new postings, /rank shortlists the fits, /apply tailors a CV and cover letter, /interview preps every round, /outcome tracks each result." width="100%">
</p>

The core workflow (self-profiling, fit evaluation, and the drafter-reviewer application pipeline) is **language- and country-agnostic**. Job portal search skills live in [community-portals](https://github.com), one folder per job board, owned by the people who use it: copy in the ones for your market, or build your own with `/add-portal`.

The framework encodes career guidance best practices, including structured evaluation criteria, forward-looking cover letter framing, and optional salary benchmarking.

## Quick start

> 🎥 **Prefer to see it in action first?** [The Next New Thing did a hands-on walkthrough](https://youtube.com) of how the workflow is actually used, from setup to a finished application (recorded August 2026 - commands may have evolved since).

### What you need

| | |
|---|---|
| **[Claude Code](https://claude.com)** | The CLI. Requires a Claude Pro/Max/Team subscription or Anthropic API credits (pay-per-token, usually cheaper for occasional use). |
| **Python 3.10+** | For the tools in `tools/`. |
| **[Bun](https://bun.sh)** | Runs the job portal CLI tools you copy in from community-portals. |
| **LaTeX** | A distribution with `lualatex` and `xelatex`: [TeX Live](https://tug.org), [MacTeX](https://tug.org), [TinyTeX](https://yihui.org), or [MiKTeX](https://miktex.org). |
| **pypdf** *(optional)* | `pip install pypdf` for `/apply`'s ATS parseability check. |

<details>
<summary>Fine print: other agents, LaTeX engines, PDF text extraction</summary>

- **Using a different agent tool** (Codex, Antigravity, Gemini CLI)? Start at [`AGENTS.md`](AGENTS.md) - portal search skills you copy into `.agents/skills/` work there out of the box, and [community forks](https://github.com/discussions/78) adapt the full workflow.
- **LaTeX engines.** The CV compiles with `lualatex` (pdflatex often fails on modern MiKTeX installs with `fontawesome5` font-expansion errors); the cover letter compiles with `xelatex` because `cover.cls` requires `fontspec`. If using a minimal TeX install such as TinyTeX or BasicTeX, install the extra packages listed in [SETUP.md](SETUP.md#minimal-tex-install-tinytexbasictex).
- **PDF text extraction.** `pypdf` is BSD-licensed and needs no Poppler. Poppler `pdftotext` remains a fallback (macOS: `brew install poppler`, Debian/Ubuntu: `apt install poppler-utils`, Windows: `choco install poppler`). If both are missing, the check degrades to a visual keyword review.

</details>

### 1. Clone this fork

```bash
git clone https://github.com
cd ai-job-search
```

> [!IMPORTANT]
> **Remember that public forks on GitHub are visible to everyone.** When running `/setup`, ensure your personal details (phone number, address, CV data) are kept in untracked/ignored files so you don't accidentally push private information to GitHub. See [SETUP.md section 8](SETUP.md#8-pulling-upstream-updates-into-your-fork) for pulling upstream updates.

### 2. Add job portals for your market (optional)

AI Job Search ships no job-board CLIs; without one, `/scrape` searches with WebSearch. Pick your boards in [community-portals](https://github.com) (`portals/<country>/`, or `portals/global/` for LinkedIn and multi-market boards), and ask your agent, swapping in your board's folder:

> Copy https://github.com/tree/main/portals/dk/jobindex-search into `.agents/skills/` and show me its code and what it does. Once I confirm, run `python3 tools/install_portal_clis.py` (Windows: `python tools\install_portal_clis.py`).

`/setup` prints this prompt for the boards you name.

<details>
<summary>What the installer does</summary>

It runs `bun install` in every portal CLI under `.agents/skills/*/cli/` - discovered, not listed, so a portal you copy from community-portals or add with `/add-portal` is included on a fresh clone. With no portal installed it does nothing. Every install runs with `--ignore-scripts`, and a CLI whose `package.json` declares an install-time lifecycle script, `trustedDependencies` or `workspaces`, or that has a `bunfig.toml` or `.npmrc` beside it, is skipped and reported rather than run. `--dry-run` shows what it would install.

For zero-dependency portals such as `linkedin-search` and `freehire-search` the install is optional: they run with plain `bun`; `bun install` only pulls TypeScript dev types.

</details>

### 3. Set up your profile

```bash
claude
# Then inside Claude Code:
/setup
```

