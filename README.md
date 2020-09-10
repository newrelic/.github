[![Community Plus header](https://github.com/newrelic/opensource-website/raw/master/src/images/categories/Community_Plus.png)](https://opensource.newrelic.com/oss-category/#community-plus)

# .github

Standard policy and procedure across the New Relic GitHub organization.

## Rulesets

The following is a high-level overview of the rulesets being enforced by [repolinter-action](https://github.com/newrelic/repolinter-action).

### [NR1 Catalog](./repolinter-rulesets/new-relic-one-catalog-project.json)

 * `license-file-exists` - Checks that a `LICENSE` or `COPYING` file exists. Does not check for a particular variant of license, it is up to the repository maintainer to ensure the license is correct.
 * `readme-file-exists` - Checks that a `README.md` file exists.
 * `readme-starts-with-nr1-catalog-header` - Checks that the [NR1 Catalog header](https://github.com/newrelic/opensource-website/wiki/Open-Source-Category-Snippets#category-new-relic-one-catalog-project) is present in the first line of `README.md`. This header must be the most recent version, and may need to be updated:
```md
[![New Relic One Catalog Project header](https://github.com/newrelic/opensource-website/raw/master/src/images/categories/New_Relic_One_Catalog_Project.png)](https://opensource.newrelic.com/oss-category/#new-relic-one-catalog-project)
```
 * `readme-contains-security-section` - Checks that a `## Security` markdown header exists in `README.md`. This check is designed to determine if a security section is present.
 * `readme-contains-link-to-security-policy` - Checks that a link to the security policy is present in `README.md`. The security policy can be found under `https://github.com/newrelic/<repo-name>/security/policy` or `../../security/policy`.
 * `readme-contains-discuss-topic` - Checks that a valid link to `discuss.newrelic.com` exists in `README.md`.
 * `third-party-notices-file-exists` - Checks that a `THIRD_PARTY_NOTICES.md` file exists, does not verify that the content is correct. A `THIRD_PARTY_NOTICES.md` file may any have of the following alternate filenames:
   * `THIRD_PARTY_NOTICES*`
   * `THIRD-PARTY-NOTICES*`
   * `THIRDPARTYNOTICES*`
 * `nr1-catalog-config-file-exists` - Checks that a `catalog/config.json` file exists for the NR1 catalog metadata.
 * `nr1-catalog-config-file-exists` - Checks that a `catalog/screenshots` directory exists for the NR1 catalog metadata.
 * `nr1-catalog-screenshots-directory-exists` - Checks that a `catalog/screenshots` directory exists for the NR1 catalog metadata.
 * `nr1-catalog-documentation-file-exists` - Checks that a `catalog/documentation.md` file exists for the NR1 catalog metadata.
 * `package-scripts-present` - Checks that `eslint-fix` and `eslint-check` scripts are present in the `package.json`. These scripts may contain any commands.

### [Community Plus](./repolinter-rulesets/community-plus.yml)

 * `license-file-exists` - Checks that a `LICENSE` or `COPYING` file exists. Does not check for a particular variant of license, it is up to the repository maintainer to ensure the license is correct.
 * `readme-file-exists` - Checks that a `README.md` file exists.
 * `readme-starts-with-community-plus-header` - Checks that the [Community Plus header](https://github.com/newrelic/opensource-website/wiki/Open-Source-Category-Snippets#category-community-plus) is present in the first line of the `README`. To allow compatibility with both Markdown and reStructured text, this rule only checks the first 5 lines of the `README` for the presence of the image link (`https://github.com/newrelic/opensource-website/raw/master/src/images/categories/Community_Plus.png`) and the opensource link (`https://opensource.newrelic.com/oss-category/#community-plus`). This header must be the most recent version to pass, and may need to be updated to one of the following:

Markdown:
```md
[![Community Plus header](https://github.com/newrelic/opensource-website/raw/master/src/images/categories/Community_Plus.png)](https://opensource.newrelic.com/oss-category/#community-plus)
```
reStructured Text:
```rst
|header|

.. |header| image:: https://github.com/newrelic/opensource-website/raw/master/src/images/categories/Community_Plus.png
    :target: https://opensource.newrelic.com/oss-category/#community-plus
```
 * `readme-contains-link-to-security-policy` - Checks that a link to the security policy is present in `README.md`. The security policy can be found under `https://github.com/newrelic/<repo-name>/security/policy` or `../../security/policy`.
 * `readme-contains-discuss-topic` - Checks that a valid link to `discuss.newrelic.com` exists in `README.md`.
 * `third-party-notices-file-exists` - Checks that a `THIRD_PARTY_NOTICES.md` file exists, does not verify that the content is correct. A `THIRD_PARTY_NOTICES.md` file may any have of the following alternate filenames:
   * `THIRD_PARTY_NOTICES*`
   * `THIRD-PARTY-NOTICES*`
   * `THIRDPARTYNOTICES*`

## Notes for Ruleset Creation
* If the rule intends to check the contents of `README.md`, ensure that a [`README.rst`](https://github.com/DevDungeon/reStructuredText-Documentation-Reference) is also supported, as some projects prefer reStructured text to Markdown (ex. [newrelic-python-agent](https://github.com/newrelic/newrelic-python-agent)).