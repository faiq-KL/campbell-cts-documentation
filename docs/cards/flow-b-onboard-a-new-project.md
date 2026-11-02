# Flow B: Onboard a new project

Every card in this flow, in full. The [plan page](https://faiq-kl.github.io/campbell-cts-documentation/vm-to-webapp-plan.html) shows the diagram.

## Project lead

*People*

Fills in the intake form, answers the interview and approves every setting. Nothing is copied from another project.

Not explored yet.

## Intake form and project docs

*Service, Outside*

In the project's ACC folder.

- `<CODE>_Intake_Form.docx`: the intake form (guide UG-02A)
- `START_HERE, scope and schedule references, grid map`: project facts in plain English
- `User guides UG-00 to UG-12`: how-to guides for the team

Not explored yet.

## Onboarding chat

*Skill + repo tools, Git + Claude account*

An AI chat that interviews the lead and checks each answer twice on the live model.

- `project-onboarding`: the skill
- `python -m onboarding`: checks the Manifest, lists models, reads drawing naming rules
- `api/ + frontend/`: a local setup screen, not seen in use

Not explored yet.

## Project settings in ACC

*Service, Outside*

The two files every run and chat reads. Changed only with the lead's OK.

- `BIM_Project_Manifest.json`: where things are: folders, files, models, page repos
- `BIM_Project_Profile.yaml`: how to read them: areas, naming rules, spreadsheet columns (checked by schemas/project-profile.schema.json)

Not explored yet.

## Added to the bot

*On the VM, On the VM*

The project joins the bot's daily run.

- `python -m orchestrator add-project`: the command
- `runner.json`: gets the project's times, repos and steps
- `.hcg/owner.json`: first publish marks the VM as owner

Not explored yet.

## New pages, set up by hand

*Repos, In git*

Copied from NOVA102's pages (as VKU's were), each with its own Cloudflare site and sign-in list.

- `<Project>-Tracker, <Project>-Impact-Dashboard`: the project's pages
- `HCG-BIM-Portal/config/sites.json + access.json`: adds the pages and users to the portal
- `HCG-Portfolio-Dashboard/data/<code>.json`: adds the project to the leadership page

Not explored yet.

## Dry run, then write

*Bot, In git*

The bot first runs without writing. After the lead reviews it, it may write.

- `write_back.mode: dry_run → write`: the switch in the Profile

Not explored yet.
