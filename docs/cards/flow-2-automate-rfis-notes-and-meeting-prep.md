# Flow 2: Automate RFIs, notes and meeting prep

Every card in this flow, in full. The [plan page](https://faiq-kl.github.io/campbell-cts-documentation/vm-to-webapp-plan.html) shows the diagram.

## Keep the rules and procedures

*Repo, In git*

Every HCG procedure in plain English. The lead's rulings are written here first, then the skills are rebuilt from it.

- `Rule book`: the repo

**Today:** Every HCG procedure in plain English, in its own repo. kb.py builds the ClickUp workbooks and flowcharts. Its plugin command rebuilds the 13 playbook skills by hand, from a starter folder that isn't in git, and the lead uploads them.

**What could change:** The rule book stays in git. The skills are built from it. Unchanged rule book. The webapp loads the skills from git. Whether they rebuild by themselves when the rule book changes is still open.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Rule book | Procedures and the lead's rulings. | No | No change needed. |  |
| kb.py build | Run by hand: builds the ClickUp workbooks and flowcharts. | No | No change needed. |  |
| Skills rebuild (kb.py plugin) | By hand, then uploaded by the lead. | Yes | Suggested: rebuilt from the rule book whenever it changes, once the starter folder is in git. | Webapp |

## Run jobs that change project files

*Schedulers ×3, Claude account*

Timers that start the AI on these jobs. They write to real project files.

- `NOVA102 daily RFI & submittal sweep (VM)` (Weekdays 08:24): Checks Procore for new questions (RFIs), approvals (submittals) and reports, and files each into the right ACC folder. Asks Procore for cover PDFs, its only action there. When ARCO posts a new schedule, it loads the dates (after a backup), re-links the work packages and updates the slip-history workbook. If ARCO renumbered activities, it also updates the Nova102-Tracker page. Its notes feed the bot.
- `NOVA102 Project Memory update + flags (VM)` (Weekdays 10:45): Reads email, Teams, meeting notes and Procore, then updates the project diary and "flags" (things the lead may be missing).
- `VKU BIM Assistant shadow + run summary email (VM)` (:25 past each hour after a VKU run): Checks the VKU run, writes VKU's change log and emails the lead a summary.

**Today:** Three timed AI jobs in the lead's Claude account, working through the VM.

**What could change:** All three run in the webapp with their AI, the same instructions and the same timing, through official APIs. The webapp's timer starts them. They use Procore's, Autodesk's, Microsoft's and Fireflies' APIs and write to the webapp's notes. Each one goes live after a side-by-side check against today.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| RFI and submittal sweep (weekdays 08:24) | Procore through Chrome with the deputy's login, where its only action is asking for cover PDFs. Files into ACC, loads ARCO's schedule updates, writes the sweep note, emails the lead and the deputy. | Yes | Procore's and Autodesk's APIs. The same note and email. | Webapp |
| Project Memory and flags (weekdays 10:45) | Outlook, Teams, Fireflies and Procore, then the memory files in ACC and the Claude Project. Emails the flags to the lead and the deputy. | Yes | Microsoft's, Fireflies' and Procore's APIs. The memory files still go to ACC. The same flags email. | Webapp |
| VKU check (:25 in the run windows) | Checks VKU's run in Newtron's ACC, the bot's records on the VM and the page builds on GitHub. Appends VKU's change log, emails the lead. | Yes | Reads the run in the webapp, ACC and GitHub through their APIs. The same change log and email. | Webapp |

**Watch out:**

- Today, the memory job's instructions still use the built-in browser, which lost its Procore sign-in. It may be writing 'Procore not read' every day.

## Run jobs that write notes

*Schedulers ×2, Claude account*

Timers for two jobs that write notes and send emails.

- `NOVA102 change log from the BIM Assistant run log (VM)` (:20 past each hour after a run): Copies the bot's run summary into the day's note.
- `Meeting prep + follow-up (all of the lead's meetings) (VM)` (Weekdays, :45 past each hour, 06:45 to 17:45): Emails a prep outline before each meeting. After a recorded meeting, emails a summary with proposed Forma issues for the lead to approve (it never creates them).

**Today:** The change log writes each run's summary into the day's note. Meeting prep emails an outline before each of the lead's meetings and a follow-up with proposed issues after recorded ones.

**What could change:** Both run in the webapp with their AI and write to the webapp's notes. The same jobs, timing, notes and emails.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Change log (:20, 10:20 to 12:20 and 17:20 to 19:20) | Reads the run log, the bot's database, today's log and the VM's timer, writes into the Claude Project. | Yes | Reads the bot's status in the webapp, writes the same note to the webapp's notes. | Webapp |
| Meeting prep and follow-up (weekdays, hourly 06:45 to 17:45) | Calendar, Outlook, Teams, Fireflies and project notes. Emails the lead, plus the deputy when they attend. Saves each follow-up's proposed issues as a note in the Claude Project. | Yes | Microsoft's and Fireflies' APIs. The same emails, and the same follow-up notes in the webapp's notes. | Webapp |

## Check that every job ran

*Schedulers ×3, Claude account*

Timers for three checks. They raise an alarm by email or phone push if something failed (Flow 4).

- `NOVA102 RFI sweep watchdog (VM)` (Weekdays 09:05): Did the sweep run and send its email?
- `NOVA102 BIM Assistant run watchdog (VM)` (11:40 and 17:40): Did the bot finish and publish?
- `BIM runner VM heartbeat (cloud)` (12:35 and 19:35): Runs in Claude's cloud: are the VM's jobs still running?

**Today:** The sweep watchdog (09:05) and the run watchdog (11:40, 17:40) work through the VM. The heartbeat (12:35, 19:35) runs in Claude's cloud and alerts the lead by push and email.

**What could change:** The two watchdogs run in the webapp. The heartbeat stays outside it, suggested as a small task in the lead's Claude account that checks the webapp's status page. The same checks, in the webapp. The heartbeat keeps watching from outside, suggested as a small task in the lead's Claude account that checks the webapp's status page.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Sweep watchdog (09:05) | Checks the sweep's run in Claude's task list, its files on the VM and its note in the Claude Project. Emails the lead only if something is wrong. | Yes | The same checks, in the webapp. | Webapp |
| Run watchdog (11:40 and 17:40) | Reads the run log, the portal's GitHub history and the VM's timer. Emails the lead only if something is wrong. | Yes | The same, with the webapp's schedule in place of the timer. | Webapp |
| Heartbeat (12:35 and 19:35) | In Claude's cloud. Checks that the VM's jobs still fire and that the change-log note has the bot's run summary. Sends the result to the lead by push and email. | Yes | Suggested: a small task in the lead's Claude account that checks the webapp's status page, with the same push and email. |  |

**Watch out:**

- A watcher must never live on the thing it watches (the lesson of the VM outage).

## Read Procore, email and meetings

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

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Procore | Chrome on the VM, the deputy's login. | Yes | Procore's API, once ARCO's Procore admin connects our app. | Webapp |
| Outlook, Teams, calendar | Claude's Microsoft 365 connector, read only. | Yes | Microsoft's API, with the lead's consent. | Webapp |
| Fireflies | Claude's connector. | Yes | Fireflies' official tool server, with the lead's key. Never for VKU. | Webapp |
| GitHub | Signed in as hcg-bim-runner on the VM. | Yes | GitHub's API, with the webapp's own access. | Webapp |
| conduit tools | The Navisworks model, read through conduit on the VM. | Yes | Through conduit on the VM, over the private link. | VM |
| Claude's task list | The watchers check in it that the other jobs ran. | Yes | The webapp's own job records. | Webapp |

**Watch out:**

- Fireflies' docs say Free and Pro plans allow only 50 API requests a day. Hourly meeting prep likely needs Business.

## Load the AI's instructions

*Skills, Claude account*

Instruction packs the AI loads, so every job follows the same procedure.

- `hcg-bim-assistant-playbook`: HCG's procedures: 13 skills (12 in the company-wide copy)
- `conduit-aec`: 5 skills for working with 3D models
- `whats-on-my-plate`: a teammate's tasks for the day, from the task board
- `project-onboarding`: for new projects. In the repo, not published

**Today:** HCG's playbook (13 skills, built from the rule book), conduit-aec (5), whats-on-my-plate, and project-onboarding.

**What could change:** The same skills, loaded by the webapp from git. The webapp's engine loads the same skills from git.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Playbook (13 skills) | Uploaded by hand to the lead's Claude account. | Yes | Loaded by the webapp from git. | Webapp |
| conduit-aec (5 skills) | From the company's plugin store. | Yes | Loaded by the webapp. | Webapp |
| whats-on-my-plate | Only in the lead's Claude account. | Yes | Copied into git, then loaded by the webapp. | Webapp |

## Run Claude on the VM

*AI worker, On the VM*

Claude, signed in as the lead. Seven jobs run in the Claude app on the VM, the heartbeat in Claude's cloud. It follows each job's written instructions without asking.

- `Claude desktop app`: a keep-alive task reopens it and Chrome every 10 minutes (an update once closed it and the jobs stopped for 3½ days)
- `Windows-MCP`: lets it read the VM's files, logs and Task Scheduler
- `Project Manifest`: the map of folders and files that chats and some jobs use
- `Not running`: 8 paused copies tied to the lead's PC (the Flow 4 backup), and 3 finished one-off jobs

**Today:** The Claude app on the VM, signed in as the lead. Seven jobs work through the VM, the heartbeat runs in Claude's cloud, and a keep-alive reopens the app.

**What could change:** Claude runs in the webapp: one engine for chat, approvals and the timed jobs. No Claude app on any computer. The webapp runs Anthropic's Agent SDK, the engine behind Claude Code, with an API key. It is paid first from the API credit included in the lead's plan. Tools are standard tool servers, so the engine could be swapped later. It reaches the VM only through the private link.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Claude app and keep-alive | On the VM. | Yes | Could be removed. Claude would run in the webapp on Anthropic's Agent SDK. | Not needed |
| The jobs' send logs and saved state | On the VM. | Yes | Copied into the webapp, so no job starts fresh and sends duplicates. | Webapp |
| Windows-MCP | Lets Claude run commands on the VM: read files, logs and Task Scheduler, run Python and the email script, and use git. | Yes | Could be removed. The webapp's own code and tools do the same work, with no open access to a computer. | Not needed |
| Project Manifest | The project's map of folders and files. Chats and the sweep and memory procedures look up paths in it. Read from the VM's synced folder. | Yes | The same file, read from ACC through the API. | Webapp |

**Where Claude runs, as weighed**

| Option | What it means | Finding |
|---|---|---|
| Agent SDK in the webapp | Claude Code's engine on our own server. | Suggested. |
| Managed Agents | Anthropic hosts the engine. | Not suggested. Beta, can't reach the VM directly, notes limited to 100 kB per file. |
| Claude app on a computer | As today. | Not suggested. It keeps the app and the lead's sign-in on a computer. |
| Plain API calls | Our code does the steps, Claude only writes. | Not suggested. Behaviour would change. |

Suggested because it reaches the VM privately and keeps the notes as plain files.

## File PDFs in ACC

*Service, Outside*

The real project folder (as in Flow 1). The jobs file documents here.

- `ACC / Forma via Desktop Connector`: the sync tool
- `RFI and submittal PDFs`: read by the sweep, with Bluebeam markups (notes drawn on PDFs)

**Today:** The sweep files RFI and submittal PDFs into the VM's synced folders and reads their markups.

**What could change:** The jobs file into the same ACC folders through Autodesk's API. The same folders, through the API.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Filing and reading PDFs | The VM's synced folders. | Yes | Autodesk's API. | Webapp |

## Update the tracker page

*Repo, In git*

Only when ARCO renumbers its schedule: the sweep updates this page's schedule section and publishes it.

- `Nova102-Tracker`: SEED_P6M block in index.html

**Today:** The sweep's procedure edits the schedule section of the Nova102-Tracker page (SEED_P6M) and pushes it with git. The edits so far came from Claude coding sessions under the lead's name.

**What could change:** When ARCO renumbers its schedule, the sweep updates the tracker page from the webapp. The same edit, pushed from the webapp.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Tracker page schedule section | When ARCO renumbers its schedule, the section is re-linked and pushed with git. The sweep's procedure covers it, but so far it was done in Claude coding sessions under the lead's name. | Yes | Edited and pushed from the webapp. | Webapp |

## Keep the project notes

*Claude Project, Claude account*

The AI's shared notebook: instructions plus 357 files (sweep results, diary, change log, meeting notes).

- `Nova BIM Schedule`: the Claude Project

**Today:** A Claude Project with instructions and 357 files (340 notes, 15 standards, 2 spreadsheets), plus a VKU Project. Chats find them with Claude's built-in search. Jobs open them by name.

**What could change:** The notes become plain files in a private git repo, with the same names, so no instruction changes. Claude opens notes by name and searches inside them, the way Claude Code works. Every change is kept as a version. A meaning-based search is added only if a test shows misses.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Nova BIM Schedule notes (357) | In the Claude Project. | Yes | In git, same names. | Webapp |
| VKU Project notes | In a separate VKU Claude Project. | Yes | In git. | Webapp |
| Claude's own project memory | In the lead's account. | Yes | Copied once into a memory file. | Webapp |
| Chat history | In the lead's account. | Yes | In the webapp, searchable. | Webapp |
| Project instructions | The Claude Project's instructions, loaded into every chat and job. | Yes | The same text, kept with the notes and loaded by the webapp. | Webapp |
| Finding notes | Chats use Claude's built-in project search. Jobs open notes by name. | Yes | Claude opens notes by name and searches inside them. A meaning-based search only if a test shows misses. | Webapp |

**Watch out:**

- Claude Projects have no export API, so the one-time copy needs a method.

## Send the emails

*Script, Not in git*

Script on the VM that sends the jobs' emails through Amazon. Only to the lead and the deputy.

- `ses_send.py → Amazon SES`

**Today:** ses_send.py on the VM, not in git, sends through Amazon's email, only to the lead and the deputy.

**What could change:** The jobs' emails are sent by the webapp through Amazon's email, from the same sender to the same people. The same emails, sent by the webapp. The script is saved to a repo.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Email script (ses_send.py) | On the VM, not in git. | Yes | Saved to a repo. The webapp sends the same emails. | Webapp |

## Receive summaries and alerts

*People*

Get the summaries and alerts by email, plus a phone push from the heartbeat.

**Today:** Summaries and alerts by email, plus a phone push from the heartbeat.

**What could change:** The lead and the deputy get the same emails, plus the heartbeat's push. Unchanged.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Emails and push | Summaries and alerts by email, plus a phone push from the heartbeat. | No | No change needed. |  |
| Job notices | Claude sends the lead each job's closing message as a notice by email. The heartbeat's notice is its alert, by push and email. | Yes | Suggested: a webapp notification for each job's closing message, on phone or browser. The heartbeat keeps Claude's push and email. | Webapp |
