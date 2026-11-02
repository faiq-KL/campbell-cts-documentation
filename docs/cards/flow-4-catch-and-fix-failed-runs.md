# Flow 4: Catch and fix failed runs

Every card in this flow, in full. The [plan page](https://faiq-kl.github.io/campbell-cts-documentation/vm-to-webapp-plan.html) shows the diagram.

## Who raises the alarm

*Schedulers, Claude account + VM*

They only report. None of them fixes anything.

- `NOVA102 BIM Assistant run watchdog (VM)` (11:40 and 17:40): The bot's run is missing, failed or incomplete.
- `NOVA102 RFI sweep watchdog (VM)` (Weekdays 09:05): The sweep failed, sent no email, or Procore is signed out.
- `VKU BIM Assistant shadow + run summary email (VM)` (:25 past each hour after a run): Problem email for VKU.
- `BIM runner VM heartbeat (cloud)` (12:35 and 19:35): Phone push if the VM's jobs stopped.
- `The bot's own alerts` (during each run): Email and pop-up.

**Today:** The run watchdog, the sweep watchdog, the VKU check, the heartbeat (push), and the bot's emails and pop-ups. They only report.

**What could change:** The same alarms, now from the webapp, plus one independent alarm outside it. The watchers and the bot's alerts come from the webapp. The heartbeat stays outside the webapp, suggested as a small task in the lead's Claude account that checks the webapp's status page.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| AI watchers | In Claude, using the VM. | Claude + VM | In the webapp. | Webapp |
| The bot's alert emails | From the VM. | VM | From the webapp. | Webapp |
| Heartbeat | In Claude's cloud. | Claude cloud | Suggested: a small task in the lead's Claude account that checks the webapp's status page, with the same push and email. | To explore |
| The bot's pop-ups | On the VM's screen, during each run. | VM | Suggested: a webapp notification on phone or browser. | To explore |
| What-to-do lines in the alarms | Each watcher's email says what to do, such as remote desktop into the VM, read the progress log, or start the VM in Amazon's console. | Claude + VM | Could point to the webapp instead of the VM. The lead checks the new wording. | Webapp |

## The lead, or the stand-in

*People*

Reads the alert and decides what to do.

**Today:** Reads the alert and decides what to do.

**What could change:** The lead, or the stand-in, reads the alert and decides. Unchanged.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Who decides | The lead or the stand-in. | People | No change needed. | People |

## Fix it on the VM

*On the VM, On the VM*

Remote desktop into the VM, then check:

- `logs\progress_<date>.log`: how far the run got
- `Is the Claude app open?`: switch back on any switched-off jobs
- `Do Revit 2025 / 2026 respond?`: if not, restart them
- `Is Procore signed in?`: in the VM's Chrome and the Claude app's browser
- `Is the Forma download signed in?`: sign in with: python -m site_feeds.forma_export --signin

**Today:** Remote desktop into the VM: the progress log, the Claude app and its jobs, Revit, and the Procore and Forma sign-ins.

**What could change:** Fixing starts in the webapp. Nobody needs to open the Windows computer for daily work. Run status, logs and job results are in the webapp. Procore and Forma sign-ins are gone. Only rare Revit, Navisworks or Windows problems need someone on the Windows computer, and that is us, not the lead.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Progress log | A file on the VM. | VM | The webapp's run status. | Webapp |
| Claude app and its jobs | On the VM. | VM | Could be removed, with the jobs in the webapp. | Not needed |
| Revit and Navisworks checks | The lead checks that Revit 2025 and 2026 answer and restarts them, or closes a file left open in Navisworks. | VM | On the Windows computer, fixed by us when needed. | Windows computer |
| Procore and Forma sign-ins | Chrome on the VM, and the Claude app's browser for Procore. | VM | Could be removed. Forma through Autodesk's API, and Procore through its API once ARCO's Procore admin connects our app. | Not needed |
| Re-run a job | The lead runs the sweep again from its task page in Claude, after fixing the cause. | Claude + VM | Could be started again from the webapp's chat, only when the lead asks. | Webapp |

## AWS console

*Service, Outside*

Amazon's control panel. Starts or reboots the VM if it is stopped or frozen (region us-east-2).

**Today:** The lead starts or reboots the VM in Amazon's console.

**What could change:** Restarting the Windows computer: by hand, or a webapp button. Suggested: a 'Restart Windows computer' button in the webapp, through the hosting provider's API, for the lead and us only, logged. The webapp itself is restarted by hand from its console, since it can't restart itself when it's down.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Restarting the VM | The lead starts or reboots the VM in Amazon's console, by hand. | Amazon console | Suggested: a 'Restart' button in the webapp for the Windows computer, for the lead and us, logged. The webapp itself is restarted from its hosting console. | To explore |

## The lead's PC (backup)

*Fallback, Not in git*

Last resort: switch on the PC's Daily run task and paused job copies. Never both machines at once, or everything runs twice.

**Today:** An old copy of the bot and paused copies of the jobs on the lead's PC, switched on only if the VM can't run. Never both.

**What could change:** The lead's PC backup: suggested to remove it. The bot and the jobs now live in the webapp, so the PC can't stand in for them, and keeping it current is a burden. Suggested: remove it and rely on the hosting backups and a quick restore. Risk: a long webapp outage means no runs until it's restored.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| The PC's bot and its paused job copies | Kept as the last resort. | Lead's PC | Suggested: remove it, and rely on the hosting's backups and a quick restore. | To explore |

## Re-run a step

*Repo, In git*

Only when the lead asks, outside run times, through a one-off task. Each step skips itself if it already ran today.

- `hcg-bim-assistant`: the bot's repo

**Today:** A one-off task on the VM. Each step skips itself if it already ran today.

**What could change:** Re-running a step stays a chat request, only when the lead asks, outside run times. From the webapp's chat, with the same skip rule.

| Part | Currently | Runs on now | Can be optimized | Would run on |
|---|---|---|---|---|
| Re-run a step | A one-off task on the VM. | VM | From the webapp's chat. | Webapp |
