# Flow 4: Catch and fix failed runs

Every card in this flow, in full. The [plan page](https://faiq-kl.github.io/campbell-cts-documentation/vm-to-webapp-plan.html) shows the diagram.

## Raise the alarm

The AI watchers and the bot's emails warn the client when a run or job fails.

*Schedulers, Claude account + VM*

They only report. None of them fixes anything.

- `NOVA102 BIM Assistant run watchdog (VM)` (11:40 and 17:40): The bot's run is missing, failed or incomplete.
- `NOVA102 RFI sweep watchdog (VM)` (Weekdays 09:05): The sweep failed, sent no email, or Procore is signed out.
- `VKU BIM Assistant shadow + run summary email (VM)` (:25 past each hour after a run): Problem email for VKU.
- `BIM runner VM heartbeat (cloud)` (12:35 and 19:35): Phone push if the VM's jobs stopped.
- `The bot's own alerts` (during each run): Email and pop-up.

**Today:** The run watchdog, the sweep watchdog, the VKU check, the heartbeat (push), and the bot's emails and pop-ups. They only report.

**What could change:** The same alarms, now from the webapp, plus one independent alarm outside it. The watchers and the bot's alerts come from the webapp. The heartbeat stays outside the webapp, suggested as a small task in the client's Claude account that checks the webapp's status page.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| AI watchers | In Claude, using the VM. | Yes | In the webapp. | Webapp |
| The bot's alert emails | From the VM. | Yes | From the webapp. | Webapp |
| Heartbeat | In Claude's cloud. | Yes | Suggested: a small task in the client's Claude account that checks the webapp's status page, with the same push and email. |  |
| The bot's pop-ups | On the VM's screen, during each run. | Yes | Suggested: a webapp notification on phone or browser. | Webapp |
| What-to-do lines in the alarms | Each watcher's email says what to do, such as remote desktop into the VM, read the progress log, or start the VM in Amazon's console. | Yes | Could point to the webapp instead of the VM. The client checks the new wording. | Webapp |

## Decide what to do

The client, or the stand-in, reads the alarm and decides the next step.

*People*

Reads the alert and decides what to do.

**Today:** Reads the alert and decides what to do.

**What could change:** The client, or the stand-in, reads the alert and decides. Unchanged.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Who decides | The client or the stand-in. | No | No change needed. |  |

## Fix it on the VM

The client signs in to the VM to find the cause and fix it.

*On the VM, On the VM*

Remote desktop into the VM, then check:

- `logs\progress_<date>.log`: how far the run got
- `Is the Claude app open?`: switch back on any switched-off jobs
- `Do Revit 2025 / 2026 respond?`: if not, restart them
- `Is Procore signed in?`: in the VM's Chrome and the Claude app's browser
- `Is the Forma download signed in?`: sign in with: python -m site_feeds.forma_export --signin

**Today:** Remote desktop into the VM: the progress log, the Claude app and its jobs, Revit, and the Procore and Forma sign-ins.

**What could change:** Fixing starts in the webapp. Nobody needs to open the VM for daily work. Run status, logs and job results are in the webapp. Procore and Forma sign-ins are gone. Only rare Revit, Navisworks or Windows problems need someone on the VM, and that is us, not the client.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Progress log | A file on the VM. | Yes | The webapp's run status. | Webapp |
| Claude app and its jobs | On the VM. | Yes | Could be removed, with the jobs in the webapp. | Not needed |
| Revit and Navisworks checks | The client checks that Revit 2025 and 2026 answer and restarts them, or closes a file left open in Navisworks. | Yes | On the VM, fixed by us when needed. | VM |
| Procore and Forma sign-ins | Chrome on the VM, and the Claude app's browser for Procore. | Yes | Could be removed. Forma through Autodesk's API, and Procore through its API once ARCO's Procore admin connects our app. | Not needed |
| Re-run a job | The client runs the sweep again from its task page in Claude, after fixing the cause. | Yes | Could be started again from the webapp's chat, only when the client asks. | Webapp |

## Restart the VM

If the VM is down, the client restarts it in Amazon's console.

*Service, Outside*

Amazon's control panel. Starts or reboots the VM if it is stopped or frozen (region us-east-2).

**Today:** The client starts or reboots the VM in Amazon's console.

**What could change:** Restarting the VM: by hand, or a webapp button. Suggested: a 'Restart VM' button in the webapp, through the hosting provider's API, for the client and us only, logged. The webapp itself is restarted by hand from its console, since it can't restart itself when it's down.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Restarting the VM | The client starts or reboots the VM in Amazon's console, by hand. | Yes | Suggested: a 'Restart' button in the webapp for the VM, for the client and us, logged. The webapp itself is restarted from its hosting console. | Webapp |

## Fall back to the client's PC

A last-resort copy of the bot and jobs on the client's own PC.

*Fallback, Not in git*

Last resort: switch on the PC's Daily run task and paused job copies. Never both machines at once, or everything runs twice.

**Today:** An old copy of the bot and paused copies of the jobs on the client's PC, switched on only if the VM can't run. Never both.

**What could change:** The client's PC backup: suggested to remove it. The bot and the jobs now live in the webapp, so the PC can't stand in for them, and keeping it current is a burden. Suggested: remove it and rely on the hosting backups and a quick restore. Risk: a long webapp outage means no runs until it's restored.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| The PC's bot and its paused job copies | Kept as the last resort. | Yes | Suggested: remove it, and rely on the hosting's backups and a quick restore. | Not needed |

## Re-run a step

Runs one step of the bot again once the problem is fixed.

*Repo, In git*

Only when the client asks, outside run times, through a one-off task. Each step skips itself if it already ran today.

- `hcg-bim-assistant`: the bot's repo

**Today:** A one-off task on the VM. Each step skips itself if it already ran today.

**What could change:** Re-running a step stays a chat request, only when the client asks, outside run times. From the webapp's chat, with the same skip rule.

| Part | Currently | Can be optimized | Suggestion | Tag |
|---|---|---|---|---|
| Re-run a step | A one-off task on the VM. | Yes | From the webapp's chat. | Webapp |
