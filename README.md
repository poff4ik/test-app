# Quiz CLI

![Node.js](https://img.shields.io/badge/Node.js-18%2B-brightgreen.svg)
![Language](https://img.shields.io/badge/JavaScript-ES%20Modules-blue.svg)
![License](https://img.shields.io/badge/License-MIT-yellow.svg)

## Project Overview

Quiz CLI is a small, interactive command-line quiz game built with modern JavaScript (ES modules). It provides a friendly, terminal-first experience for learning programming concepts with multiple categories and question sets loaded from JSON. The application demonstrates practical Node.js patterns such as async/await, file I/O, readline-based user input, and simple object-oriented design.

This project is purposely minimal and dependency-free (no third-party packages). It is a great reference implementation or starter project for learning how to build interactive CLIs with Node.js — showing examples of modular code organization, ANSI terminal colors, prompt helpers, and a clean separation of concerns between input handling, presentation, and game logic.

Intended uses:
- A learning tool for individuals or classrooms to quiz basic programming knowledge.
- A sample project showcasing ES module usage, Promises/async, and terminal UI techniques.
- A starting point to extend with more features (timers, persistence, new question sets, scoring leaderboards).

Key design decisions:
- Questions are loaded from a JSON file (data/questions.json) which makes it easy to add or edit quizzes.
- The core logic lives in src/quiz.js and is UI-agnostic (receives a readline interface for input).
- No external dependencies: only Node built-in modules are used to reduce friction and maintenance.

## Technology Stack

| Technology | Purpose |
|------------|---------|
| Node.js (>= 18.0.0) | Runtime (ES Modules support; built-in fs/promises, readline, URL/path utilities) |
| JavaScript (ES Modules) | Language and module system (import/export) |
| Built-in Node APIs | fs/promises (file IO), readline (input), path/url (path helpers) |
| ANSI escape codes | Terminal colorization (src/colors.js) |
| JSON | Question data storage (data/questions.json) |

## Key Features

- Interactive terminal quiz with multiple categories
- Configurable question counts (All, 3, 5) based on available questions
- Colorized terminal output and simple progress bar
- Randomized question order (Fisher–Yates shuffle)
- Detailed results and review of incorrect answers with explanations
- No external dependencies — runs with Node.js only
- Clear, modular code suitable for learning and extension

---

## Setup Instructions

### Prerequisites

| Requirement | Minimum Version | Install |
|-------------|-----------------|---------|
| Node.js | 18.0.0 | https://nodejs.org/ |
| Git (optional) | any | https://git-scm.com/ |

> ⚠️ The project uses ES modules (`"type": "module"`) and Node.js built-ins that require Node 18+.

### Installation (Local)

1. Clone the repository (or copy files into a folder)
   ```bash
   git clone <repository-url>
   cd quiz-cli
   ```
   If you don't have a remote repository, just place the provided files into a directory and cd into it.

2. Install dependencies
   - This project has no external dependencies. Ensure Node.js >= 18 is installed.

3. Inspect / edit questions (optional)
   ```bash
   # Open the question file to customize categories/questions
   vim data/questions.json
   ```

### Run the app

Start the Quiz CLI:
```bash
npm start
```
This runs:
```bash
node index.js
```

Alternative during development (no extra tooling required):
```bash
node index.js
```

### Verification

After running `npm start`:
- You should see a colored banner and a prompt to choose a category.
- Select a category by entering the number shown.
- Follow prompts to answer questions and view results at the end.

If you see an error like "Error: ENOENT: no such file or directory" check that data/questions.json exists and is valid JSON.

---

## Usage Examples

This CLI is interactive. Example session (user input shown after prompts):

```text
$ npm start

  ╔═══════════════════════════════════════════╗
  ║                                           ║
  ║   📚 QUIZ CLI                           ║
  ║   Test your programming knowledge!        ║
  ║                                           ║
  ╚═══════════════════════════════════════════╝

Choose a category:

  1. JavaScript Basics
  2. Node.js Fundamentals
  3. General Programming

Your choice (enter number): 1

How many questions?

  1. All questions
  2. 3 questions
  3. 5 questions

Your choice (enter number): 2

Starting quiz...
Select your answer by entering the number.

[Press Enter to continue]

Question 1 of 3
  1. var
  2. let
  3. const
  4. define

Your choice (enter number): 3

✓ Correct!
💡 The 'const' keyword declares a block-scoped constant that cannot be reassigned.

Press Enter to continue...
...
══════════════════════════════════════════════
  📊 QUIZ RESULTS
══════════════════════════════════════════════

  Category: JavaScript Basics
  Score: 3/3 (100%)

  🏆 Perfect score! Amazing!
══════════════════════════════════════════════
Thanks for playing! Keep learning! 🚀
```

How to add a new category:
- Edit data/questions.json and add a new key under "categories" with the same structure (name, questions array with question/options/answer/explanation).

Programmatic usage (embedding Quiz class):
- The Quiz class is exported from src/quiz.js and can be reused in scripts that provide a readline.Interface:
```javascript
import { createInterface } from './src/input.js';
import { Quiz } from './src/quiz.js';
import fs from 'fs/promises';

async function run() {
  const rl = createInterface();
  const data = JSON.parse(await fs.readFile('./data/questions.json', 'utf8'));
  const questions = data.categories.javascript.questions.slice(0, 3);
  const quiz = new Quiz(questions, 'JavaScript Basics');
  while (!quiz.isComplete) {
    await quiz.askQuestion(rl);
    await rl.question('\nPress Enter to continue...', () => {});
  }
  quiz.showResults();
  rl.close();
}
run();
```

---

## File Structure

```
.
├── .DS_Store (binary macOS metadata file; can be ignored)
├── index.js
├── package.json
├── data
│   └── questions.json
└── src
    ├── colors.js
    ├── input.js
    └── quiz.js
```

Component purposes:

| Path | Purpose |
|------|---------|
| index.js | Application entry point: loads questions, manages main loop and user flow |
| package.json | Project metadata, scripts, and engine requirement (Node >= 18) |
| data/questions.json | Quiz data: categories, questions, options, correct answer index, explanations |
| src/colors.js | Simple ANSI-based color helpers and convenience functions |
| src/input.js | Readline-based input helpers: createInterface, prompt, select, confirm, pressEnter |
| src/quiz.js | Quiz logic: shuffling, question flow, scoring, progress bar, results display |

---

## Additional Details

Environment variables
- This project does not require any environment variables. It uses local file I/O to load questions from data/questions.json.

Dependencies
- No external (npm) dependencies.
- Uses Node built-in modules: fs/promises, path, url, readline.

Available scripts (package.json)
```json
"scripts": {
  "start": "node index.js",
  "test": "node --test"
}
```
- npm start — run the CLI
- npm test — runs Node's built-in test runner (no tests included by default)

Node engine requirement
- package.json specifies:
```json
"engines": { "node": ">=18.0.0" }
```
Run `node -v` to confirm your version.

Security & Validation
- Minimal surface area: no external network access and no third-party packages.
- Error handling: main() wraps the run loop in try/catch and logs errors with a stack trace before exiting with code 1.
- Input validation: select() and confirm() validate numeric selection and yes/no answers before proceeding.

Error handling patterns
- JSON parsing errors when loading data/questions.json will throw and terminate the program with an explanatory message.
- Missing or malformed question fields (e.g., missing options or answer indices) may result in runtime errors; validate the JSON structure when editing.

Testing
- No unit tests are included. To add tests, create test files and run with `npm test` (Node's test runner) or incorporate a test framework (Jest, Mocha).
- Example (adding a simple test file):
  1. Create `test/quiz.test.js`.
  2. Use Node's built-in test module or a chosen framework.
  3. Run `npm test`.

Packaging as a global CLI (optional)
- To install locally and use as a global command during development:
```bash
npm link
# then run
quiz-cli   # if you add a bin entry to package.json and set executable
```
> Note: package.json currently has no "bin" field. Add one to publish or to create a global command.

Troubleshooting

- Error: "SyntaxError: Unexpected token import" — Ensure Node >= 18 and that package.json has `"type": "module"`.
- Error: "ENOENT: no such file or directory, open 'data/questions.json'" — Make sure you ran the CLI from the project root and data/questions.json exists.
- JSON parse errors — Edit data/questions.json and ensure valid JSON (tools: `jq`, online JSON validators).

Contributing

1. Fork the repository.
2. Create a feature branch: git checkout -b feature/your-feature.
3. Make your changes and write tests where applicable.
4. Commit: git commit -m "Add feature".
5. Push and open a pull request for review.

Guidelines:
- Keep changes small and focused.
- Update data/questions.json with new questions following the existing schema.
- Maintain consistent code style and add comments for non-obvious logic.

Schema for questions (data/questions.json)
Each category:
```json
"categoryKey": {
  "name": "Category Name",
  "questions": [
    {
      "question": "Question text",
      "options": ["opt1", "opt2", "opt3"],
      "answer": 1,               // zero-based index of correct option
      "explanation": "Optional explanation text"
    }
  ]
}
```

License

This project is licensed under the MIT License. See the LICENSE file for details.

---

If you'd like, I can:
- Add a "bin" entry and a preconfigured package.json snippet to install CLI globally.
- Generate a Dockerfile for containerized runs.
- Add unit tests for the Quiz class and input helpers.
