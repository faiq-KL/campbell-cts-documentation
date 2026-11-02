# Decisions and needs

What we decided for each flow, and what we need to get there. The [plan page](https://faiq-kl.github.io/campbell-cts-documentation/vm-to-webapp-plan.html) shows how each flow works today, card by card.

## Rules

- **Rule 1:** what things do stays the same. Only where they run changes.
- **One design:** we plan only the finished setup, not a before and after. Approvals and tests come before it goes live.
- **API first:** if an official API exists, we use it. A desktop program stays only where no API exists.

## Flow 1: Update project data

### What we decided

- **Where it runs:** the bot's own code runs in the webapp on Chicago time, with its memory and settings at the same values. Only the Revit and Navisworks steps run on the Windows computer, sent over the private link. The pop-up clicker and the session keeper stay there. The Claude keep-alive goes.
- **Timer:** weekdays at 09:00 and 16:00 Chicago time, holidays included, NOVA102 then VKU, at most once per run time. A missed run starts when the webapp is back. Manual runs come from chat, only when the lead asks, and count as the next run.
- **One timing change:** the 30-minute morning wait is dropped, so morning results arrive 30 minutes earlier. One setting brings it back.
- **Files in:** every input comes from ACC through Autodesk's API, including Forma issues and RFIs, with no Chrome. Issued drawings take Forma's own upload date. The lead has the final say, because some Actual Finish dates can shift.
- **Files out:** the same files go to the same ACC folders through the API, with the same 6 safety steps and the same backups. ACC's history will show our app's account as the author.
- **Navisworks' files:** the file helper moves them to and from ACC through Autodesk's API. Desktop Connector goes.
- **Scans, 4D and 5D:** the scan check, the 4D viewer, 4D progress and the 5D workbook run in the webapp with the same code, on a worker sized for the biggest scan. The Revit export and the Navisworks timeline snapshot stay on the Windows computer.
- **Alerts:** the same emails through Amazon's email, from the same sender, to the lead only. Teams stays off.
- **Pages:** the 10 pages stay exactly as they are. The webapp publishes the same data to the same 7 with its own GitHub access, so GitHub's history shows its name, and runs the same live check. Pages kept by hand stay by hand.
- **Cloudflare:** stays as it is. The live-check key and the tracker sync script move to the webapp. Claude claims Task Board items through the portal's API, with its own sign-in.

Noted for later: the bot's Revit reading can move to Autodesk's online Revit (Automation API) after a side-by-side test. Parked: clash checking in ACC instead of Navisworks, and a Run now button. Dropped: one app replacing the 10 pages, and Autodesk's free data service.

### What we need

1. **Triad, Newtron and ARCO** connect our app to their Autodesk accounts: read access to the project files, and write access only where the bot writes today. The lead tells us who their Autodesk admins are.
2. **An Autodesk account for the Windows computer**, not the lead's, with Revit, Navisworks Manage and Forma Design Collaboration licences, added to the three companies' projects.
3. **From the lead:** which Navisworks licence the VM uses and when it renews, who opens the NWD, their OK to run conduit on the new computer with a network mode, and their OK on the drawing dates after a comparison shows how many move.
4. **To build:** conduit's network mode or bridge, the file helper, and the private link.
5. **From the VM:** everything on the [What we still need from the VM](https://faiq-kl.github.io/campbell-cts-documentation/vm-to-webapp-plan.html#missing) list, starting with the clash export step and the newest Navisworks bridge source. Also the bot's memory and settings, the tracker sync script and the email script.
6. **Access for the webapp:** Amazon email, and a Cloudflare key for the live check.
7. **Task Board:** The lead's OK and a small portal change, so Claude can claim tasks with its own sign-in.

## Flow 2: Run scheduled AI jobs

### What we decided

- **Engine:** Claude runs in the webapp on Anthropic's Agent SDK, the engine behind Claude Code, with an API key. It is paid first from the API credit in the lead's Claude plan. There is no Claude app on any computer, so the keep-alive goes.
- **Jobs:** the 7 timed jobs that use the VM run in the webapp with the same AI, instructions, timing, notes and emails: the RFI and submittal sweep, Project Memory and flags, the change log, the VKU check, meeting prep, and the sweep and run watchdogs.
- **Heartbeat:** stays outside the webapp, because a watcher must never run on the thing it watches.
- **Connections:** official APIs or official tool servers for Procore, Autodesk, Microsoft 365, Fireflies (never for VKU) and GitHub. Claude's own connectors only work inside claude.ai.
- **Notes:** the 357 project files and the VKU notes become plain files in a private git repo, with the same names, so no instruction changes. Claude's project memory is copied in once, and chat history is kept in the webapp. Claude opens notes by name and searches inside them. A meaning-based search is added only if a test shows misses.
- **Skills and rule book:** the same skills, loaded by the webapp from git. The rule book stays as it is.
- **Filing and emails:** the sweep files into the same ACC folders through the API and updates the tracker page from the webapp. Emails go out through Amazon's email, from the same sender to the same people.
- **No double sends:** every job's send logs and saved state are copied over, so no job starts fresh and sends twice.
- **Going live:** each job goes live only after a side-by-side check against today.

Not carried over: the 2 finished acting-lead digests and the 3 finished one-off jobs. Their text is kept on record. Google Drive is connected but unused, so there is nothing to replace unless the lead says otherwise. Not taken: Managed Agents, the Claude app on a computer, and plain API calls.

### What we need

1. **Anthropic:** an API key, and the lead's Claude plan, which sets the monthly API credit.
2. **Procore:** The lead's OK to use its API, since they set a no-API rule, and ARCO's Procore admin connecting our app.
3. **Microsoft:** The lead's consent for Outlook, Teams and calendar. Newtron's IT may need to approve too.
4. **Fireflies:** The lead's key. Hourly meeting prep likely needs the Business plan, since Free and Pro allow only 50 API requests a day.
5. **Autodesk:** the same connections as Flow 1, for the sweep's filing and the VKU check.
6. **From the VM:** every job's send logs and saved state, and the email script.
7. **From the lead's Claude account:** the notes, the VKU notes, Claude's project memory and the whats-on-my-plate skill. The lead tells us where the skills starter folder is.
8. **From the lead:** a screenshot of their Connectors page, whether anything uses Google Drive, and whether Greenhead (pinned in their Claude account) is part of this system.

## Flow 3: Review and approve

### What we decided

- **Chat:** moves into the webapp for the lead, the deputy and the teammates, on the same engine as the jobs, with the same words and procedures, such as 'start my tasks for the day'. Each person signs in. The deputy and the teammates keep their own Claude for other work.
- **Approvals:** typed in chat as today. The webapp carries them out and records who approved and when. Only the lead or the stand-in can approve, and a stand-in is still set by a written note.
- **Held items:** released by the webapp with the same commands, for register changes, page data and scan results.
- **Approved changes:** written from chat only after approval, outside run times, with the same safety steps. This covers Coordination Schedule changes, 4D approval files and tracker notes.
- **Forma issues:** created through Autodesk's Issues API, still as drafts. No sign-in token is copied from a browser anymore.
- **Work done when asked:** 4D builds, clash reviews and quantities reach Revit and Navisworks through conduit on the Windows computer. Document control, RFI questions, prefab and Trello work, and Forma correspondence use Autodesk's, Procore's and Trello's APIs.
- **Unchanged:** the six Triad signers and their sign-off page (applied only after the lead approves), the Lead Queue (filled by hand), replies to BES, ARCO, CMMC and Triad (drafted by Claude, sent by the lead), and rule changes (approved by the lead).
- **Pages kept by hand:** stay by hand, with their scripts in the webapp. The RFI Sweep stays on the lead's PC.

Dropped: filling the Lead Queue automatically, a stand-in role set for a date range, and the RFI Sweep's search index on our server. Not taken: keeping chat in the Claude app, with or without a connector to the webapp.

### What we need

1. **Sign-ins and roles** in the webapp for the lead, the deputy and the teammates, including the VKU team.
2. **From the lead:** their list of Claude app extras, and their answer on LQ-000: should a signed-in click on the Lead Queue count as approval?
3. **Trello:** API access for the prefab and Trello work.
4. **Autodesk and Procore:** the connections from Flows 1 and 2, including Autodesk's Issues API for Forma issues.
5. **The tracker sync script** saved into a repo, for the tracker notes.

## Flow 4: Respond to incidents

### What we decided

- **Alarms:** the same watchers and the bot's alert emails, now from the webapp. They only report, they never fix.
- **Independent alarm:** the heartbeat stays outside the webapp, so the lead still hears when the webapp itself is down.
- **Who decides:** the lead, or the stand-in, as today.
- **Fixing:** starts from the webapp's run status, logs and job results. The Claude app and the Procore and Forma sign-ins are gone.
- **Windows problems:** rare Revit, Navisworks or Windows problems are fixed by us on the Windows computer, not by the lead.
- **Re-running a step:** a chat request, only when the lead asks, outside run times, with the same skip rule.

### What we need

1. **Rewritten directions:** the 'what to do' part of every watcher's email, pointing at the webapp and the Windows computer instead of the VM. Everything else in those emails stays the same. The lead sees the final wording.

Not worked out yet: Flow 5 (Sell and support Conduit), Flow A (Release code changes) and Flow B (Onboard a new project).
