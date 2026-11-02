# Flow 5: Sell and support Conduit

Every card in this flow, in full. The [plan page](https://faiq-kl.github.io/campbell-cts-documentation/vm-to-webapp-plan.html) shows the diagram.

## Make the conduit product

*Repo, In git*

The same Revit and Navisworks tools the bot uses, sold to other companies.

- `conduit`: the repo

Not explored yet.

## Deliver the skills

*Repo, In git*

A script (conduit/tools/publish-marketplace.ps1) copies the skills here by pull request. Once merged, Cowork shares them.

- `conduit-marketplace`: the repo

Not explored yet.

## Build a release

*Repo tools, In git*

Builds the installers on the lead's PC and signs them with a key only that PC has, so Windows trusts them.

- `conduit/build/Build-Release.ps1`: builds a release
- `build/Build-Addins.ps1`: builds and signs the Revit add-ins
- `build/Build-TesterKit.ps1`: one kit with every installer, for testers
- `Claude and ChatGPT packages`: each product is built for both
- `NuGet, DigiCert`: outside services the build uses

Not explored yet.

## Build a release in the cloud

*Scheduler, In git*

The other way to build: GitHub builds on a clean machine.

- `GitHub Actions: Build release packages` (On a version tag (v…), or by hand): No version tags exist in our copy yet.

Not explored yet.

## Share skills across the company

*Skills, Claude account*

Cowork shares the skills with everyone in the company account.

- `Conduit for AEC`: the skill bundle

Not explored yet.

## Upload a release

*Repo tool, In git*

The lead runs it by hand, with a Keygen token, to upload each release.

- `conduit/tools/publish-release.ps1`

Not explored yet.

## Manage licences and downloads

*Service, Outside*

Online licensing service. Holds licence keys and release files. Licence events email the lead through the website.

- `Keygen`

Not explored yet.

## Send the support digest

*Scheduler, In git*

A daily summary of the website's support chats.

- `support-digest (Netlify function)` (Daily 12:00 UTC (06:00 or 07:00 CT, depending on daylight saving)): Emails the summary to the lead.

Not explored yet.

## Run the company website

*Repo, In git*

Hosted on Netlify: product pages, licensed downloads and an AI support chat.

- `campbellcts-site`: the repo
- `netlify/functions/kb/*.md`: what the support chat knows
- `tools/build_blog.py`: builds the blog, run by hand

Not explored yet.

## Download and use conduit

*People*

Customers download with a licence key and can use the support chat. Testers get a tester kit and a free trial key.

Not explored yet.

## Power the support chat

*Service, Outside*

Used by the support chat.

- `Anthropic API`: the AI that answers
- `Supabase`: stores the chat log
- `Resend`: sends the emails

Not explored yet.

## Handle escalations and alerts

*People*

Gets support escalations, licence alerts and the daily digest.

Not explored yet.
