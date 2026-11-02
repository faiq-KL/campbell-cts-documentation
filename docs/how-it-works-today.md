# How it works today

Each flow step by step, as production runs now. The [plan page](https://faiq-kl.github.io/campbell-cts-documentation/vm-to-webapp-plan.html) shows each flow's diagram.

## Flow 1: Update project data

Twice each weekday, at 09:00 and 16:00 CT, the bot reads the 3D models and project files, then updates the schedule spreadsheets and the web pages. Anything that fails a safety check is held for the lead.

1. A Windows timer on the VM starts the bot (**hcg-bim-assistant**).
2. The bot opens each 3D model through **conduit**, read only, and finds what changed.
3. It also reads the project's spreadsheets, drawings, RFI folders, clash report, laser scans and Forma issues.
4. It writes the results to the project files in ACC (after a backup) and to a run log.
5. It updates 7 of the 10 web pages, and Cloudflare puts them online. The other 3 are updated by hand (the Action Hub also has a newer bot step).
6. Results that fail its safety checks are held for the lead instead of written. It emails the lead when a run is blocked, slow, failed or finished, and shows pop-ups on the VM.

## Flow 2: Run scheduled AI jobs

AI jobs run on timers all day. They file RFIs and submittals, keep the project notes, prepare meetings and email summaries, while three watchers check that everything ran.

1. **Timers** start each job. Seven run in the Claude app on the VM, and the heartbeat runs in Claude's cloud.
2. **The AI** (Claude, signed in as the lead) does the job, using the team's skills and reading what it needs.
3. It **writes the results**: documents and schedule updates in ACC, the tracker page's schedule section, and notes.
4. It **emails** the lead and the deputy, and no one else.
5. The three watchers only check for problems. Their alarms start Flow 4.

## Flow 3: Review and approve

Anything unusual waits for a person. The lead decides in chat, Triad signs off schedule cards, and the AI then carries out exactly what was approved.

1. The bot and the AI hold anything unusual instead of writing it.
2. The lead decides in a Claude chat or on the Lead Queue page.
3. Triad's signers approve or reject schedule cards on the sign-off board.
4. In chat, the AI carries out exactly what was approved.
5. The lead's rulings go into the rule book, so future jobs follow them.
6. Teammates start their day in chat, and claim tasks on the Task Board before changing shared files.

## Flow 4: Respond to incidents

When a run or a job fails, a watcher alerts the lead. The lead finds the cause on the VM, then restarts or re-runs what failed, with a backup PC as the last resort.

1. A watcher or the bot alerts the lead by email or phone push.
2. The lead logs into the VM and checks the run log, the Claude app, Revit, and the Procore and Forma sign-ins.
3. If the VM is down, they restart it in Amazon's AWS console.
4. If a step failed, it is re-run only when the lead asks, outside run times.
5. Last resort: run the jobs on the lead's PC instead, never on both.
6. Two things fix themselves: the keep-alive reopens the Claude app and Chrome, and the bot restarts a frozen Revit.

## Flow 5: Sell and support Conduit

The company also sells conduit, its Revit and Navisworks software. Releases are built and uploaded for licensed download, and an AI chat on the website answers customers.

1. The lead's software, **conduit**, is also sold to other companies.
2. Installers are built and signed on the lead's PC, or built by GitHub.
3. A script uploads each release to Keygen, which holds licence keys and files.
4. Customers enter their licence key on campbellcts.com and download.
5. The site's AI chat answers questions and emails the lead when needed. A daily summary reaches the lead at 07:00 CT.
6. Conduit's AI skills reach everyone in the company through the marketplace repo.

## Flow A: Release code changes

Code changes come from coding sessions and land on GitHub. Saving to a repo's main branch puts the pages and the website live, while the VM runs its own copy of the code.

1. The lead and AI coding sessions change the code on GitHub.
2. Saving a page repo's main branch makes Cloudflare put it live within minutes. Most page repos have no test branch.
3. The website goes live the same way, through Netlify.
4. The bot saves fresh page data to main twice a day (as **hcg-bim-runner**).
5. The VM runs its own copy of the code. Some bot fixes were made on the VM itself and saved to GitHub from there. How other changes reach the VM is unknown. One bot file and changes to the Navisworks bridge exist only on the VM.
6. AI skills and job instructions are edited by hand in the Claude app, not through GitHub.
7. A dev copy of the VM was made. Only its file names are known.

## Flow B: Onboard a new project

A new project starts with an AI interview of its lead. The answers become two settings files, then the bot adds the project and its pages are copied from an existing project.

1. The project lead fills in an intake form and is interviewed by the onboarding AI chat.
2. The chat checks each answer twice on the live model, then proposes two settings files, saved in ACC once the lead approves.
3. The project is added to the bot, and its web pages are copied from NOVA102's.
4. The bot first runs in dry-run mode and writes nothing. After the lead reviews it, it switches to write mode.
