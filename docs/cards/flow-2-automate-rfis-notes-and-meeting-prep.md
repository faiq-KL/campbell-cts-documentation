# Flow 2: Automate RFIs, notes and meeting prep

Every card in this flow, in full. The [plan page](https://faiq-kl.github.io/campbell-cts-documentation/vm-to-webapp-plan.html) shows the diagram.

## The rule book

*Repo, In git*

Every HCG procedure in plain English. The lead's rulings are written here first, then the skills are rebuilt from it.

- `Rule book`: the repo

**Today:** Every HCG procedure in plain English, in its own repo. kb.py builds the ClickUp workbooks and flowcharts. Its plugin command rebuilds the 13 playbook skills by hand, from a starter folder that isn't in git, and the lead uploads them.

**What could change:** The rule book stays in git. The skills are built from it. Unchanged rule book. The webapp loads the skills from git. Whether they rebuild by themselves when the rule book changes is still open.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Rule book | Procedures and the lead's rulings. | GitHub | No change needed. | GitHub |
| kb.py build | Run by hand: builds the ClickUp workbooks and flowcharts. | By hand | No change needed. | By hand |
| Skills rebuild (kb.py plugin) | By hand, then uploaded by the lead. | By hand | Suggested: rebuilt from the rule book whenever it changes, once the starter folder is in git. | To explore |

## Jobs that change project files

*Schedulers ×3, Claude account*

Timers that start the AI on these jobs. They write to real project files.

- `NOVA102 daily RFI & submittal sweep (VM)` (Weekdays 08:24): Checks Procore for new questions (RFIs), approvals (submittals) and reports, and files each into the right ACC folder. Asks Procore for cover PDFs, its only action there. When ARCO posts a new schedule, it loads the dates (after a backup), re-links the work packages and updates the slip-history workbook. If ARCO renumbered activities, it also updates the Nova102-Tracker page. Its notes feed the bot.
- `NOVA102 Project Memory update + flags (VM)` (Weekdays 10:45): Reads email, Teams, meeting notes and Procore, then updates the project diary and "flags" (things the lead may be missing).
- `VKU BIM Assistant shadow + run summary email (VM)` (:25 past each hour after a VKU run): Checks the VKU run, writes VKU's change log and emails the lead a summary.

**Today:** Three timed AI jobs in the lead's Claude account, working through the VM.

**What could change:** All three run in the webapp with their AI, the same instructions and the same timing, through official APIs. The webapp's timer starts them. They use Procore's, Autodesk's, Microsoft's and Fireflies' APIs and write to the webapp's notes. Each one goes live after a side-by-side check against today.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| RFI and submittal sweep (weekdays 08:24) | Procore through Chrome with the deputy's login, where its only action is asking for cover PDFs. Files into ACC, loads ARCO's schedule updates, writes the sweep note, emails the lead and the deputy. | Claude + VM | Procore's and Autodesk's APIs. The same note and email. | Webapp |
| Project Memory and flags (weekdays 10:45) | Outlook, Teams, Fireflies and Procore, then the memory files in ACC and the project. | Claude + VM | Microsoft's, Fireflies' and Procore's APIs. The memory files still go to ACC. | Webapp |
| VKU check (:25 in the run windows) | Checks VKU's run in Newtron's ACC, appends VKU's change log, emails the lead. | Claude + VM | Reads the run in the webapp and ACC through the API. | Webapp |

**Watch out:**

- Today, the memory job's instructions still use the built-in browser, which lost its Procore sign-in. It may be writing 'Procore not read' every day.

## Jobs that write notes

*Schedulers ×2, Claude account*

Timers for two jobs that write notes and send emails.

- `NOVA102 change log from the BIM Assistant run log (VM)` (:20 past each hour after a run): Copies the bot's run summary into the day's note.
- `Meeting prep + follow-up (all of the lead's meetings) (VM)` (Weekdays, :45 past each hour, 06:45 to 17:45): Emails a prep outline before each meeting. After a recorded meeting, emails a summary with proposed Forma issues for the lead to approve (it never creates them).

**Today:** The change log writes each run's summary into the day's note. Meeting prep emails an outline before each of the lead's meetings and a follow-up with proposed issues after recorded ones.

**What could change:** Both run in the webapp with their AI and write to the webapp's notes. The same jobs, timing, notes and emails.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Change log (:20, 10:20 to 12:20 and 17:20 to 19:20) | Reads the run log, the bot's database, today's log and the VM's timer, writes into the Claude Project. | Claude + VM | Reads the bot's status in the webapp, writes the same note to the webapp's notes. | Webapp |
| Meeting prep and follow-up (weekdays, hourly 06:45 to 17:45) | Calendar, Outlook, Teams, Fireflies and project notes. Emails the lead, plus the deputy when they attend. | Claude + VM | Microsoft's and Fireflies' APIs. The same emails. | Webapp |

## Watchers

*Schedulers ×3, Claude account*

Timers for three checks. They raise an alarm by email or phone push if something failed (Flow 4).

- `NOVA102 RFI sweep watchdog (VM)` (Weekdays 09:05): Did the sweep run and send its email?
- `NOVA102 BIM Assistant run watchdog (VM)` (11:40 and 17:40): Did the bot finish and publish?
- `BIM runner VM heartbeat (cloud)` (12:35 and 19:35): Runs in Claude's cloud: are the VM's jobs still running?

**Today:** The sweep watchdog (09:05) and the run watchdog (11:40, 17:40) work through the VM. The heartbeat (12:35, 19:35) runs in Claude's cloud and alerts the lead by push and email.

**What could change:** The two watchdogs run in the webapp. The heartbeat must stay outside the webapp. The same checks, in the webapp. The heartbeat keeps watching from outside.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Sweep watchdog (09:05) | Checks the sweep's run, files and note on the VM. | Claude + VM | The same checks, in the webapp. | Webapp |
| Run watchdog (11:40 and 17:40) | The run log, the portal's GitHub history, the VM's timer. | Claude + VM | The same, with the webapp's schedule in place of the timer. | Webapp |
| Heartbeat (12:35 and 19:35) | In Claude's cloud, checking the VM's jobs. | Claude cloud | Suggested: a small task in the lead's Claude account that checks the webapp's status page, with the same push and email. | To explore |

**Watch out:**

- A watcher must never live on the thing it watches (the lesson of the VM outage).

## What the AI reads

*Service, Outside*

All read only.

- `Procore`: ARCO's website: new questions, approvals, reports, schedules. Signed in as the deputy, through the VM's Chrome (sweep) or the Claude app's browser (memory job)
- `Microsoft 365 · Teams · Fireflies`: The lead's email, Teams chats and recorded meeting notes (Fireflies never used for VKU)
- `Google Drive`: connected, but not seen in use
- `conduit tools`: the Navisworks model
- `GitHub`: checks pages were published (hcg-bim-runner account)
- `The bot's run log`: in ACC (Flow 1), read by the change-log job and the watchers
- `Claude's task list`: the watchers check the other jobs ran

**Today:** Procore (the deputy's login in Chrome), Microsoft 365 and Fireflies (Claude's connectors in the lead's account), the run log in ACC, GitHub (hcg-bim-runner), Claude's task list, and Google Drive (connected, unused).

**What could change:** Every connection becomes an official API or a ready-made tool server. Claude's own connectors only work inside claude.ai, so the webapp uses each company's API or its official tool server.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Procore | Chrome on the VM, the deputy's login. | VM | Procore's API, once ARCO's Procore admin connects our app. | Webapp |
| Outlook, Teams, calendar | Claude's Microsoft 365 connector, read only. | Claude account | Microsoft's API, with the lead's consent. | Webapp |
| Fireflies | Claude's connector. | Claude account | Fireflies' official tool server, with the lead's key. Never for VKU. | Webapp |
| GitHub | Signed in as hcg-bim-runner on the VM. | VM | GitHub's API, with the webapp's own access. | Webapp |
| conduit tools | The Navisworks model, read through conduit on the VM. | VM | Through conduit on the Windows computer, over the private link. | Windows computer |
| Claude's task list | The watchers check in it that the other jobs ran. | Claude account | The webapp's own job records. | Webapp |

**Watch out:**

- Fireflies' docs say Free and Pro plans allow only 50 API requests a day. Hourly meeting prep likely needs Business.

## Skills: the AI's instructions

*Skills, Claude account*

Instruction packs the AI loads, so every job follows the same procedure.

- `hcg-bim-assistant-playbook`: HCG's procedures: 13 skills (12 in the company-wide copy)
- `conduit-aec`: 5 skills for working with 3D models
- `whats-on-my-plate`: a teammate's tasks for the day, from the task board
- `project-onboarding`: for new projects. In the repo, not published

**Today:** HCG's playbook (13 skills, built from the rule book), conduit-aec (5), whats-on-my-plate, and project-onboarding.

**What could change:** The same skills, loaded by the webapp from git. The webapp's engine loads the same skills from git.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Playbook (13 skills) | Uploaded by hand to the lead's Claude account. | Claude account | Loaded by the webapp from git. | Webapp |
| conduit-aec (5 skills) | From the company's plugin store. | Claude account | Loaded by the webapp. | Webapp |
| whats-on-my-plate | Only in the lead's Claude account. | Claude account | Copied into git first. | Webapp |

## The AI assistant

*AI worker, On the VM*

Claude, signed in as the lead. Seven jobs run in the Claude app on the VM, the heartbeat in Claude's cloud. It follows each job's written instructions without asking.

- `Claude desktop app`: a keep-alive task reopens it and Chrome every 10 minutes (an update once closed it and the jobs stopped for 3½ days)
- `Windows-MCP`: lets it read the VM's files, logs and Task Scheduler
- `Project Manifest`: every folder path comes from it
- `Not running`: 8 paused copies tied to the lead's PC (the Flow 4 backup), and 3 finished one-off jobs

**Today:** The Claude app on the VM, signed in as the lead. Seven jobs work through the VM, the heartbeat runs in Claude's cloud, and a keep-alive reopens the app.

**What could change:** Claude runs in the webapp: one engine for chat, approvals and the timed jobs. No Claude app on any computer. The webapp runs Anthropic's Agent SDK, the engine behind Claude Code, with an API key. It is paid first from the API credit included in the lead's plan. Tools are standard tool servers, so the engine could be swapped later. It reaches the Windows computer only through the private link.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Claude app and keep-alive | On the VM. | VM | Could be removed. Claude would run in the webapp on Anthropic's Agent SDK. | Not needed |
| The jobs' send logs and saved state | On the VM. | VM | Copied into the webapp, so no job starts fresh and sends duplicates. | Webapp |
| Windows-MCP | Lets Claude read the VM's files, logs and Task Scheduler. | VM | Could be removed. The webapp's own tools read the same data, with no open access to a computer. | Not needed |
| Project Manifest | Every folder path the jobs use comes from it, read from the synced folder. | VM | The same file, read from ACC through the API. | Webapp |

**Where Claude runs, as weighed**

| Option | What it means | Finding |
|---|---|---|
| Agent SDK in the webapp | Claude Code's engine on our own server. | Suggested. |
| Managed Agents | Anthropic hosts the engine. | Not suggested. Beta, can't reach the Windows computer directly, notes limited to 100 kB per file. |
| Claude app on a computer | As today. | Not suggested. It keeps the app and the lead's sign-in on a computer. |
| Plain API calls | Our code does the steps, Claude only writes. | Not suggested. Behaviour would change. |

Suggested because it reaches the Windows computer privately and keeps the notes as plain files.

## Project files in ACC / Forma

*Service, Outside*

The real project folder (as in Flow 1). The jobs file documents here.

- `ACC / Forma via Desktop Connector`: the sync tool
- `RFI and submittal PDFs`: read by the sweep, with Bluebeam markups (notes drawn on PDFs)

**Today:** The sweep files RFI and submittal PDFs into the VM's synced folders and reads their markups.

**What could change:** The jobs file into the same ACC folders through Autodesk's API. The same folders, through the API.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Filing and reading PDFs | The VM's synced folders. | VM | Autodesk's API. | Webapp |

## Tracker page

*Repo, In git*

Only when ARCO renumbers its schedule: the sweep updates this page's schedule section and publishes it.

- `Nova102-Tracker`: SEED_P6M block in index.html

**Today:** The sweep edits the schedule section of the Nova102-Tracker page (SEED_P6M) and pushes it with git.

**What could change:** When ARCO renumbers its schedule, the sweep updates the tracker page from the webapp. The same edit, pushed from the webapp.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Tracker page schedule section | Edited and pushed from the VM. | VM | Edited and pushed from the webapp. | Webapp |

## Project notes: Nova BIM Schedule

*Claude Project, Claude account*

The AI's shared notebook: instructions plus 357 files (sweep results, diary, change log, meeting notes).

- `Nova BIM Schedule`: the Claude Project

**Today:** A Claude Project with instructions and 357 files (340 notes, 15 standards, 2 spreadsheets), plus a VKU Project. Chats find them with Claude's built-in search. Jobs open them by name.

**What could change:** The notes become plain files in a private git repo, with the same names, so no instruction changes. Claude opens notes by name and searches inside them, the way Claude Code works. Every change is kept as a version. A meaning-based search is added only if a test shows misses.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Nova BIM Schedule notes (357) | In the Claude Project. | Claude account | In git, same names. | Git |
| VKU Project notes | In the Claude Project. | Claude account | In git. | Git |
| Claude's own project memory | In the lead's account. | Claude account | Copied once into a memory file. | Git |
| Chat history | In the lead's account. | Claude account | In the webapp, searchable. | Webapp |
| Memory files in ACC | Written by the memory job. | ACC | Still written, through the API. | ACC |
| Project instructions | The Claude Project's instructions, loaded into every chat and job. | Claude account | The same text, kept with the notes and loaded by the webapp. | Git |
| Finding notes | Chats use Claude's built-in project search. Jobs open notes by name. | Claude account | Claude opens notes by name and searches inside them. A meaning-based search only if a test shows misses. | Webapp |

**Watch out:**

- Claude Projects have no export API, so the one-time copy needs a method.

## Email sender

*Script, Not in git*

Script on the VM that sends the jobs' emails through Amazon. Only to the lead and the deputy.

- `ses_send.py → Amazon SES`

**Today:** ses_send.py on the VM, not in git, sends through Amazon's email, only to the lead and the deputy.

**What could change:** The jobs' emails are sent by the webapp through Amazon's email, from the same sender to the same people. The same emails, sent by the webapp. The script is saved to a repo.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Email script (ses_send.py) | On the VM, not in git. | VM | Saved to a repo. The webapp sends the same emails. | Webapp |

## The lead and the deputy

*People*

Get the summaries and alerts by email, plus a phone push from the heartbeat.

**Today:** Summaries and alerts by email, plus a phone push from the heartbeat.

**What could change:** The lead and the deputy get the same emails, plus the heartbeat's push. Unchanged.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Emails and push | Summaries and alerts by email, plus a phone push from the heartbeat. | Email | No change needed. | Email |
