# Flow 3: Review, approve and carry out

Every card in this flow, in full. The [plan page](https://faiq-kl.github.io/campbell-cts-documentation/vm-to-webapp-plan.html) shows the diagram.

## Held for a person

*Waiting, Run log + notes*

Unusual results the bot or AI did not write. Listed in run summaries, notes and emails. Nothing is written until someone decides.

- `Register rows`: a drawing status went down, or a revision disappeared
- `Scan results`: progress measured from new scans
- `Broken page data`: for example, zero Forma issues. Big jumps are published with a warning instead (a rule the lead set)
- `Finish dates with no PDF yet`: the model shows the package issued, but its drawing PDF isn't in Forma. Once it is, the bot writes the date itself
- `TimeLiner re-dates`: 4D schedule dates to change, from the sweep
- `Proposed Forma issues`: from meeting follow-ups
- `Flags marked "Decision: lead"`: from the project diary

**Today:** Register rows that would go backwards, page data that fails its checks, and scan results are held for the lead.

**What could change:** Held items work as today. The lead approves in the webapp's chat and the webapp releases them. The lead approves in chat, as today. The webapp runs the same release commands and records who approved and when.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Register rows | A work package's drawing status would go down, it would lose a drawing sheet, or a run would change far more rows than usual. Held and listed for the lead in the run summary and emails. | VM | Held the same way, shown in the webapp's run status and emails. | Webapp |
| Scan results | Progress measured from a new scan. Held until the lead approves it. | VM | Held the same way, shown in the webapp's run status. | Webapp |
| Broken page data | For example, zero Forma issues. Held until the lead approves it. Big jumps are published with a warning instead (a rule the lead set). | VM | The same checks and rule, in the webapp. | Webapp |
| Finish dates with no PDF yet | The bot waits until the drawing PDF is in Forma, then writes the date itself. | VM | The same wait. The date could come from Forma's own upload date, read through the API, if the lead agrees. | Webapp |
| TimeLiner re-dates | Proposed by the sweep, approved by the lead in chat. | Claude + VM | Proposed by the webapp's sweep, approved in the webapp's chat. | Webapp |
| Proposed Forma issues | Suggested in meeting follow-up emails, created in chat after the lead approves. | Claude + VM | Suggested the same way, created through Autodesk's Issues API after approval. | Webapp |
| Flags marked for the lead | Raised in the project diary for the lead to decide. | Claude account | The same flags in the notes, answered by the lead in the webapp's chat. | Webapp |
| Schedule links to re-check | When an ARCO schedule update reuses or removes activity numbers, links the sweep cannot match are marked for re-check and listed in its email for the lead. | Claude + VM | Listed the same way by the webapp's sweep, answered by the lead in the webapp's chat. | Webapp |

## Project Lead and team

*People*

The lead decides (the deputy has stood in). VKU has its own small team.

**Today:** The lead decides. The deputy has stood in through a written note. VKU has its own small team.

**What could change:** The same people decide in the same way. Unchanged. A stand-in is still set by a written note. Each person signs in to the webapp, so only the lead or the stand-in can approve.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Stand-in lead | A written note from the lead, kept in the project notes. | Claude account | The same note, kept with the notes in git. The webapp follows it, so only the lead or the stand-in can approve. | Git |

## Triad signers

*People*

Six people at Triad who approve, change or reject schedule cards.

**Today:** Six people at Triad approve, change or mark schedule cards Not ours.

**What could change:** The six Triad signers keep using the same sign-off page. Unchanged.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Triad signers | The sign-off page. | Cloudflare | No change needed. | Cloudflare |

## Lead Queue

*Repo page, In git*

One page listing every decision waiting for the lead. The lead approves, changes, holds or passes on each. The AI then carries it out in chat.

- `Nova102-Action-Hub`: page /lead
- `lead/seed.json`: the list, filled by hand from project notes
- `LEAD_SYNC_CLIENT_ID`: key for the AI to read it directly: not set up yet

**Today:** A page listing every decision waiting for the lead (137 items), filled by hand into lead/seed.json. A click does not count as approval yet.

**What could change:** The Lead Queue stays as it is, filled by hand. Unchanged, filled from the webapp's chat.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| lead/seed.json | Filled by hand. | By hand | No change needed. | By hand |
| Lead Queue page | A page in the Action Hub listing 137 decisions. The lead marks each one approve, change, hold or close, and every choice is logged. Claude acts on approvals given in chat. | Cloudflare | No change needed. | Cloudflare |

## Sign-off board

*Repo, In git*

Saves each Triad decision with the signer's email. It never changes the schedule. The AI applies sign-offs in chat.

- `Nova102-Action-Hub`: page /triad-board

**Today:** Saves each Triad decision with the signer's email. Claude applies validated cards in chat after the lead approves.

**What could change:** The sign-off board stays as it is. Sign-offs are applied only after the lead approves. Unchanged, applied from the webapp's chat.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Applying sign-offs | By Claude in chat, after the lead approves. | Claude + VM | The same, in the webapp's chat. | Webapp |
| Sign-off board page | Saves each Triad decision with the signer's email. It never changes the schedule. | Cloudflare | No change needed. | Cloudflare |

## Work done when asked

*Procedures, Git + Claude account*

Not daily. Started on request, each following its rule-book procedure.

- `BIM-10 prefab and field`: prefab checks, shop schedules, the supplier's Trello board
- `BIM-12 clash and issues`: clash reviews, filing the lead's clash report, clash issues in Forma (hidden until a person publishes them)
- `BIM-13 field BOM`: material quantities from Navisworks
- `BIM-11 4D builds`: 4D model rebuilds, with a person watching
- `BIM-04 document control`: drawings in hand, and publishing drawings to Forma
- `BIM-08 RFI and submittal questions`: answers about RFIs and submittals
- `Forma correspondence`: filing emails and letters in Forma (including a catch-up batch)

**Today:** 4D builds (BIM-11), clash reviews (BIM-12), quantities (BIM-13), document control (BIM-04), RFI questions (BIM-08), Forma correspondence, and prefab and Trello work (BIM-10), in chats using the VM.

**What could change:** Work done when asked runs in the webapp's chat. Revit and Navisworks work reaches the Windows computer through conduit. The same procedures from the webapp's chat, through conduit and the official APIs.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| 4D builds, clash reviews and quantities (BIM-11, BIM-12, BIM-13) | Navisworks and Revit on the VM, through conduit. | VM | Through conduit on the Windows computer. | Windows computer |
| BIM-04, BIM-08, BIM-10, Forma correspondence | Chats working through the VM. | Claude + VM | Autodesk's, Procore's and Trello's APIs. | Webapp |

**Watch out:**

- 4D builds are watched by a person today. The webapp needs a way to show Navisworks' view.

## AI chat sessions

*AI in chat, Claude account*

People working with Claude in the Nova BIM Schedule and VKU projects. Shared changes are claimed on the Task Board first, and kept out of the bot's run times (09:00 to 10:15 and 16:00 to 17:15).

- `"Start my tasks for the day"`: a teammate's first message: run summary and QC checklist
- `Decisions and approvals`: The lead approves exact changes
- `Task Board, issue drafts, Monday expectation gap check`: other chat work
- `Manual run steps`: only when the lead asks
- `Teammates' own Claude`: with the company skills
- `Claude's sandbox`: some tools ran on uploaded copies, not real files

**Today:** People chat with Claude in the Nova BIM Schedule and VKU projects. The lead's chats work through the VM. The deputy approved in their own Claude, in the shared project.

**What could change:** Chat moves into the webapp for the lead, the deputy and the teammates. A chat page in the webapp, on the same engine as the jobs, with the same words and procedures, such as 'start my tasks for the day'. Each person signs in. Approvals are recorded with name and time. Task Board claims go through the portal's API.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Start my tasks for the day | In the Claude app, reading the run log through the VM. | Claude + VM | In the webapp's chat. | Webapp |
| Approvals | Typed in chat. Claude then carries out the exact approved action through the VM, such as the bot's commands. | Claude + VM | Typed in the webapp's chat and recorded with name and time. Only the lead or the stand-in can approve. The webapp carries them out. | Webapp |
| Manual runs | Only when the lead asks. | Claude + VM | From the webapp's chat, same rule. | Webapp |
| The deputy and teammates | Their own Claude, on their own computers. | Claude account | Their own sign-in to the webapp. They keep their own Claude for other work. | Webapp |
| Claude app extras (phone app, voice, artifacts) | In the Claude app. | Claude account | Suggested: the webapp's chat works in a phone's browser, takes files and images, and shows what Claude makes. Voice is added only if the lead uses it. | To explore |
| Task Board claims | Before changing a shared file, the task is claimed on the Task Board, through a browser signed in as a person, or by a person by hand. | Browser | Claude claims it through the portal's API, with its own sign-in. Needs a small portal change. | Webapp |
| Other chat work | Issue drafts for the lead to approve. | Claude + VM | The same work in the webapp's chat. | Webapp |
| Run-time rule | Shared changes are kept out of the bot's run times (09:00 to 10:15 and 16:00 to 17:15). | Claude + VM | The same rule, checked by the webapp before any shared write. | Webapp |

**Chat options, as weighed**

| Option | What it means | Finding |
|---|---|---|
| Our own chat in the webapp | A chat page built on the same engine as the jobs. | Suggested. |
| As today, on the Windows computer | The Claude app working through that computer. | Not suggested. It keeps the app and the lead's sign-in on that computer. |
| Claude app plus a connector | Claude reaches the webapp through a list of tools. | Not suggested. Chat would stay in the Claude app, not in our webapp. |

Suggested so that nobody needs a computer with the Claude app.

## Approved changes, issues and replies

*Service, Outside*

Only what was approved is written.

- `Coordination Schedule`: finish dates, held rows, Triad-approved cards
- `4D approval files`: applied by the next morning's bot run
- `Tracker shared state`: page data, saved with tracker_sync_cf.py on the VM
- `Forma issues`: clash issues stay hidden until a person publishes them. Questions to Triad go out open
- `Replies to BES, ARCO, CMMC, Triad`: drafted by the AI, sent after the lead checks the evidence

**Today:** Coordination Schedule changes, 4D approval files, tracker notes, Forma issues and reply drafts, written from chats using the VM.

**What could change:** Approved changes are written from the webapp, only after approval, outside run times, with the same safety steps. The same changes from the webapp's chat, through Autodesk's API.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Coordination Schedule changes | From chats using the VM. | Claude + VM | From the webapp's chat, through the API. | Webapp |
| 4D approval files | On the VM. | VM | Written by the webapp's chat, sent with the morning Navisworks job. | Webapp |
| Tracker shared state | Saved with tracker_sync_cf.py on the VM. A copy is on a side branch of the bot's repo, not in its main code. | VM | The same script, checked against the VM's copy and merged into the bot's repo, run by the webapp. | Webapp |
| Forma issues | Made in Forma in the browser. Clash issues start in the 3D viewer, questions to Triad in the issue form. Each stays a draft until a person publishes it. | Browser | Autodesk's Issues API, still created as drafts. | Webapp |
| Replies to BES, ARCO, CMMC, Triad | Drafted by Claude, sent by the lead after checking. | Claude account | The same, drafted in the webapp's chat and sent by the lead after checking. | Webapp |

**Watch out:**

- An old project note still shows a method that copies a sign-in token. The rule book withdrew it, and the API keeps it out.

## Release held items

*Repo commands, In git*

Commands that publish what the bot held, once approved.

- `python -m orchestrator site-feed-approve`: held page data
- `python -m orchestrator scan-approve`: held scan results

**Today:** approve-regression, site-feed-approve and scan-approve, run on the VM.

**What could change:** The three release commands run in the webapp. The same commands, run by the webapp from chat.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Release a held register change (approve-regression) | Run on the VM from chat. It records the approval, and the next run writes the change. | VM | Run by the webapp from chat. The next run writes the change, as today. | Webapp |
| Release held page data (site-feed-approve) | On the VM. | VM | In the webapp. | Webapp |
| Release a held scan result (scan-approve) | On the VM. | VM | In the webapp. | Webapp |

## Pages kept by hand

*Repos, In git*

Pages, or parts of pages, the bot does not update.

- `Nova102-RFI-Sweep`: from the lead's PC (it holds the search index)
- `Nova102-Scan-Tracker`: after a scan is approved
- `Nova102-Impact-Dashboard`: punch list, open items, expectation gaps
- `Nova102-Action-Hub`: main to-do list and Lead Queue items
- `HCG-Portfolio-Dashboard`: decisions and highlights

**Today:** Action Hub, Scan Tracker, and parts of the dashboards and the portfolio, with their own scripts. The RFI Sweep is updated from the lead's PC, which holds its search index.

**What could change:** Pages kept by hand stay by hand. Their scripts run in the webapp. The same pages, kept by hand from the webapp's chat. The RFI Sweep stays on the lead's PC.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Hand updates and their scripts | Done by the lead in Claude coding sessions, not from the VM. The Scan Tracker tools are also set up on the VM. | Claude account | Could be done from the webapp's chat. | Webapp |
| RFI Sweep | From the lead's PC. | Lead's PC | No change needed. | Lead's PC |

## Rule changes

*Repo, In git*

The lead's rulings update the rule book, then the skills and workbooks are rebuilt.

- `Rule book`: the repo
- `kb.py build`: ClickUp workbooks and flowcharts
- `kb.py plugin`: the playbook skills

**Today:** Claude proposes the rule-book change in chat. The lead approves, and the rule book is updated.

**What could change:** Rule changes go into the rule book as today. The same, from the webapp's chat.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Rule changes | Claude proposes the rule-book change in chat. The lead approves, and the rule book is updated. | Claude + VM | The same, proposed in the webapp's chat. | Webapp |
| Project instructions | New text is drafted in chat. The lead pastes it into the Claude Project. | Claude account | Could be kept in git with the notes and changed from the webapp's chat after the lead approves. | Git |
