# verify-git-ref

This action can be used to verify whether a git ref (branch/tag/sha) exists in a repository. By default this action will exit with a code of 1 if the provided ref does not appear to exist. This behavior can be changed by setting the `throw-errors` flag to false.

## Index <!-- omit in toc -->

- [verify-git-ref](#verify-git-ref)
  - [Prerequisites](#prerequisites)
  - [Inputs](#inputs)
  - [Outputs](#outputs)
  - [Usage Examples](#usage-examples)
  - [Contributing](#contributing)
    - [Incrementing the Version](#incrementing-the-version)
    - [Source Code Changes](#source-code-changes)
    - [Updating the README.md](#updating-the-readmemd)
  - [Code of Conduct](#code-of-conduct)
  - [License](#license)

## Prerequisites

This action requires that the `actions/checkout` action has run and a `fetch-depth: 0` has been set. The action cannot examine the tags and branches of the repository unless they've been pulled down.

## Inputs

| Parameter        | Is Required | Default | Description                                                                                                         |
|------------------|-------------|---------|---------------------------------------------------------------------------------------------------------------------|
| `branch-tag-sha` | true        | N/A     | The GitHub ref (branch/tag/sha) that should be checked.                                                             |
| `throw-errors`   | false       | `true`  | Flag indicating whether the action should throw an error if the ref does not exist. Accepted values: `true\|false`. |

## Outputs

| Output       | Description                                                                       |
|--------------|-----------------------------------------------------------------------------------|
| `REF_EXISTS` | Flag indicating whether the provided reference exists: `true\|false`              |
| `REF_TYPE`   | String indicating the type of the provided reference: `tag\|branch\|sha\|unknown` |

## Usage Examples

```yml
name: Deploy to Dev
on:
  workflow_dispatch:
    inputs:
      branch-tag-sha:
        description: "The branch/tag/sha to deploy"
        required: true
jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0

      - name: verify version exists before deploying
        # You may also reference just the major or major.minor version
        uses: im-open/verify-git-ref@v1.2.1
        with:
          branch-tag-sha: ${{ github.event.inputs.branch-tag-sha }}

      - name: Deploy
        run: ./deploy-to-dev.sh
```

## Contributing

When creating PRs, please review the following guidelines:

- [ ] The action code does not contain sensitive information.
- [ ] At least one of the commit messages contains the appropriate `+semver:` keywords listed under [Incrementing the Version] for major and minor increments.
- [ ] The README.md has been updated with the latest version of the action.  See [Updating the README.md] for details.

### Incrementing the Version

This repo uses [git-version-lite] in its workflows to examine commit messages to determine whether to perform a major, minor or patch increment on merge if [source code] changes have been made.  The following table provides the fragment that should be included in a commit message to active different increment strategies.

| Increment Type | Commit Message Fragment                     |
|----------------|---------------------------------------------|
| major          | +semver:breaking                            |
| major          | +semver:major                               |
| minor          | +semver:feature                             |
| minor          | +semver:minor                               |
| patch          | *default increment type, no comment needed* |

### Source Code Changes

The files and directories that are considered source code are listed in the `files-with-code` and `dirs-with-code` arguments in both the [build-and-review-pr] and [increment-version-on-merge] workflows.  

If a PR contains source code changes, the README.md should be updated with the latest action version.  The [build-and-review-pr] workflow will ensure these steps are performed when they are required.  The workflow will provide instructions for completing these steps if the PR Author does not initially complete them.

If a PR consists solely of non-source code changes like changes to the `README.md` or workflows under `./.github/workflows`, version updates do not need to be performed.

### Updating the README.md

If changes are made to the action's [source code], the [usage examples] section of this file should be updated with the next version of the action.  Each instance of this action should be updated.  This helps users know what the latest tag is without having to navigate to the Tags page of the repository.  See [Incrementing the Version] for details on how to determine what the next version will be or consult the first workflow run for the PR which will also calculate the next version.

## Code of Conduct

This project has adopted the [im-open's Code of Conduct](https://github.com/im-open/.github/blob/main/CODE_OF_CONDUCT.md).

## License

Copyright &copy; 2023, Extend Health, LLC. Code released under the [MIT license](LICENSE).

<!-- Links -->
[Incrementing the Version]: #incrementing-the-version
[Updating the README.md]: #updating-the-readmemd
[source code]: #source-code-changes
[usage examples]: #usage-examples
[build-and-review-pr]: ./.github/workflows/build-and-review-pr.yml
[increment-version-on-merge]: ./.github/workflows/increment-version-on-merge.yml
[git-version-lite]: https://github.com/im-open/git-version-lite
