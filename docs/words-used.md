# Words used

Plain meanings of the terms on the [plan page](https://faiq-kl.github.io/campbell-cts-documentation/vm-to-webapp-plan.html) and in these docs.

- **NOVA102, VKU:** The two construction projects. NOVA102, a data-centre building, is the main one.
- **HCG:** The team that keeps the electrical 3D model and the project paperwork up to date.
- **Lead, deputy:** The lead makes the project decisions. The deputy is a teammate who stands in when the lead is away.
- **ARCO, Triad, BES, CMMC:** ARCO is the general contractor and Triad the electrical contractor. BES and CMMC are design firms.
- **Revit, Navisworks:** Revit holds the electrical 3D model. Navisworks combines every trade's model to find clashes and play the schedule.
- **NWF, NWD:** Navisworks files. The NWF lists the models, clash checks and 4D timeline. The NWD is a full snapshot.
- **Clash:** Two things in the combined model hitting each other, like a pipe through a beam.
- **conduit:** A pipe that carries cables. Also the company's software that lets programs and AI work inside Revit and Navisworks.
- **ACC, Forma:** Autodesk's online project folder and issue list. A Forma issue is a tracked problem, like a ticket. Desktop Connector syncs a copy to the VM.
- **Procore:** ARCO's project website for questions, approvals and schedules.
- **RFI, submittal:** An RFI is a formal question to the designers. A submittal is product or drawing detail sent for approval.
- **Work package, Coordination Schedule:** A work package is one chunk of construction work. The Coordination Schedule is the master spreadsheet of them.
- **P6, Lookahead:** P6 is ARCO's master schedule. The Lookahead is its next 6 to 12 weeks.
- **4D, 5D:** 4D links the model to the schedule to show the build over time. 5D adds quantities.
- **Laser scan:** A 3D scan of what is built, compared with the model to measure progress.
- **VM:** The rented Windows machine in Amazon's cloud where production runs today. In the suggested setup a smaller VM keeps only Revit, Navisworks, conduit and the file helper, and nobody needs to open it. Times are US Central (CT).
- **Bot:** Our Python program (hcg-bim-assistant) that updates project files and pages twice a day. It doesn't use AI.
- **Repo, main branch:** A folder of code on GitHub. Saving to its main branch makes a change live.
- **Scheduled task, skill, Claude Project:** An AI job on a timer, a set of instructions the AI loads, and a shared space of notes the AI reads and writes.
- **Webapp:** Our app on our own server. It could run the bot, Claude and the notes.
- **Private link:** A connection between the webapp and the VM, closed to the internet.
- **File helper:** A small program that would move Navisworks' files to and from ACC through Autodesk's API, in place of Desktop Connector.
- **Agent SDK:** Anthropic's engine behind Claude Code. It could run Claude inside the webapp.
- **API, tool server:** An API lets our app talk to another system directly. A tool server (MCP) is a standard way to give Claude tools.
