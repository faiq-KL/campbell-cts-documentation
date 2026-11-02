# Flow A: Release code changes

Every card in this flow, in full. The [plan page](https://faiq-kl.github.io/campbell-cts-documentation/vm-to-webapp-plan.html) shows the diagram.

## The lead and AI coding sessions

*People + AI*

Where code changes come from.

- `orchestrator preview-sites`: private preview of the pages, never publishes
- `npm run deploy (HCG-BIM-Portal)`: puts the portal live from a computer, skipping GitHub
- `hcg-bim-assistant/.claude/agents`: 8 helper roles for AI coding sessions
- `TASKS.md, PROGRESS.md, *_KICKOFF.md`: build plan, work log and start notes for AI coding sessions

Not explored yet.

## The bot's saved data

*Bot, On the VM*

Twice a day the bot saves fresh data to 7 page repos' main branch, which puts it live.

- `hcg-bim-runner`: the bot's GitHub account

Not explored yet.

## Who may sign in

*Repo config, In git*

Lists of who may see each page. A script run by hand copies them to Cloudflare.

- `HCG-BIM-Portal/config/access.json`: copied with tools/sync_access.py
- `Nova102-Action-Hub/config/people.json`: the Action Hub's list

Not explored yet.

## Claude company account

*Claude, Claude account*

AI skills and job instructions are not updated from GitHub.

- `Skills and plugins`: uploaded by hand ("DC Publish")
- `conduit-marketplace`: synced by Cowork (Claude's skill sharing)
- `Scheduled task instructions`: edited in the Claude app

Not explored yet.

## GitHub

*Service, Outside*

Where all 15 repos live.

- `Company account`: owns the repos
- `Most page repos`: no branches: changes go straight to main, which is live
- `Repos with branches and pull requests`: conduit, conduit-marketplace, the rule book, campbellcts-site, hcg-bim-assistant
- `hcg-bim-assistant`: main branch is called master

Not explored yet.

## Cloudflare builds

*Service, Outside*

Puts each page repo's main branch live automatically, and enforces sign-in.

- `Cloudflare Workers Builds + Access`

Not explored yet.

## Netlify

*Service, Outside*

Puts the company website live from its main branch.

- `campbellcts-site`

Not explored yet.

## The VM's own copies of the code

*On the VM, On the VM*

The code the bot really runs. Some bot fixes were made here and pushed to GitHub as hcg-bim-runner. How other changes arrive is unknown.

- `hcg-bim-assistant checkout`: C:\Users\Administrator\repos\hcg-bim-assistant, plus one file not in GitHub (navis_export_clash_xml.py)
- `Conduit servers`: installed programs in AppData\Local\Conduit (Revit, point cloud, fabrication)
- `Navisworks bridge`: a local build in C:\Users\Administrator\mcp\navisworks, with changes not in any repo
- `HCGBIM.OpenGuard add-in`: the bot's Revit add-in
- `Copies of the page repos`: in C:\Users\Administrator\repos and C:\HCGBIM\publish. The bot pushes from these

Not explored yet.

## Live pages and website

*Live*

What the team, BES, Triad, leadership and customers see.

Not explored yet.

## A dev copy of the VM

*Dev copy, Not in git*

A test copy of the VM exists. Its notes are in the Claude Project, and we only have their names.

- `Dev copy runbook`
- `Notes on creating the dev copy`
- `cts_dev_clone_userdata.ps1`: start-up script for the copy
- `Notes on the dev account`
- `Notes on access to the dev copy`
- `Prompt for the dev copy desktop`
- `Sandbox notes for a teammate`

Not explored yet.
