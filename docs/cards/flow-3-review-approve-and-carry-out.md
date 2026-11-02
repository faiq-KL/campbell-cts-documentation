# Flow 3: Review, approve and carry out

Every card in this flow, in full. The [plan page](https://faiq-kl.github.io/campbell-cts-documentation/vm-to-webapp-plan.html) shows the diagram.

## Hold risky changes for a person

Risky changes wait here until the client approves them.

*Waiting, Run log + notes*

Unusual results the bot or AI did not write. Listed in run summaries, notes and emails. Nothing is written until someone decides.

- `Register rows`: a drawing status went down, or a revision disappeared
- `Scan results`: progress measured from new scans
- `Broken page data`: for example, zero Forma issues. Big jumps are published with a warning instead (a rule the client set)
- `Finish dates with no PDF yet`: the model shows the package issued, but its drawing PDF isn't in Forma. Once it is, the bot writes the date itself
- `TimeLiner re-dates`: 4D schedule dates to change, from the sweep
- `Proposed Forma issues`: from meeting follow-ups
- `Flags marked "Decision: client"`: from the project diary

**Today:** Register rows that would go backwards, page data that fails its checks, and scan results are held for the client.

**What could change:** Held items work as today. The client approves in the webapp's chat and the webapp releases them. The client approves in chat, as today. The webapp runs the same release commands and records who approved and when.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Register rows | A work package's drawing status would go down, it would lose a drawing sheet, or a run would change far more rows than usual. Held and listed for the client in the run summary and emails. | Yes, shown in the webapp's run status. | Held the same way, shown in the webapp's run status and emails. | Webapp |
| Scan results | Progress measured from a new scan. Held until the client approves it. | Yes, shown in the webapp's run status. | Held the same way, shown in the webapp's run status. | Webapp |
| Broken page data | For example, zero Forma issues. Held until the client approves it. Big jumps are published with a warning instead (a rule the client set). | Yes, the webapp runs the checks. | The same checks and rule, in the webapp. | Webapp |
| Finish dates with no PDF yet | The bot waits until the drawing PDF is in Forma, then writes the date itself. | Yes, the date comes from Forma. | The same wait. The date could come from Forma's own upload date, read through the API, if the client agrees. | Webapp |
| TimeLiner re-dates | Proposed by the sweep, approved by the client in chat. | Yes, approved in the webapp's chat. | Proposed by the webapp's sweep, approved in the webapp's chat. | Webapp |
| Proposed Forma issues | Suggested in meeting follow-up emails, created in chat after the client approves. | Yes, created through Autodesk's API. | Suggested the same way, created through Autodesk's Issues API after approval. | Webapp |
| Flags marked for the client | Raised in the project diary for the client to decide. | Yes, answered in the webapp's chat. | The same flags in the notes, answered by the client in the webapp's chat. | Webapp |
| Schedule links to re-check | When an ARCO schedule update reuses or removes activity numbers, links the sweep cannot match are marked for re-check and listed in its email for the client. | Yes, answered in the webapp's chat. | Listed the same way by the webapp's sweep, answered by the client in the webapp's chat. | Webapp |

## Make the project decisions

The client makes the project decisions, and a deputy can stand in.

*People*

The client decides (the deputy has stood in). VKU has its own small team.

**Today:** The client decides. The deputy has stood in through a written note. VKU has its own small team.

**What could change:** The same people decide in the same way. Unchanged. A stand-in is still set by a written note. Each person signs in to the webapp, so only the client or the stand-in can approve.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Stand-in | A written note from the client, kept in the project notes. | Yes, the webapp checks who may approve. | The same note, kept with the notes in git. The webapp follows it, so only the client or the stand-in can approve. | Webapp |

## Sign off schedule cards

Six people at Triad approve schedule cards on their sign-off page.

*People*

Six people at Triad who approve, change or reject schedule cards.

**Today:** Six people at Triad approve, change or mark schedule cards Not ours.

**What could change:** The six Triad signers keep using the same sign-off page. Unchanged.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Triad signers | The sign-off page. | No, they keep their sign-off page. | No change needed. |  |

## Queue decisions for the client

The Lead Queue page lists 137 decisions waiting for the client.

*Repo page, In git*

One page listing every decision waiting for the client. The client approves, changes, holds or passes on each. The AI then carries it out in chat.

- `Nova102-Action-Hub`: page /lead
- `lead/seed.json`: the list, filled by hand from project notes
- `LEAD_SYNC_CLIENT_ID`: key for the AI to read it directly: not set up yet

**Today:** A page listing every decision waiting for the client (137 items), filled by hand into lead/seed.json. A click does not count as approval yet.

**What could change:** The Lead Queue stays as it is, filled by hand. Unchanged, filled from the webapp's chat.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| lead/seed.json | Filled by hand. | No, it's still filled by hand. | No change needed. |  |
| Lead Queue page | A page in the Action Hub listing 137 decisions. The client marks each one approve, change, hold or close, and every choice is logged. Claude acts on approvals given in chat. | No, the page stays on Cloudflare. | No change needed. |  |

## Record Triad's sign-offs

A page that saves each Triad sign-off, which Claude applies after the client approves.

*Repo, In git*

Saves each Triad decision with the signer's email. It never changes the schedule. The AI applies sign-offs in chat.

- `Nova102-Action-Hub`: page /triad-board

**Today:** Saves each Triad decision with the signer's email. Claude applies validated cards in chat after the client approves.

**What could change:** The sign-off board stays as it is. Sign-offs are applied only after the client approves. Unchanged, applied from the webapp's chat.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Applying sign-offs | By Claude in chat, after the client approves. | Yes, applied from the webapp's chat. | The same, in the webapp's chat. | Webapp |
| Sign-off board page | Saves each Triad decision with the signer's email. It never changes the schedule. | No, the page stays on Cloudflare. | No change needed. |  |

## Do work when asked

Work Claude does only when asked, like 4D builds, clash reviews and document control.

*Procedures, Git + Claude account*

Not daily. Started on request, each following its rule-book procedure.

- `BIM-10 prefab and field`: prefab checks, shop schedules, the supplier's Trello board
- `BIM-12 clash and issues`: clash reviews, filing the client's clash report, clash issues in Forma (hidden until a person publishes them)
- `BIM-13 field BOM`: material quantities from Navisworks
- `BIM-11 4D builds`: 4D model rebuilds, with a person watching
- `BIM-04 document control`: drawings in hand, and publishing drawings to Forma
- `BIM-08 RFI and submittal questions`: answers about RFIs and submittals
- `Forma correspondence`: filing emails and letters in Forma (including a catch-up batch)

**Today:** 4D builds (BIM-11), clash reviews (BIM-12), quantities (BIM-13), document control (BIM-04), RFI questions (BIM-08), Forma correspondence, and prefab and Trello work (BIM-10), in chats using the VM.

**What could change:** Work done when asked runs in the webapp's chat. Revit and Navisworks work reaches the VM through conduit. The same procedures from the webapp's chat, through conduit and the official APIs.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| 4D builds, clash reviews and quantities (BIM-11, BIM-12, BIM-13) | Navisworks and Revit on the VM, through conduit. | No, it needs Revit and Navisworks. | No change needed. Stays on the VM, reached from the webapp's chat through conduit. | VM |
| BIM-04, BIM-08, BIM-10, Forma correspondence | Chats working through the VM. | Yes, done through Autodesk's, Procore's and Trello's APIs. | Autodesk's, Procore's and Trello's APIs. | Webapp |

**Watch out:**

- 4D builds are watched by a person today. The webapp needs a way to show Navisworks' view.

## Work with the AI in chat

The client and the team ask Claude for work and give approvals in chat.

*AI in chat, Claude account*

People working with Claude in the Nova BIM Schedule and VKU projects. Shared changes are claimed on the Task Board first, and kept out of the bot's run times (09:00 to 10:15 and 16:00 to 17:15).

- `"Start my tasks for the day"`: a teammate's first message: run summary and QC checklist
- `Decisions and approvals`: The client approves exact changes
- `Task Board, issue drafts, Monday expectation gap check`: other chat work
- `Manual run steps`: only when the client asks
- `Teammates' own Claude`: with the company skills
- `Claude's sandbox`: some tools ran on uploaded copies, not real files

**Today:** People chat with Claude in the Nova BIM Schedule and VKU projects. The client's chats work through the VM. The deputy approved in their own Claude, in the shared project.

**What could change:** Chat moves into the webapp for the client, the deputy and the teammates. A chat page in the webapp, on the same engine as the jobs, with the same words and procedures, such as 'start my tasks for the day'. Each person signs in. Approvals are recorded with name and time. Task Board claims go through the portal's API.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Start my tasks for the day | In the Claude app, reading the run log through the VM. | Yes, started from the webapp's chat. | In the webapp's chat. | Webapp |
| Approvals | Typed in chat. Claude then carries out the exact approved action through the VM, such as the bot's commands. | Yes, the webapp records and carries them out. | Typed in the webapp's chat and recorded with name and time. Only the client or the stand-in can approve. The webapp carries them out. | Webapp |
| Manual runs | Only when the client asks. | Yes, started from the webapp's chat. | From the webapp's chat, same rule. | Webapp |
| The deputy and teammates | Their own Claude, on their own computers. | Yes, they sign in to the webapp. | Their own sign-in to the webapp. They keep their own Claude for other work. | Webapp |
| Claude app extras (phone app, voice, artifacts) | In the Claude app. | Yes, the webapp's chat works on phones and takes files. | Suggested: the webapp's chat works in a phone's browser, takes files and images, and shows what Claude makes. Voice is added only if the client uses it. | Webapp |
| Task Board claims | Before changing a shared file, the task is claimed on the Task Board, through a browser signed in as a person, or by a person by hand. | Yes, Claude claims it through the portal's API. | Claude claims it through the portal's API, with its own sign-in. Needs a small portal change. | Webapp |
| Other chat work | Issue drafts for the client to approve. | Yes, done in the webapp's chat. | The same work in the webapp's chat. | Webapp |
| Run-time rule | Shared changes are kept out of the bot's run times (09:00 to 10:15 and 16:00 to 17:15). | Yes, the webapp checks it before every shared change. | The same rule, checked by the webapp before any shared write. | Webapp |

**Chat options, as weighed**

| Option | What it means | Finding |
|---|---|---|
| Our own chat in the webapp | A chat page built on the same engine as the jobs. | Suggested. |
| As today, on the VM | The Claude app working through that computer. | Not suggested. It keeps the app and the client's sign-in on that computer. |
| Claude app plus a connector | Claude reaches the webapp through a list of tools. | Not suggested. Chat would stay in the Claude app, not in our webapp. |

Suggested so that nobody needs a computer with the Claude app.

## Carry out approved changes

After approval, Claude changes the schedule, files and issues, and drafts replies.

*Service, Outside*

Only what was approved is written.

- `Coordination Schedule`: finish dates, held rows, Triad-approved cards
- `4D approval files`: applied by the next morning's bot run
- `Tracker shared state`: page data, saved with tracker_sync_cf.py on the VM
- `Forma issues`: clash issues stay hidden until a person publishes them. Questions to Triad go out open
- `Replies to BES, ARCO, CMMC, Triad`: drafted by the AI, sent after the client checks the evidence

**Today:** Coordination Schedule changes, 4D approval files, tracker notes, Forma issues and reply drafts, written from chats using the VM.

**What could change:** Approved changes are written from the webapp, only after approval, outside run times, with the same safety steps. The same changes from the webapp's chat, through Autodesk's API.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Coordination Schedule changes | From chats using the VM. | Yes, written through Autodesk's API. | From the webapp's chat, through the API. | Webapp |
| 4D approval files | On the VM. | Yes, the webapp's chat writes them. | Written by the webapp's chat, sent with the morning Navisworks job. | Webapp |
| Tracker shared state | Saved with tracker_sync_cf.py on the VM. A copy is on a side branch of the bot's repo, not in its main code. | Yes, the webapp runs the script from GitHub. | The same script, checked against the VM's copy and merged into the bot's repo, run by the webapp. | Webapp |
| Forma issues | Made in Forma in the browser. Clash issues start in the 3D viewer, questions to Triad in the issue form. Each stays a draft until a person publishes it. | Yes, created through Autodesk's API. | Autodesk's Issues API, still created as drafts. | Webapp |
| Replies to BES, ARCO, CMMC, Triad | Drafted by Claude, sent by the client after checking. | Yes, drafted in the webapp's chat. | The same, drafted in the webapp's chat and sent by the client after checking. | Webapp |

**Watch out:**

- An old project note still shows a method that copies a sign-in token. The rule book withdrew it, and the API keeps it out.

## Release held items

Commands that release a held change once the client approves it.

*Repo commands, In git*

Commands that publish what the bot held, once approved.

- `python -m orchestrator site-feed-approve`: held page data
- `python -m orchestrator scan-approve`: held scan results

**Today:** approve-regression, site-feed-approve and scan-approve, run on the VM.

**What could change:** The three release commands run in the webapp. The same commands, run by the webapp from chat.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Release a held register change (approve-regression) | Run on the VM from chat. It records the approval, and the next run writes the change. | Yes, the webapp runs it from chat. | Run by the webapp from chat. The next run writes the change, as today. | Webapp |
| Release held page data (site-feed-approve) | On the VM. | Yes, the webapp runs it from chat. | In the webapp. | Webapp |
| Release a held scan result (scan-approve) | On the VM. | Yes, the webapp runs it from chat. | In the webapp. | Webapp |

## Update pages by hand

Some pages are updated by hand by the client, not by the bot.

*Repos, In git*

Pages, or parts of pages, the bot does not update.

- `Nova102-RFI-Sweep`: from the client's PC (it holds the search index)
- `Nova102-Scan-Tracker`: after a scan is approved
- `Nova102-Impact-Dashboard`: punch list, open items, expectation gaps
- `Nova102-Action-Hub`: main to-do list and Lead Queue items
- `HCG-Portfolio-Dashboard`: decisions and highlights

**Today:** Action Hub, Scan Tracker, and parts of the dashboards and the portfolio, with their own scripts. The RFI Sweep is updated from the client's PC, which holds its search index.

**What could change:** Pages kept by hand stay by hand. Their scripts run in the webapp. The same pages, kept by hand from the webapp's chat. The RFI Sweep stays on the client's PC.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Hand updates and their scripts | Done by the client in Claude coding sessions, not from the VM. The Scan Tracker tools are also set up on the VM. | Yes, done from the webapp's chat. | Could be done from the webapp's chat. | Webapp |
| RFI Sweep | From the client's PC. | No, it stays on the client's PC. | No change needed. |  |

## Change the rules

Claude proposes rule changes in chat, and the client approves them.

*Repo, In git*

The client's rulings update the rule book, then the skills and workbooks are rebuilt.

- `Rule book`: the repo
- `kb.py build`: ClickUp workbooks and flowcharts
- `kb.py plugin`: the playbook skills

**Today:** Claude proposes the rule-book change in chat. The client approves, and the rule book is updated.

**What could change:** Rule changes go into the rule book as today. The same, from the webapp's chat.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Rule changes | Claude proposes the rule-book change in chat. The client approves, and the rule book is updated. | Yes, proposed in the webapp's chat. | The same, proposed in the webapp's chat. | Webapp |
| Project instructions | New text is drafted in chat. The client pastes it into the Claude Project. | Yes, kept on GitHub and changed from the webapp's chat. | Could be kept in git with the notes and changed from the webapp's chat after the client approves. | Webapp |
