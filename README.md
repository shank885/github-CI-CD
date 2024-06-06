# The Docs

You are working for CorpX. The docs team have asked for your support in developing integration and deployment workflows for CorpX's internal knowledge base. The team has been using a manual process to update and publish these docs and they are looking to automate the process.

# CorpX Documentation

Welcome to CorpX documentation! Our documentation serves as a vital resource for understanding our products, services, and processes. It ensures clarity, consistency, and accessibility for both internal teams and external users. We value your contributions to keeping our documentation robust and up-to-date.

## Why Documentation Matters

Clear and comprehensive documentation is essential for several reasons:

- **Accessibility**: It provides easy access to information for all stakeholders, including developers, designers, and end-users.
- **Consistency**: It ensures consistency in communication, reducing confusion and errors.
- **Onboarding**: New team members can quickly familiarize themselves with our products and processes, accelerating their onboarding process.
- **Troubleshooting**: It serves as a reference point for troubleshooting common issues and resolving queries efficiently.

## Why Eleventy (11ty)?

We've chosen eleventy (11ty) as our static site generator for several reasons:

- **Simplicity**: Eleventy offers a simple and intuitive approach to building static sites, making it easy for contributors to focus on content creation.
- **Flexibility**: It provides the flexibility to structure content in various formats, including Markdown, HTML, and JavaScript templates.
- **Performance**: Eleventy generates fast and lightweight static sites, ensuring optimal performance for users.
- **Community Support**: With a growing community and active development, Eleventy is continuously improving and evolving to meet the needs of developers and content creators.

## Requirements

The docs team has asked for the following requirements:

### For the Integration Workflow

- Must be triggered by at least these 2 events:
  - when a new PR is created
  - when a PR is merged into the main branch

> [!TIP]
> Revisit the events that trigger workflow runs in GitHub Actions.

- Must run a linter (eslint) to check for any syntax errors in the markdown files
- Must run all the scripts in the `script/` directory in parallel and fail if any of the scripts fail
- Must build all the necessary assets to make sure the build succeeds

> [!TIP]
> The `package.json` file has a lot of information about the build scripts.

- Must cache the `node_modules` directory to speed up the build process
  - Must bust the cached `node_modules` directory if the `package.json` file has changed

### For the Deployment Workflow

- Must run when a commit has been pushed to the `main` branch
- Must never run if the integration workflow has failed
- Must never run if there are no changes introduced

> [!TIP]
> Think about how to use workflow contexts, outputs, and expressions to achieve this.

- Must deploy to the GitHub Pages environment only

> [!TIP]
> If GitHub Pages is not enabled, you can enable it in the repository settings.

- Must not have more than 1 run at a time (concurrency limit of 1)
  - If a deployment is already running and a new commit is pushed, the deployment should be cancelled and a new deployment should start
- Must upload a `tar.gz` file of the generated assets as an artifact to the workflow run

> [!TIP]
> Don't reinvent the wheel. Use marketplace actions to help you achieve your objectives.

- Must use the `GH_TOKEN` secret to authenticate with GitHub
- Must run a post-deployment test by calling the GitHub Pages URL and checking for a 200 status code

> [!TIP]
> Think how you can leverage the `shell` override to help you with this task.

- Must support a `force deployment` option (think how you can use `workflow_dispatch` events)
  - This option should be a boolean input and if set to true, the deployment should run even if there are no changes were pushed to `main`

## General guidelines

- The workflows should be as efficient as possible. You must reuse as much as you can from the solutions available to you.
- All workflow runs and should have a strict timeout of `20 minutes` and should fail if the timeout is reached. Timeout includes the integration and deployment runs.
- If you use any third-party actions, make sure to pin the action to a full length commit SHA. Read more about [pinning actions](https://docs.github.com/en/actions/security-guides/security-hardening-for-github-actions#using-third-party-actions).

## How to Contribute

Contributing to our documentation is straightforward. Follow these steps to make changes:

1. **Clone the Repository**: Clone the documentation repository to your local machine.

2. **Install Dependencies**: Navigate to the root of the project and install dependencies using npm:

    ```shell
    npm install
    ```

3. **Run Development Mode**: Start the development server and watch for changes:

    ```shell
    npm start
    ```

4. **Run Production Mode**: Before deploying changes live, compress the Sass files:

    ```shell
    npm run prod
    ```

For additional npm scripts and details, refer to the [package.json](https://github.com/conedevelopment/sprucecss-eleventy-documentation-template/blob/main/package.json).

## Content Management

Adding content to the documentation template is straightforward with Eleventy:

### Basic Structure

- The base folder for documentation pages is `posts`.
- Follow the folder structure, including the category. Each category folder must have a list page with the same name.
- Create another `posts` folder under each category folder for your posts.
- Create a `posts.json` file to set layout and permalink values.

### Eleventy Navigation

Utilize the [Eleventy Navigation plugin](https://www.11ty.dev/docs/plugins/navigation/) to set up hierarchy for automatic sidebar navigation, navigation order, and breadcrumb generation.

### Other Pages

For simple pages, create a file directly under the `src` folder and configure it with available front matter.

## Project Structure

```plaintext
spruecss-eleventy-documentation-template/
├─ node_modules/
├─ dist/
├─ src/
│  ├─ _data/
│  ├─ _includes/
│  ├─ css/
│  ├─ filters/
│  ├─ font/
│  ├─ img/
│  ├─ js/
│  ├─ posts/
│  ├─ scss/
│  ├─ shortcodes/
│  ├─ transforms/
│  ├─ changelog.md
│  ├─ faq.md
│  ├─ index.md
├─ .eleventy.js
├─ package.json
├─ README.md
├─ ...
```

- **_data**: Some global data, like the name of your site and helpers like the active navigation element or current year.
- **__includes**: All of the layout and partial templates.
- **css**: The compiled CSS.
- **filters**: The additional filters that you can use.
- **font**: The custom fonts.
- **img**: The static image files.
- **posts**: The markdown contents.
- **scss**: The Sass files.
- **shortcodes**: The available shortcodes.
- **transforms**: The transformations.


<test>