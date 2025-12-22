# PHPCSStandards Community Health

## Community Health

Default [community health files] for all repositories in this organisation.

* [CODE_OF_CONDUCT](CODE_OF_CONDUCT.md)
* [FUNDING](FUNDING.yml)
* [SECURITY](SECURITY.md)

Individual repositories can overrule the default files by committing their own version of one or more of these files to the repository in question.  
_Keep in mind that doing so is discouraged in this organisation._

Additional community health files may be added over time and/or when GitHub adds support for them.


## Re-usable workflows

Aside from the community health files, this repository also offers a number of configuration file driven re-usable GitHub Actions workflows.

### What does "configuration file driven" mean and why are the workflows set up that way ?

What is meant by "configuration file driven", is that these workflows, by default, will silently bow out and not execute any checks.

**_Only if a specific configuration file is encountered in the root directory of the repository calling the reusable workflow, will the workflow execute the checks it is supposed to run._**

This setup allows for (other) re-usable workflows to safely reference these workflows without immediately breaking CI builds for "end-user" repositories if the end-user repository does not comply with the rules in a new CI check (yet).

It also means that, as the configuration for each tool is always in a committed file in the "user" repository, it will be relatively straight-forward for contributors to run checks locally with the same configuration as used in CI.

### Available re-usable workflows

The following re-usable workflows are available:
* [`reusable-markdownlint.yml`][reusable-markdownlint] which runs a (code-style) [linter for Markdown/CommonMark files][markdownlint-cli2].  
    **Requires**: a `.markdownlint-cli2.yml` or `.markdownlint-cli2.yaml` configuration file in the project root.
* [`reusable-phpstan.yml`][reusable-phpstan] which runs the [PHPStan] tool.  
    **Requires**: a `phpstan.neon.dist` or `phpstan.neon` configuration file in the project root.  
    **Inputs**:
    - `phpVersion`: Optional. The PHP version to use. Defaults to 'latest'.
    - `phpstanVersion`: Optional. The PHPStan version to use. Defaults to the latest available version.
* [`reusable-remark.yml`][reusable-remark] which runs a different [linter for Markdown files][remark-lint] which typically runs more QA-style checks, like checking for broken links.  
    **Requires**: a `.remarkrc` configuration file in the project root.  
    Optionally, a project can also include a `.remarkignore` file in the project root. This file will be respected, but has no influence on whether the tool will run.  
    **Inputs**:
    - `fail-on-warnings`: Optional. Whether to exit as failed when there are warnings. Defaults to "true".
* [`reusable-yamllint.yml`][reusable-yamllint] which runs two linters for Yaml files.
    1. [yamllint] which checks all YAML files for syntax validity, code style and runs some QA checks too.  
        **Requires**: a `.yamllint.yml` or `.yamllint.yaml` configuration file in the project root.  
        **Inputs**:
        + `strict`: Optional. Whether to enable strict mode. Defaults to "false".
    2. [actionlint] which runs a static analysis check on GitHub Actions workflow files only.
        _Note: this is the only check without a configuration file requirement._

Example configuration files for most of these can be found in the root directory of this repository.


## A .github repository with versioning ?

While it may be uncommon for a `.github` repository to contain tags, the tags are to allow workflows in repositories which _use_ the above mentioned re-usable workflows to "pin" to a specific version of a workflow in a way that Dependabot can still update them.

This has two benefits:
1. **Stability**  
    A change in the re-usable workflow itself, or in an action runner used by a re-usable workflow, can "break" builds.  
    For example, an update of the `DavidAnson/markdownlint-cli2-action` action runner in the [reusable-markdownlint] workflow, may contain a newer version of the `markdownlint` tooling, which may contain new rules which could fail builds in repositories using the workflow.  
    Pinning and letting Dependabot update these pins prevents these type of "unmanaged" CI breaks.
2. **Security**  
    With commit-hash pinned workflows, the dependencies used are fixed to specific versions and updates are managed, which means that it is more difficult for an attacker to be able to infiltrate the workflow runs and get access to secrets and/or cause other havoc.

> [!IMPORTANT]  
> It is best practice for tags for repositories which will be used in GitHub Actions workflows to be prefixed with `v` before the version number, so tags in this repository should start with a `v` prefix too.


[community health files]: https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file

[reusable-markdownlint]:  https://github.com/PHPCSStandards/.github/blob/main/.github/workflows/reusable-markdownlint.yml
[markdownlint-cli2]:      https://github.com/DavidAnson/markdownlint-cli2

[reusable-phpstan]:       https://github.com/PHPCSStandards/.github/blob/main/.github/workflows/reusable-phpstan.yml
[phpstan]:                https://phpstan.org/

[reusable-remark]:        https://github.com/PHPCSStandards/.github/blob/main/.github/workflows/reusable-remark.yml
[remark-lint]:            https://github.com/remarkjs/remark-lint

[reusable-yamllint]:      https://github.com/PHPCSStandards/.github/blob/main/.github/workflows/reusable-yamllint.yml
[yamllint]:               https://yamllint.readthedocs.io/en/stable/
[actionlint]:             https://github.com/rhysd/actionlint
