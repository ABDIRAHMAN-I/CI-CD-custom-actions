## Creating Custom GitHub Actions 🚀
GitHub Custom Actions allow you to create reusable automation components that can be shared between workflows or even published for others to use. They are defined in a directory and consist of metadata files (`action.yml` or `action.yaml`), scripts, and any supporting files.

### Why Create Custom Actions? 🤔
- **Reusability**: Write code once and use it across multiple workflows or repositories.
- **Shareability**: Publish actions to the GitHub Marketplace for others to use.
- **Customization**: Tailor actions specifically to your unique requirements.

### Types of GitHub Actions
1. **JavaScript Actions**: Written in JavaScript, can run directly in the runner.
2. **Docker Actions**: Packaged in Docker containers, useful for custom environments.
3. **Composite Actions**: Combine multiple steps defined in YAML, ideal for modular and reusable tasks.

### Creating a Custom Action: Step-by-Step 🛠️

1. **Create a New Directory for the Action**
   - In your repository, create a directory named `.github/actions/my-action`.

2. **Create the Metadata File**
   - The metadata file (`action.yml`) defines the inputs, outputs, and main entry point of your action.

   **Example `action.yml`:**
   ```yaml
   name: "My Custom Action"
   description: "A custom GitHub Action to greet the user"
   inputs:
     name:
       description: "The name of the person to greet"
       required: true
       default: "World"
   outputs:
     greeting:
       description: "The greeting message"
   runs:
     using: "node12"
     main: "index.js"
   ```

3. **Write the Code for Your Action**
   - Create a file named `index.js` in the action directory. This is the main code that will run when the action is triggered.

   **Example `index.js`:**
   ```javascript
   const core = require('@actions/core');

   async function run() {
     try {
       const name = core.getInput('name');
       const greeting = `Hello, ${name}!`;
       console.log(greeting);
       core.setOutput('greeting', greeting);
     } catch (error) {
       core.setFailed(error.message);
     }
   }

   run();
   ```

   - This example takes an input (`name`) and prints a greeting message.

4. **Add the `node_modules` Directory**
   - To ensure dependencies are available, add the `node_modules` directory by running:
     ```sh
     npm install @actions/core
     ```

5. **Commit and Push Your Custom Action**
   - Add, commit, and push your changes to the repository.

6. **Using the Custom Action in a Workflow**
   - Reference the custom action from your workflow YAML file:

   **Example Workflow Using the Custom Action:**
   ```yaml
   name: Use Custom Action

   on: [push]

   jobs:
     greet:
       runs-on: ubuntu-latest
       steps:
         - name: Checkout code
           uses: actions/checkout@v2

         - name: Run custom action
           uses: ./.github/actions/my-action
           with:
             name: "GitHub User"
   ```

### Publishing Your Custom Action
- To share your action with others, push it to a public repository and add a version tag (e.g., `v1.0.0`). You can then publish it to the GitHub Marketplace.

---


