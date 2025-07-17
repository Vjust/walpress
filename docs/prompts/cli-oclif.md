Absolutely — here’s a working TypeScript CLI setup using yargs with commandDir to support nested commands like:

myapp project push
myapp site create <project-id> <site-name>
myapp page list


⸻

🛠️ 1. Project Structure

myapp/
├── src/
│   ├── cli.ts            ← Entry point
│   └── commands/
│       ├── project/
│       │   ├── push.ts
│       │   └── pull.ts
│       ├── site/
│       │   ├── create.ts
│       │   └── list.ts
│       └── page/
│           └── list.ts
├── tsconfig.json
└── package.json


⸻

📦 2. Install Dependencies

npm install yargs @types/yargs ts-node typescript


⸻

🧠 3. src/cli.ts – CLI Entry Point

#!/usr/bin/env ts-node

import yargs from 'yargs'
import { hideBin } from 'yargs/helpers'

yargs(hideBin(process.argv))
  .commandDir('commands', {
    recurse: true, // ⬅️ allow nested subfolders
    extensions: ['ts'],
  })
  .demandCommand()
  .strict()
  .help()
  .argv


⸻

🔧 4. Sample Command: src/commands/project/push.ts

import { Argv } from 'yargs'

exports.command = 'push'
exports.describe = 'Push project config to the cloud'

exports.builder = (yargs: Argv) => yargs

exports.handler = async () => {
  console.log('Pushing project...')
  // Your SDK logic here
}


⸻

🔧 Another Command: src/commands/site/create.ts

import { Argv } from 'yargs'

exports.command = 'create <projectId> <siteName>'
exports.describe = 'Create a new site under a project'

exports.builder = (yargs: Argv) =>
  yargs
    .positional('projectId', {
      describe: 'ID of the project',
      type: 'string',
    })
    .positional('siteName', {
      describe: 'Name of the site to create',
      type: 'string',
    })

exports.handler = async (argv: any) => {
  console.log(`Creating site '${argv.siteName}' under project ${argv.projectId}`)
  // Call your component system here
}


⸻

✅ 5. Running the CLI

Make sure your CLI entry has the shebang (#!/usr/bin/env ts-node) and is executable:

chmod +x src/cli.ts

Run it:

ts-node src/cli.ts project push
ts-node src/cli.ts site create my-project-id my-site-name


⸻

🧼 Bonus Tips
	•	Add "type": "module" in package.json if using ES Modules.
	•	Add a bin entry to make it a real CLI tool:

"bin": {
  "myapp": "./dist/cli.js"
}



⸻

