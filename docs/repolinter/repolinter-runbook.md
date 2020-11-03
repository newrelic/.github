# Repolinter Runbook

Repolinter is a tool designed to automatically find and fix open source policy issues in a repository. At New Relic, we have deployed Repolinter via Repolinter action to all of our public repositories to allow greater consistency in policy enforcement.

<!-- toc -->
- [Quick Reference](#quick-reference)
  - [Add Repolinter to a single repository](#add-repolinter-to-a-single-repository)
    - [Alternate method](#alternate-method)
  - [Add Repolinter to many repositories](#add-repolinter-to-many-repositories)
  - [Change the policy for a repository](#change-the-policy-for-a-repository)
  - [Creating a Policy](#creating-a-policy)
- [Architecture Overview](#architecture-overview)
<!-- endtoc -->

## Quick Reference

### Add Repolinter to a single repository

Repolinter Action can be added to a repository by opening a pull request against the target repository with the action workflow file. An [alternate method](#alternate-method) is also available for repositories that find PRs or GitHub actions difficult to work with.

1. Fork (or create a branch) on the target repository, and clone it locally so you can modify it.
2. Copy [`repolinter-action-template.yml`](./repolinter-action-template.yml) into `.github/workflows/repolinter.yml` in the cloned repository.
3. Edit the copied `repolinter.yml` and replace `<config url>` with a link to the desired policy.
   *  The link will be formatted as `https://raw.githubusercontent.com/newrelic/.github/main/{path to policy}`. For example: `https://raw.githubusercontent.com/newrelic/.github/main/repolinter-rulesets/new-relic-one-catalog-project.json` for [`repolinter-rulesets/new-relic-catalog-project.json`](../repolinter-rulesets/new-relic-one-catalog-project.json).
4. Save the file, commit it, and push the changes. Commit message I typically use is `ci: add repolinter action workflow`.
5. Open a pull request with your changes. Use `ci: Add Open Source Policy Workflow` for the title and the text in [`pr-body.md`](./pr-body.md) for the body text.

#### Alternate method

A centralized deployment of Repolinter exists in this repository under [.github/workflows/repolinter-apply.yml](../../.github/workflows/repolinter-apply.yml). This deployment serves as an alternative to the distributed workflow for teams that find it difficult to work with. Note that for this deployment to work properly `nr-opensource-bot` must have write permissions to the target repository.

1. Fork (or create a branch) on this repository, and clone it locally so you can modify it.
2. Edit `.github/workflows/repolinter-apply.yml`, adding the target repository and relative path to the policy under `jobs.apply-repolinter.strategy.matrix.include`. Save, commit, and push this file.
3. Contact a team member who can grant write permissions for the target repository to `nr-opensource-bot`.
4. Create a pull request with your changes to this repository. Make sure the checks pass before merging.

### Add Repolinter to many repositories

Adding Repolinter to many repositories requires opening a large number of identical pull requests. I recommend Google's [github-repo-automation](https://github.com/googleapis/github-repo-automation) to automate this process. Instructions to run this tool are below:

0. Make sure you have a GitHub account (or a token) with **write access** to every repository that you would like to add Repolinter to. This tool creates a branch on the target repository, and will fail if it cannot write.
   * In addition to a personal access token, your local `git` instance will need to be [setup with an SSH key](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/connecting-to-github-with-ssh) to allow cloning over SSH. The SSH key used must have the same permissions as the token.
1. Use `npm i -g @google/repo` to install the tool. You'll need NodeJS installed if it isn't already.
2. Find or create an empty workspace folder. I'll use `{work}` as the relative path to that workspace folder and `{work abs}` as the absolute path.
3. Copy [`repolinter-action-template.yml`](./repolinter-action-template.yml) to `{work}/repolinter.yml` and [`pr-body.md`](./pr-body.md) to `{work}/pr-body.md`. 
4. Create a `.repo.yml` file in `{work}`, and paste in the following contents:
   ```yaml
   githubToken: <token>
   clonePath: ./.work
   repos:
   -  org: <org>
      name: <name>
   # more repositories here
   ```
5. Generate a [GitHub personal access token](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/creating-a-personal-access-token) using the account you would like to make the PRs as. Check the `public_repo` scope when creating this token. When complete, replace `<token>` in `.repo.yml` with the generated value.
6. Replace the template under `repos` with the list of repositories you would like to add Repolinter to. You may want to use a scripting language to generate the YAML formatting for this list, as it can get rather long. An example complete configuration is shown below:
   ```yaml
   githubToken: df51c00f76bcd3a0e586d02d40bcd314a2456213
   clonePath: ./.work
   repos:
   -  org: newrelic
      name: repolinter-action
   -  org: newrelic
      name: .github
   # ...
   ```
7. Replace `{work abs}` with the absolute path to your working directory in the following command, then run it to start automatically creating PRs:
   ```sh
   REPO_CONFIG_PATH=./.repo.yml repo apply --silent --branch feature/repolinter-action --message "ci: Add Open Source Policy Workflow" --comment "$(< ./pr-body.md)" "mkdir -p .github/workflows && cp /{work abs}/repolinter.yml .github/workflows/repolinter.yml"
   ```
8. Watch the output for errors. If all goes well, you should see text like this for every repository:
   ```console
   Loaded 1 repositories from GitHub.
   Total 1 unique repositories loaded.
   companion-cube
   Executing command: mkdir -p .github/workflows && cp /Users/nkoontz/Documents/code/github-repo-automation-rollout/repolinter.yml .github/workflows/repolinter.yml
      success! https://github.com/aperture-science-incorporated/companion-cube/pull/54
   ```
   Some common problems:
     * `git@github.com: Permission denied (publickey).` - Git is not able to authenticate to GitHub over SSH. Is your git instance setup with an SSH key?
     * `cannot create branch feature/repolinter-action, skipping this repository: Error: Request failed with status code 404` - The Personal Access Token does not have permissions to write to this repository. Check that the `public_repo` scope and granted and the user has write permissions to the repository.
     * `cannot create branch feature/repolinter-action, skipping this repository: Error: Request failed with status code 422` - A branch named `feature/repolinter-action` already exists on this repository. Is there already a PR for Repolinter Action open?
     * `GaxiosError: Request failed with status code 401` - Check the Personal Access Token. Is the token pasted correctly? Does it have the permissions needed?

### Change the policy for a repository

regular deployment, alternate deployment

### Creating a Policy

## Architecture Overview

