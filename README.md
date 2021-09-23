# verify-git-ref

This action can be used to verify whether a git ref (branch/tag/sha) exists in a repository.  By default this action will exit with a code of 1 if the provided ref does not appear to exist.  This behavior can be changed by setting the `throw-errors` flag to false.

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
| `REF_EXISTS` | Flag indicating whether the provided reference exists:  `true\| false` |

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
        uses: im-open/verify-git-ref@v1.0.0
        with:
          branch-tag-sha: ${{ github.event.inputs.branch-tag-sha }}

      - name: Deploy
        run: ./deploy-to-dev.sh
```


## Code of Conduct

This project has adopted the [im-open's Code of Conduct](https://github.com/im-open/.github/blob/master/CODE_OF_CONDUCT.md).

## License

Copyright &copy; 2021, Extend Health, LLC. Code released under the [MIT license](LICENSE).
