# Flow 1: Track model changes and update schedules

Every card in this flow, in full. The [plan page](https://faiq-kl.github.io/campbell-cts-documentation/vm-to-webapp-plan.html) shows the diagram.

## Timers on the VM

*Schedulers ×4, On the VM*

Windows Task Scheduler. A manual re-run (only when the lead asks) uses a one-off task.

- `\HCG BIM Assistant\Daily run` (Weekdays 09:00 and 16:00 CT, and at sign-in): Starts the bot: NOVA102 first (60 to 90 min), then VKU. Each slot runs once a day at most.
- `\HCG BIM Assistant\Dialog watcher` (At sign-in): Clicks safe answers on Revit and Navisworks pop-ups so runs don't stall. Never clicks save, sync or publish. Script not in git.
- `\HCG BIM Assistant\Claude keep-alive` (Every 10 min, and at sign-in): Reopens the Claude app and Chrome if they closed.
- `\HCG BIM Assistant\Keep desktop session` (Every 5 min): If the remote desktop disconnects, keeps the VM's screen session alive so Revit and the pop-up clicker keep working.

**Today:** Four Windows timers on the VM: the Daily run (weekdays 09:00 and 16:00 CT), the pop-up clicker, the Claude keep-alive and the desktop-session keeper.

**What could change:** The webapp's timer starts the runs at the same times as today. The webapp starts each run at 09:00 and 16:00 Chicago time on weekdays, holidays included, NOVA102 then VKU, at most once per run time, with the same time limits. It uses Chicago wall-clock time, so a clock change can't shift it. A missed run starts when the webapp is back. Manual runs come from the webapp's chat, only when the lead asks.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Daily run | Weekdays 09:00 and 16:00 CT, plus a catch-up at sign-in. NOVA102 then VKU, each slot at most once a day. Since the clock change it likely fires an hour early. | VM | The webapp's timer, in Chicago time, with the same order and limits. A missed run starts when the webapp is back. | Webapp |
| The 30-minute morning wait | A leftover from the lead's laptop. | VM | Could be dropped, so results arrive 30 minutes earlier. One setting brings it back. | Not needed |
| Pop-up clicker | Answers Revit and Navisworks pop-ups so runs don't stall. Its script is not in git. | VM | Can stay on the Windows computer, with its script saved to a repo first. | Windows computer |
| Desktop-session keeper | Keeps the screen session alive for Revit and Navisworks. | VM | Can stay on the Windows computer. | Windows computer |
| Claude keep-alive | Reopens the Claude app and Chrome. | VM | Could be removed. No Claude app or Chrome would be needed on the Windows computer. | Not needed |
| Manual runs | A one-off task, only when the lead asks. It counts as the next run time. | VM | From the webapp's chat, with the same rule. | Webapp |

**Watch out:**

- Since the clock change the VM's timer likely fires at 08:00 and 15:00, so the 09:00 run comes late and there is no 16:00 run. Only the lead can fix it on the VM.

## 3D models

*Service, On the VM*

The NOVA102 and VKU electrical models in Revit and Navisworks, opened one at a time. Read only: never saved back to the shared models.

**Today:** Each run opens NOVA102's 3 models and VKU's 1 model in Revit, one at a time, read only. Each morning Navisworks loads NOVA102 - Triad.nwf (54 models), runs 69 clash tests, exports the clash report, applies approved 4D changes and saves the NWF and NWD.

**What could change:** Revit and Navisworks stay on the Windows computer, for the bot and for chat. The same Revit reading and Navisworks steps, on the Windows computer. The webapp sends the jobs over the private link and gets the results back. Navisworks has no online version, so it can only run there.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Bot's Revit reading | NOVA102's 3 models and VKU's 1, opened one at a time in Revit 2025 and 2026, read only. | VM | Desktop Revit on the Windows computer. | Windows computer |
| Chat's live Revit | Chats ask the open model through conduit. | VM | The same Revit, asked from the webapp's chat. | Windows computer |
| Navisworks morning step | Each morning on the VM: 69 clash tests, the clash report, approved 4D changes, and the NWF and NWD saved. | VM | The same steps on the Windows computer. | Windows computer |

**Where the bot reads Revit, as weighed**

| Option | What it means | Finding |
|---|---|---|
| Desktop Revit on the Windows computer | As today, next to chat's Revit. | Suggested. |
| Autodesk's online Revit (Automation API) | The bot's reading runs in Autodesk's cloud. Revit 2025, 2026 and cloud models are supported. Paid in Autodesk credits, about 2 Flex tokens per processing hour. | Noted as possible. Needs a reading package without a screen and a side-by-side test. |
| Autodesk's free data service | Parts lists from published model versions only. | Not suggested. |

Revit must stay on the Windows computer for chat anyway, so the online option would not remove it.

**Watch out:**

- Under Autodesk's current terms, BIM Collaborate Pro no longer includes Navisworks Manage for new purchases and renewals.

**What we need:**

- An Autodesk account for the Windows computer, not the lead's, with Revit, Navisworks Manage and Forma Design Collaboration licences, added to Triad's, ARCO's and Newtron's projects.

## conduit: the link to Revit and Navisworks

*Repo, In git*

The company's software that lets the bot (and the AI) open and read Revit and Navisworks. Installed on the VM as add-ins and small servers.

- `conduit`: the repo

**Today:** The company's software that lets Claude work inside Revit and Navisworks. On the VM it talks only to the Claude app on the same computer. OpenGuard fixes one known Revit error when the bot opens TEMP-POWER.

**What could change:** conduit stays on the Windows computer and is reached from the webapp over the private link. The same conduit on the Windows computer, with a small network mode or bridge so the webapp's Claude and bot can reach it. It answers only inside the private link, never on the internet.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Revit add-in and tool | Used by the Claude app on the VM. | VM | Reached from the webapp. | Windows computer |
| Navisworks bridge | A newer local build on the VM, ahead of the repo. | VM | Copied as it is, source saved to a repo. | Windows computer |
| OpenGuard | Fixes TEMP-POWER's error, in the run's session only. | VM | Copied as it is. | Windows computer |
| Point cloud and fabrication tools | Used by chats. | VM | Reached from the webapp. | Windows computer |

**What we need:**

- The lead's OK to run conduit on the new computer with a network mode.
- The Navisworks bridge's newest source from the VM.

## Other things the bot reads

*Service, Outside*

Read only. All come from the VM's synced copy of the project folder, except Forma issues and RFIs, which it downloads from the Forma website. Spreadsheets are read from a copy.

- `Project Manifest + Profile`: settings files: where everything is, and how to read it
- `Coordination Schedule`: the work-package spreadsheet
- `Issued drawing PDFs`: IFR / IFC / IFF folders (issued for review, construction or fabrication)
- `RFI and submittal folders`: their status: open, in review, answered
- `Forma issues and RFIs`: from ARCO's and Triad's Forma projects, through the bot's own Chrome
- `RFI sweep state file`: Procore items from the morning AI sweep (Flow 2)
- `Clash report (XML)`: the 09:00 run re-runs the clash tests and exports it. The newest export is read, the run's or the lead's
- `New laser scans (.e57)`
- `4D approval files`: schedule changes the lead approved (Flow 3)

**Today:** Read from the VM's synced copy of the project folders. Forma issues and RFIs are exported by a bot-controlled Chrome.

**What could change:** The webapp reads the bot's inputs straight from ACC through Autodesk's API. Autodesk's API for all of them. The files stay the master copies in ACC. Triad, Newtron and ARCO connect our app to their Autodesk accounts.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Project Manifest and Profile | From the synced folder. | VM | From ACC through the API. | Webapp |
| Coordination Schedule | From the synced folder. | VM | From ACC through the API. | Webapp |
| Issued drawing PDFs | Dated by the VM's file date. These dates become Actual Finish dates. | VM | Dated by Forma's own upload date, the preferred option. It can differ by minutes or days, so the lead has the final say. | Webapp |
| RFI and submittal folders | The folder an RFI sits in is its status. | VM | The same folders, read through the API. | Webapp |
| Forma issues and RFIs | Chrome on the VM exports Excel files. | VM | Autodesk's Issues and RFIs APIs. No Chrome. | Webapp |
| Clash report | Saved by the Navisworks step. | VM | Sent from the Windows computer to the webapp. | Webapp |
| New laser scans | In the synced scan folder. | VM | From ACC through the API. | Webapp |
| 4D approval files | Written by chats, read by the morning Navisworks step. | VM | Written by the webapp's chat, sent with the morning Navisworks job. | Webapp |
| RFI sweep state file | Written by the morning AI sweep on the VM. The bot reads its Procore items. | VM | Both run in the webapp, so the bot reads it there. | Webapp |

**Watch out:**

- Autodesk's support page says Forma's date and Desktop Connector's file date can differ by minutes or days.

## The bot: hcg-bim-assistant

*Repo, In git*

A Python program. For each project it finds what changed in the model, matches drawings to work packages and updates the spreadsheets. It also builds the clash list, the issue-tracker and task-board data and a BES / Triad feed, checks new scans, and applies approved 4D changes in the morning. Results that fail its safety checks are held for the lead.

- `hcg-bim-assistant`: the repo (main branch: master)
- `runner.json`: its settings, only on the VM
- `HCGBIM.OpenGuard add-in`: small Revit add-in that avoids one known error when opening a model

**Today:** A Python program on the VM. Each run reads the models, matches drawings to work packages, updates spreadsheets and pages, builds the clash, issue and 4D feeds, and holds risky results for the lead.

**What could change:** The bot's thinking moves to the webapp. The Windows computer keeps only the Revit and Navisworks steps. The webapp runs the same code on Chicago time, sends model jobs to the Windows computer over the private link and gets the results back. Its memory (orchestrator.db) and settings (runner.json) move with the same values.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Revit reading and Navisworks step | On the VM. | VM | On the Windows computer. | Windows computer |
| Matching, register plan, workbooks, feeds | On the VM. | VM | In the webapp, same code. | Webapp |
| Memory and settings | orchestrator.db and runner.json on the VM. | VM | In the webapp, same values. | Webapp |
| Held items | Released by commands run on the VM from chat. | VM | Released by the webapp, from chat. | Webapp |
| File dates | From the synced files. | VM | From ACC through the API. | Webapp |
| Clock | The VM runs on Chicago time. | VM | The bot's work runs on Chicago time. | Webapp |
| The bot's code | The VM runs its own copy, with one file that is not in GitHub (the clash export step). | VM | The webapp runs the same code from GitHub, once that file is saved there. | Webapp |

**Watch out:**

- The clash export step was never saved to GitHub. Copy it from the VM first (missing item 1).

## Scan and 4D tools

*Repo tools, In git*

The bot's last two steps: a 4D timeline snapshot, and the 4D refresh, which also writes the 5D quantity workbooks. The scan page is updated by hand after the lead approves a scan.

- `Nova102-Scan-Tracker/tools`: scripts in the scan page's repo

**Today:** When a new scan lands, the bot exports the conduits from Revit, compares the scan with them on a 1.5 cm grid, and holds the result for the lead. The 4D viewer and the 5D quantity workbook refresh after each run.

**What could change:** The scan check and the 4D and 5D tools run in the webapp. Only the Revit conduit export and the Navisworks timeline snapshot stay on the Windows computer. The Windows computer exports the conduits. The webapp gets the scan from ACC through the API and runs the same comparison. The Scan Tracker page stays a by-hand step after approval.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Conduit export | Revit on the VM. | VM | Revit on the Windows computer. | Windows computer |
| Scan comparison | Python on the VM. | VM | The same code in the webapp, on a worker sized for the biggest scan. | Webapp |
| Held scan result and its release | On the VM. | VM | In the webapp. | Webapp |
| Scan Tracker page | Rebuilt by hand after approval. | By hand | Can stay by hand after approval. | By hand |
| 4D viewer, 4D progress, 5D workbook | After each run, on the VM. | VM | In the webapp, same code and times. | Webapp |
| Timeline snapshot | Navisworks, each morning. | VM | Navisworks on the Windows computer. | Windows computer |

**Watch out:**

- Scan files are big. Measure peak memory before sizing the webapp's worker.

## Project files in ACC / Forma

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

**What could change:** The same files land in the same ACC folders, written by the webapp through Autodesk's API with the same safety steps. The webapp downloads the file, checks it, writes a scratch copy, confirms ACC still has the version it downloaded, uploads, and checks the upload. Backups stay. The NWF and NWD are uploaded by the file helper on the Windows computer.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Coordination Schedule | 6 safety steps. | VM | The same 6 steps, through the API. | Webapp |
| Backups | Copies before every write. | ACC | Keep as they are. | ACC |
| Other workbooks and the run log | Model Impact Tracker, materials workbook, Lookahead, RFI and Submittal Impact Register, model inventories and diffs, clash report, 5D workbooks and the run log, written to the synced folder. | VM | The same files, written through the API. | Webapp |
| NWF and NWD | Saved by Navisworks, uploaded by Desktop Connector. | VM | Saved by Navisworks, uploaded by the file helper through the API. | Windows computer |
| Desktop Connector | Syncs whole project folders on the VM. | VM | Could be removed. A file helper would move only Navisworks' files. | Not needed |

**Watch out:**

- ACC's version history will show our app's account as the author.

**What we need:**

- Write rights for our app only where the bot writes today.

## Run alerts

*Service, Outside*

Progress and problems, during each run.

- `Amazon SES`: email to the lead, from the bot email address
- `Windows pop-ups`: on the VM
- `Teams`: not used: the Teams link in runner.json is empty

**Today:** During each run the bot emails the lead through Amazon's email and shows pop-ups on the VM. Teams is switched off.

**What could change:** The webapp sends the same alert emails. The run-finished pop-up needs a new home. The same emails through Amazon's email, from the same sender to the same person. The lead never looks at the Windows computer, so the pop-up's job moves to the webapp.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Alert emails | When a run is blocked, slow, gives up, fails or finishes: an email from the bot email address to the lead. | VM | The same emails, sent by the webapp. | Webapp |
| Run-finished pop-up | On the VM's screen. A click opens the summary. | VM | Suggested: a webapp notification on phone or browser that opens the summary. | To explore |
| Progress lines | A file on the VM. | VM | The webapp's run status. | Webapp |

**What we need:**

- Amazon email access for the webapp.

## The 10 web pages

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
- `Nova102-RFI-Sweep`: answered questions. Updated by hand from the lead's PC

**Today:** 10 pages, each its own GitHub repo, put online by Cloudflare behind a sign-in. The bot pushes new data into 7 of them and checks each update went live. The rest are kept by hand.

**What could change:** The 10 pages stay exactly as they are. Only the bot's publishing moves to the webapp. The webapp writes the same data into the same 7 repos with the same code, through GitHub's API with its own access, and runs the same live check.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Publishing to 7 pages | Git pushes from the VM. | VM | From the webapp, with its own GitHub access. | Webapp |
| Live check | Build status, then the page opened with a key. | VM | The same, from the webapp. | Webapp |
| The pages themselves | On Cloudflare. | Cloudflare | No change needed: same addresses, look, sign-in and data. | Cloudflare |
| Pages kept by hand | Action Hub, Scan Tracker, parts of the dashboards, RFI Sweep (the lead's PC). | By hand | Can stay by hand, from the webapp's chat. The RFI Sweep can stay on the lead's PC. | By hand |

## The lead

*People*

The only person on the bot's alert list (runner.json).

**Today:** The only person on the bot's alert list.

**What could change:** The lead keeps getting the same alerts at the same address. The same, now sent by the webapp.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Alert list | The lead only, set in runner.json on the VM. | VM | The same list, kept in the webapp's settings. | Webapp |

## Cloudflare: hosting and sign-in

*Service, Outside*

Puts each page online and checks who may open it (emailed code or Microsoft login). The bot checks its updates went live.

- `Cloudflare`: the hosting company
- `4 small databases`: what people type into the pages: notes, tasks, owners, records, sign-offs, Lead Queue decisions
- `1 file bucket`: laser-scan files
- `Page libraries`: some pages load code from public sites (jsDelivr, cdnjs)

**Today:** Puts each page online and checks who may open it. Holds the pages' 4 small databases, including the Task Board, and 1 scan file bucket.

**What could change:** Cloudflare stays as it is. The keys and scripts the bot and chats use move to the webapp. Unchanged. The live-check key and the tracker sync script move to the webapp.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Hosting, sign-in, databases, file bucket | Cloudflare. | Cloudflare | No change needed. | Cloudflare |
| Live-check key | On the VM. | VM | In the webapp's secure settings. | Webapp |
| Tracker sync script (tracker_sync_cf.py) | Only on the VM, not in a repo. | VM | Rescued into a repo, run by the webapp. | Webapp |
| Page libraries | Some pages load code from public sites (jsDelivr, cdnjs). | Cloudflare | No change needed. | Cloudflare |

## Page users

*People*

HCG's team and leadership, plus BES and Triad (their own items only). They also add notes, tasks and sign-offs.

**Today:** HCG's team and leadership, plus BES and Triad, who see their own items only.

**What could change:** Nothing changes for the people who use the pages. The same pages, addresses, sign-in and data.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Page users | HCG's team and leadership, plus BES and Triad, who see their own items only. | Cloudflare | No change needed. | Cloudflare |
