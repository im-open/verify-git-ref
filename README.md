# verify-git-ref

This action can be used to verify whether a git ref (branch/tag/sha) exists in a repository.  By default this action will exit with a code of 1 if the provided ref does not appear to exist.  This behavior can be changed by setting the `throw-errors` flag to false.

## Index

- [Prerequisites](#prerequisites)
- [Inputs](#inputs)
- [Outputs](#outputs)
- [Example](#example)
- [Contributing](#contributing)
  - [Incrementing the Version](#incrementing-the-version)
- [Code of Conduct](#code-of-conduct)
- [License](#license)

## Prerequisites
This action requires that the `actions/checkout` action has run and a `fetch-depth: 0` has been set.  The action cannot examine the tags and branches of the repository unless they've been pulled down.

## Inputs
| Parameter        | Is Required | Default | Description                                                                                                         |
| ---------------- | ----------- | ------- | ------------------------------------------------------------------------------------------------------------------- |
| `branch-tag-sha` | true        | N/A     | The GitHub ref (branch/tag/sha) that should be checked.                                                             |
| `throw-errors`   | false       | `true`  | Flag indicating whether the action should throw an error if the ref does not exist. Accepted values: `true\|false`. |

## Outputs
| Output       | Description                                                            |
| ------------ | ---------------------------------------------------------------------- |
| `REF_EXISTS` | Flag indicating whether the provided reference exists:  `true\|false`  |
| `REF_TYPE`   | String indicating the type of the provided reference: `tag\|branch\|sha\|unknown`       |

## Example

```yml

name: Deploy to Dev
on:
  workflow_dispatch:
    inputs: 
      branch-tag-sha: 
        description: 'The branch/tag/sha to deploy'
        required: true
jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
        with:
          fetch-depth: 0

      - name: verify version exists before deploying
        uses: im-open/verify-git-ref@v1.1.0
        with:
          branch-tag-sha: ${{ github.event.inputs.branch-tag-sha }}

      - name: Deploy
        run: ./deploy-to-dev.sh
```


## Contributing

When creating new PRs please ensure:
1. For major or minor changes, at least one of the commit messages contains the appropriate `+semver:` keywords listed under [Incrementing the Version](#incrementing-the-version).
2. The `README.md` example has been updated with the new version.  See [Incrementing the Version](#incrementing-the-version).
3. The action code does not contain sensitive information.

### Incrementing the Version

This action uses [git-version-lite] to examine commit messages to determine whether to perform a major, minor or patch increment on merge.  The following table provides the fragment that should be included in a commit message to active different increment strategies.
| Increment Type | Commit Message Fragment                     |
| -------------- | ------------------------------------------- |
| major          | +semver:breaking                            |
| major          | +semver:major                               |
| minor          | +semver:feature                             |
| minor          | +semver:minor                               |
| patch          | *default increment type, no comment needed* |

## Code of Conduct

This project has adopted the [im-open's Code of Conduct](https://github.com/im-open/.github/blob/master/CODE_OF_CONDUCT.md).

## License

Copyright &copy; 2021, Extend Health, LLC. Code released under the [MIT license](LICENSE).

[git-version-lite]: https://github.com/im-open/git-version-lite
