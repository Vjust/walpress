Here’s a full TypeScript CLI example using commander.js + a custom router to support nested commands like:

myapp project push
myapp site create <project-id> <site-name>
myapp page list

Unlike yargs.commandDir, commander.js doesn’t support automatic recursive loading — so you define a command router manually.

⸻

🛠️ 1. Project Structure

myapp/
├── src/
│   ├── cli.ts              ← Entry point
│   └── commands/
│       ├── project/
│       │   ├── push.ts
│       │   └── pull.ts
│       ├── site/
│       │   └── create.ts
│       └── page/
│           └── list.ts
├── package.json
├── tsconfig.json


⸻

📦 2. Install Dependencies

npm install commander
npm install -D typescript ts-node @types/node


⸻

🧠 3. src/cli.ts — Entry Point with Router

#!/usr/bin/env ts-node

import { Command } from 'commander'
import registerProjectCommands from './commands/project'
import registerSiteCommands from './commands/site'
import registerPageCommands from './commands/page'

const program = new Command()

program.name('myapp').description('My CLI App').version('1.0.0')

// Register top-level command groups
registerProjectCommands(program.command('project'))
registerSiteCommands(program.command('site'))
registerPageCommands(program.command('page'))

program.parse(process.argv)


⸻

🧭 4. Router: src/commands/project/index.ts

import { Command } from 'commander'
import push from './push'
import pull from './pull'

export default function registerProjectCommands(project: Command) {
  project
    .description('Manage projects')
    .addCommand(push)
    .addCommand(pull)
}


⸻

🔧 5. Actual Command: src/commands/project/push.ts

import { Command } from 'commander'

const cmd = new Command('push')
  .description('Push project config to the cloud')
  .action(() => {
    console.log('Pushing project...')
    // call SDK or component system
  })

export default cmd


⸻

🔧 6. Another Command: src/commands/site/create.ts

import { Command } from 'commander'

const cmd = new Command('create')
  .description('Create a new site')
  .argument('<projectId>', 'ID of the project')
  .argument('<siteName>', 'Name of the new site')
  .action((projectId, siteName) => {
    console.log(`Creating site "${siteName}" under project "${projectId}"`)
    // Call component logic
  })

export default cmd


⸻

🧭 7. Router: src/commands/site/index.ts

import { Command } from 'commander'
import create from './create'

export default function registerSiteCommands(site: Command) {
  site.description('Manage sites').addCommand(create)
}


⸻

🧭 8. Router: src/commands/page/index.ts

import { Command } from 'commander'
import list from './list'

export default function registerPageCommands(page: Command) {
  page.description('Manage pages').addCommand(list)
}


⸻

✅ Run It

ts-node src/cli.ts project push
ts-node src/cli.ts site create proj123 my-site
ts-node src/cli.ts page list


⸻

✨ Pros of This Setup

Feature	Commander.js + Router
✅ Lightweight	Yes
✅ Fully typed TS	Yes
✅ Easy control	You decide routing
🚫 No auto-loading	Manual wiring required
✅ Great UX & help	Yes


⸻

Would you like this scaffold as a downloadable zip or GitHub starter repo?