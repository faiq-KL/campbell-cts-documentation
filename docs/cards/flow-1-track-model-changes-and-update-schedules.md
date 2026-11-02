# Flow 1: Track model changes and update schedules

Every card in this flow, in full. The [plan page](https://faiq-kl.github.io/campbell-cts-documentation/vm-to-webapp-plan.html) shows the diagram.

## Start the runs on time

Four timers on the VM start the bot twice a day and keep its helpers running.

*Schedulers ×4, On the VM*

Windows Task Scheduler. A manual re-run (only when the client asks) uses a one-off task.

- `\HCG BIM Assistant\Daily run` (Weekdays 09:00 and 16:00 CT, and at sign-in): Starts the bot: NOVA102 first (60 to 90 min), then VKU. Each slot runs once a day at most.
- `\HCG BIM Assistant\Dialog watcher` (At sign-in): Clicks safe answers on Revit and Navisworks pop-ups so runs don't stall. Never clicks save, sync or publish. Script not in git.
- `\HCG BIM Assistant\Claude keep-alive` (Every 10 min, and at sign-in): Reopens the Claude app and Chrome if they closed.
- `\HCG BIM Assistant\Keep desktop session` (Every 5 min): If the remote desktop disconnects, keeps the VM's screen session alive so Revit and the pop-up clicker keep working.

**Today:** Four Windows timers on the VM: the Daily run (weekdays 09:00 and 16:00 CT), the pop-up clicker, the Claude keep-alive and the desktop-session keeper.

**What could change:** The webapp's timer starts the runs at the same times as today. The webapp starts each run at 09:00 and 16:00 Chicago time on weekdays, holidays included, NOVA102 then VKU, at most once per run time, with the same time limits. It uses Chicago wall-clock time, so a clock change can't shift it. A missed run starts when the webapp is back. Manual runs come from the webapp's chat, only when the client asks.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Daily run | Weekdays 09:00 and 16:00 CT, plus a catch-up at sign-in. NOVA102 then VKU, each slot at most once a day. Since the clock change it likely fires an hour early. | Yes, the webapp starts it at the same times. | The webapp's timer, in Chicago time, with the same order and limits. A missed run starts when the webapp is back. | Webapp |
| The 30-minute morning wait | A leftover from the client's laptop. | Yes, without it, morning results come 30 minutes sooner. | Could be removed, so results arrive 30 minutes earlier. One setting brings it back. | Not needed |
| Pop-up clicker | Answers Revit and Navisworks pop-ups so runs don't stall. Its script is not in git. | Yes, we save its code to GitHub. | Can stay on the VM, with its script saved to a repo first. | VM |
| Desktop-session keeper | Keeps the screen session alive for Revit and Navisworks. Its script is not in git. | Yes, we save its code to GitHub. | Can stay on the VM, with its script saved to a repo first. | VM |
| Claude keep-alive | Reopens the Claude app and Chrome. | Yes, the Claude app and Chrome won't be needed. | Could be removed. No Claude app or Chrome would be needed on the VM. | Not needed |
| Manual runs | A one-off task, only when the client asks. It counts as that day's most recent run time, or as the 09:00 run if it starts before 09:00. | Yes, the client starts it from the webapp's chat. | From the webapp's chat, with the same rule. | Webapp |

**Watch out:**

- Since the clock change the VM's timer likely fires at 08:00 and 15:00, so the 09:00 run comes late and there is no 16:00 run. Only the client can fix it on the VM. That late run also skips the Navisworks morning step, which only runs before 13:00.

## Open the models in Revit and Navisworks

Revit and Navisworks on the VM open the 3D models for the bot and for chats.

*Service, On the VM*

The NOVA102 and VKU electrical models in Revit and Navisworks, opened one at a time. Read only: never saved back to the shared models.

**Today:** Each run opens NOVA102's 3 models and VKU's 1 model in Revit, one at a time, read only. Each morning Navisworks loads NOVA102 - Triad.nwf (54 models), runs 69 clash tests, exports the clash report, applies approved 4D changes and saves the NWF and NWD.

**What could change:** Revit and Navisworks stay on the VM, for the bot and for chat. The same Revit reading and Navisworks steps, on the VM. The webapp sends the jobs over the private link and gets the results back. Navisworks has no online version, so it can only run there.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Bot's Revit reading | NOVA102's 3 models and VKU's 1, opened one at a time in Revit 2025 and 2026, read only. | No, only Revit can open the models. | No change needed. Stays on the VM. | VM |
| Chat's live Revit | Chats ask the open model through conduit. | No, chats need Revit open to answer questions. | No change needed. The same Revit on the VM, asked from the webapp's chat. | VM |
| Navisworks morning step | Each morning on the VM: 69 clash tests, the clash report, approved 4D changes, and the NWF and NWD saved. | No, Navisworks has no online version. | No change needed. Stays on the VM. | VM |

**Where the bot reads Revit, as weighed**

| Option | What it means | Finding |
|---|---|---|
| Desktop Revit on the VM | As today, next to chat's Revit. | Suggested. |
| Autodesk's online Revit (Automation API) | The bot's reading runs in Autodesk's cloud. Revit 2025, 2026 and cloud models are supported. Paid in Autodesk credits, about 2 Flex tokens per processing hour. | Noted as possible. Needs a reading package without a screen and a side-by-side test. |
| Autodesk's free data service | Parts lists from published model versions only. | Not suggested. |

Revit must stay on the VM for chat anyway, so the online option would not remove it.

**Watch out:**

- Under Autodesk's current terms, BIM Collaborate Pro no longer includes Navisworks Manage for new purchases and renewals.

**What we need:**

- An Autodesk account for the VM, not the client's, with Revit, Navisworks Manage and Forma Design Collaboration licences, added to Triad's, ARCO's and Newtron's projects.

## Connect to Revit and Navisworks

conduit, the company's software, lets the bot and chats work inside Revit and Navisworks.

*Repo, In git*

The company's software that lets the bot (and the AI) open and read Revit and Navisworks. Installed on the VM as add-ins and small servers.

- `conduit`: the repo

**Today:** The company's software that lets the bot and Claude work inside Revit and Navisworks. On the VM the bot and the Claude app use it.

**What could change:** conduit stays on the VM and is reached from the webapp over the private link. The same conduit on the VM, with a small network mode or bridge so the webapp's Claude and bot can reach it. It answers only inside the private link, never on the internet.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Revit add-in and tool | The bot opens and reads the models through it. Chats in the Claude app on the VM use it too. | Yes, the webapp connects to it through a private link. | Reached by the webapp's bot and chat over the private link. | VM |
| Navisworks bridge | Lets the bot's morning step drive Navisworks. The VM runs a newer build than the repo. | Yes, we save its newest code to GitHub. | Stays as it is, with its source saved to a repo. | VM |
| Point cloud and fabrication tools | Used by chats. | Yes, the webapp's chat connects to them through a private link. | Reached from the webapp over the private link. | VM |

**What we need:**

- The client's OK to run conduit on the new computer with a network mode.
- The Navisworks bridge's newest source from the VM.

## Collect files, issues and scans

Everything else the bot reads on each run: schedules, drawings, issues, clashes and scans.

*Service, Outside*

Read only. All come from the VM's synced copy of the project folder, except Forma issues and RFIs, which it downloads from the Forma website. Spreadsheets are read from a copy.

- `Project Manifest + Profile`: settings files: where everything is, and how to read it
- `Coordination Schedule`: the work-package spreadsheet
- `Issued drawing PDFs`: IFR / IFC / IFF folders (issued for review, construction or fabrication)
- `RFI and submittal folders`: their status: open, in review, answered
- `Forma issues and RFIs`: from ARCO's and Triad's Forma projects, through the bot's own Chrome
- `RFI sweep state file`: Procore items from the morning AI sweep (Flow 2)
- `Clash report (XML)`: the 09:00 run re-runs the clash tests and exports it. The newest export is read, the run's or the client's
- `New laser scans (.e57)`
- `4D approval files`: schedule changes the client approved (Flow 3)

**Today:** Read from the VM's synced copy of the project folders. Forma issues and RFIs are exported by a bot-controlled Chrome.

**What could change:** The webapp reads the bot's inputs straight from ACC through Autodesk's API. Autodesk's API for all of them. The files stay the master copies in ACC. Triad, Newtron and ARCO connect our app to their Autodesk accounts.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Project Manifest and Profile | From the synced folder. | Yes, Autodesk's API replaces the synced folder. | From ACC through the API. | Webapp |
| Coordination Schedule | From the synced folder. | Yes, Autodesk's API replaces the synced folder. | From ACC through the API. | Webapp |
| Issued drawing PDFs | Dated by the VM's file date. These dates become Actual Finish dates. | Yes, the date can come from Forma, if the client agrees. | Could be dated by Forma's own upload date, the preferred option. It can differ by minutes or days, so the client decides. | Webapp |
| RFI and submittal folders | The folder an RFI sits in is its status. | Yes, Autodesk's API reads the same folders. | The same folders, read through the API. | Webapp |
| Forma issues and RFIs | Chrome on the VM exports Excel files. | Yes, Autodesk's API replaces the Chrome exports. | Autodesk's Issues and RFIs APIs. No Chrome. | Webapp |
| Clash report | The newest export in the Clash Reports folder: the morning Navisworks step's, or one the client saved. | Yes, the VM sends it to the webapp, or the API reads it from ACC. | The step's export sent from the VM to the webapp. The client's own exports read from ACC through the API. | Webapp |
| New laser scans | In the synced scan folder. | Yes, Autodesk's API replaces the synced folder. | From ACC through the API. | Webapp |
| 4D approval files | Written by chats, read by the morning Navisworks step. | Yes, the webapp's chat writes them and sends them with the morning job. | Written by the webapp's chat, sent with the morning Navisworks job. | Webapp |
| RFI sweep state file | Written by the morning AI sweep on the VM. The bot reads its Procore items. | Yes, the sweep and the bot both move to the webapp. | Both run in the webapp, so the bot reads it there. | Webapp |

**Watch out:**

- Autodesk's support page says Forma's date and Desktop Connector's file date can differ by minutes or days.

## Find changes and plan the updates

The bot compares each model with its last version and works out which records to update.

*Repo, In git*

A Python program. For each project it finds what changed in the model, matches drawings to work packages and updates the spreadsheets. It also builds the clash list, the issue-tracker and task-board data and a BES / Triad feed, checks new scans, and applies approved 4D changes in the morning. Results that fail its safety checks are held for the client.

- `hcg-bim-assistant`: the repo (main branch: master)
- `runner.json`: its settings, only on the VM
- `HCGBIM.OpenGuard add-in`: small Revit add-in that avoids one known error when opening a model

**Today:** A Python program on the VM. Each run reads the models, matches drawings to work packages, updates spreadsheets and pages, builds the clash, issue and 4D feeds, and holds risky results for the client.

**What could change:** The bot's thinking moves to the webapp. The VM keeps only the Revit and Navisworks steps. The webapp runs the same code on Chicago time, sends model jobs to the VM over the private link and gets the results back. Its memory (orchestrator.db) and settings (runner.json) move with the same values.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Revit reading and Navisworks step | On the VM. | No, it needs Revit and Navisworks. | No change needed. Stays on the VM. | VM |
| Matching, register plan, workbooks, feeds | On the VM. | Yes, the same code moves to the webapp. | In the webapp, same code. | Webapp |
| Memory and settings | orchestrator.db and runner.json on the VM. | Yes, they move to the webapp unchanged. | In the webapp, same values. | Webapp |
| Held items | Released by commands run on the VM from chat. | Yes, the webapp releases them when the client asks in chat. | Released by the webapp, from chat. | Webapp |
| File dates | From the synced files. | Yes, Autodesk's API replaces the synced files. | From ACC through the API. | Webapp |
| Clock | The VM runs on Chicago time. | Yes, the webapp keeps Chicago time. | The bot's work runs on Chicago time. | Webapp |
| The bot's code | The VM runs its own copy, with one file that is not in GitHub (the clash export step). | Yes, the webapp runs it from GitHub, once we save the missing file. | The webapp runs the same code from GitHub, once that file is saved there. | Webapp |
| OpenGuard | Fixes TEMP-POWER's error, in the run's session only. | No, it works inside Revit. | No change needed. Stays on the VM. | VM |

**Watch out:**

- The clash export step was never saved to GitHub. Copy it from the VM first (missing item 1).

## Compare scans and build 4D views

Compares laser scans with the model to measure progress, and builds the 4D and 5D views.

*Repo tools, In git*

The bot's last two steps: a 4D timeline snapshot, and the 4D refresh, which also writes the 5D quantity workbooks. The scan page is updated by hand after the client approves a scan.

- `Nova102-Scan-Tracker/tools`: scripts in the scan page's repo

**Today:** When a new scan lands, the bot exports the conduits from Revit, compares the scan with them on a 1.5 cm grid, and holds the result for the client. The 4D viewer and the 5D quantity workbook refresh after each run.

**What could change:** The scan check and the 4D and 5D tools run in the webapp. Only the Revit conduit export and the Navisworks timeline snapshot stay on the VM. The VM exports the conduits. The webapp gets the scan from ACC through the API and runs the same comparison. The Scan Tracker page stays a by-hand step after approval.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Conduit export | Revit on the VM. | No, it needs Revit. | No change needed. Stays on the VM. | VM |
| Scan comparison | Python on the VM. | Yes, the same code moves to the webapp. | The same code in the webapp, on a worker sized for the biggest scan. | Webapp |
| Held scan result and its release | On the VM. | Yes, the webapp holds and releases it. | In the webapp. | Webapp |
| 4D viewer, 4D progress, 5D workbook | After each run, on the VM. | Yes, the webapp builds them with the same code. | In the webapp, same code and times. | Webapp |
| Timeline snapshot | Navisworks, each morning. | No, it needs Navisworks. | No change needed. Stays on the VM. | VM |

**Watch out:**

- Scan files are big. Measure peak memory before sizing the webapp's worker.

## Save the files to ACC

The bot saves the updated spreadsheets and Navisworks files to ACC, with backups first.

*Service, Outside*

The real project folder in Autodesk's cloud, synced to the VM. The bot writes:

- `ACC / Forma via Desktop Connector`: the sync tool
- `Coordination Schedule`: backed up before every write
- `Model Impact Tracker, materials workbook`: what changed and what it affects
- `Lookahead, RFI & Submittal Impact Register`: upcoming work, and what blocks it
- `Model inventories and diffs, clash report`: model parts and the day's differences
- `Navisworks NWF / NWD`: combined model files, saved with approved 4D changes
- `5D quantity crosswalks`: quantities per work package
- `Run log`: one summary per run: what updated, what was held

**Today:** The bot writes into the VM's synced folder: the Coordination Schedule (backed up first), workbooks, inventories, the clash report, 5D workbooks, the run log, and the NWF and NWD.

**What could change:** The same files land in the same ACC folders, written by the webapp through Autodesk's API with the same safety steps. The webapp downloads the file, checks it, writes a scratch copy, confirms ACC still has the version it downloaded, uploads, and checks the upload. Backups stay. The NWF and NWD are uploaded by the file helper on the VM.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Coordination Schedule | 6 safety steps. | Yes, the webapp writes it with Autodesk's API and the same 6 safety steps. | The same 6 steps, through the API. | Webapp |
| Backups | The bot copies each workbook into the backups folder before every write. | Yes, the webapp makes the same backups. | The same copies in the same folder, made by the webapp through the API. | Webapp |
| Other workbooks and the run log | Model Impact Tracker, materials workbook, Lookahead, RFI and Submittal Impact Register, model inventories and diffs, clash report, 5D workbooks and the run log, written to the synced folder. | Yes, Autodesk's API replaces the synced folder. | The same files, written through the API. | Webapp |
| NWF and NWD | Saved by Navisworks, uploaded by Desktop Connector. | Yes, a small file helper uploads them instead of Desktop Connector. | Saved by Navisworks, uploaded by the file helper through the API. | VM |
| Desktop Connector | Syncs whole project folders on the VM. | Yes, Autodesk's API and a small file helper replace it. | Could be removed. A file helper would move only Navisworks' files. | Not needed |

**Watch out:**

- ACC's version history will show our app's account as the author.

**What we need:**

- Write rights for our app only where the bot writes today.

## Alert the client about each run

Emails the client, and shows a pop-up on the VM, when a run has a problem or finishes.

*Service, Outside*

Progress and problems, during each run.

- `Amazon SES`: email to the client, from the bot email address
- `Windows pop-ups`: on the VM
- `Teams`: not used: the Teams link in runner.json is empty

**Today:** During each run the bot emails the client through Amazon's email and shows pop-ups on the VM. Teams is switched off.

**What could change:** The webapp sends the same alert emails. The run-finished pop-up needs a new home. The same emails through Amazon's email, from the same sender to the same person. In the suggested setup nobody opens the VM, so the pop-up's job moves to the webapp.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Alert emails | When a run is blocked, slow, gives up, fails or finishes: an email from the bot email address to the client. | Yes, the webapp sends the same emails. | The same emails, sent by the webapp. | Webapp |
| Windows pop-ups | On the VM's screen when a run is blocked, slow, gives up, fails or finishes. A click on the finished one opens the summary. | Yes, a phone or browser notification replaces it. | Suggested: a webapp notification on phone or browser. The finished one opens the summary. | Webapp |
| Progress lines | A file on the VM. | Yes, the webapp shows the run's progress. | The webapp's run status. | Webapp |

**What we need:**

- Amazon email access for the webapp.

## Publish the web pages

The bot sends fresh data to 7 of the 10 web pages after each run.

*Repos ×10, In git*

Each page is its own repo. New data saved to its main branch goes live within minutes. The bot updates the first 7. The last 3 are updated by hand.

- `HCG-BIM-Portal`: front door: sign-in, team task board, issue tracker and run status (from the bot), trial pages for equipment, commissioning, closeout, operations, maintenance and prefab
- `Nova102-Tracker`: drawing progress per work package, plus field progress from scans
- `Nova102-Impact-Dashboard`: model changes (bot), plus punch list, open items and expectation gaps (by hand)
- `VKU-Tracker`: drawing progress for VKU
- `VKU-Impact-Dashboard`: model changes for VKU
- `HCG-Portfolio-Dashboard`: all projects on one page, for leadership
- `Nova102-4D-Viewer`: 3D model with a timeline, refreshed by the bot
- `Nova102-Action-Hub`: BES and Triad to-do lists, Triad sign-off board, Lead Queue, 4D view. Updated by hand. A bot step for its Triad records exists, not yet seen running. Emails are drafts only for now
- `Nova102-Scan-Tracker`: built vs planned, from scans. Updated by hand
- `Nova102-RFI-Sweep`: answered questions. Updated by hand from the client's PC

**Today:** 10 pages, each its own GitHub repo, put online by Cloudflare behind a sign-in. The bot pushes new data into 7 of them and checks its Impact Dashboard updates went live. The rest are kept by hand.

**What could change:** The 10 pages stay exactly as they are. Only the bot's publishing moves to the webapp. The webapp writes the same data into the same 7 repos with the same code, through GitHub's API with its own access, and runs the same live check.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Publishing to 7 pages | Git pushes from the VM. | Yes, the webapp publishes them with its own GitHub access. | From the webapp, with its own GitHub access. | Webapp |
| Live check | After each Impact Dashboard update, the NOVA102 page is opened with a key and checked. The VKU page has no key, so its GitHub build status is checked instead. | Yes, the webapp runs the same checks. | The same checks, from the webapp. | Webapp |
| The pages themselves | On Cloudflare. | No, the pages stay on Cloudflare. | No change needed: same addresses, look, sign-in and data. |  |

## Receive alerts and decide

The client reads the run alerts and decides what happens next.

*People*

The only person on the bot's alert list (runner.json).

**Today:** The only person on the bot's alert list.

**What could change:** The client keeps getting the same alerts at the same address. The same, now sent by the webapp.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Alert list | The client only, set in runner.json on the VM. | Yes, the same list moves to the webapp's settings. | The same list, kept in the webapp's settings. | Webapp |

## Host the pages and sign-in

Cloudflare hosts the web pages, their sign-in, databases and files.

*Service, Outside*

Puts each page online and checks who may open it (emailed code or Microsoft login). The bot checks its updates went live.

- `Cloudflare`: the hosting company
- `4 small databases`: what people type into the pages: notes, tasks, owners, records, sign-offs, Lead Queue decisions
- `1 file bucket`: laser-scan files
- `Page libraries`: some pages load code from public sites (jsDelivr, cdnjs)

**Today:** Puts each page online and checks who may open it. Holds the pages' 4 small databases, including the Task Board, and 1 scan file bucket.

**What could change:** Cloudflare stays as it is. The keys and scripts the bot and chats use move to the webapp. Unchanged. The live-check key moves to the webapp.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Hosting, sign-in, databases, file bucket | Cloudflare. | No, Cloudflare keeps hosting everything. | No change needed. |  |
| Live-check key | On the VM. | Yes, it moves to the webapp's secure settings. | In the webapp's secure settings. | Webapp |
| Page libraries | Some pages load code from public sites (jsDelivr, cdnjs). | No, the pages load them the same way. | No change needed. |  |

## Use the pages

HCG's team and leaders use the pages, and BES and Triad see only their own items.

*People*

HCG's team and leadership, plus BES and Triad (their own items only). They also add notes, tasks and sign-offs.

**Today:** HCG's team and leadership, plus BES and Triad, who see their own items only.

**What could change:** Nothing changes for the people who use the pages. The same pages, addresses, sign-in and data.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Page users | HCG's team and leadership, plus BES and Triad, who see their own items only. | No, the same people see the same pages. | No change needed. |  |
