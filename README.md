## Github action for Contextual QA Checklists

Tests are nice, but sometimes you want an additional checklist of items to check before merging a PR
(for example, grammar check for documentation changes or last-minute check of visual look).
This action allows you to build filename-based checklists to remind the PR author about.

The action reads a checklist specification file (by default `CHECKLIST.yml`) from the repository root and submits a checklist comment to new/modified PRs based on what files were modified.

### Example

#### Checklist specification

`CHECKLIST.yml`

```yml
paths:
  "README.md":
    - Did you remember to add code examples for newly added methods?
    - Did you make sure that all the example code still functions as expected?
    - Did you remember to update screenshots to match new updates?
  "src/components/**.js":
    - Did you check that the component visual look still matches design documents?
  "docs/**":
    - Did you run changes past the copywrite team?
```

Only one property, `paths`, is supported at the moment. It takes path specifications as key and a list of items as value.

#### Action

`.github/workflows/checklist.yml`

```yml
on: [pull_request_target]

jobs:
  checklist_job:
    runs-on: ubuntu-latest
    name: Checklist job
    steps:
      - name: Checkout
        uses: actions/checkout@v1
      - name: Checklist
        uses: wyozi/contextual-qa-checklist-action@master
        with:
          gh-token: ${{ secrets.GITHUB_TOKEN }}
          # See options documentation below
```

#### Options

##### `comment-header`

Overrides the default header text in the PR comment.

##### `comment-footer`

Overrides the default footer text in the PR comment.

##### `include-hidden-files`

Includes files and folders starting with `.` when matching. Defaults to `false`.

##### `input-file`

The path to the checklist definition file. Default to `CHECKLIST.yml` in the project root.

##### `gh-token`

The Github token for you project.

##### `show-paths`

Shows the matched file path in the PR comment. Defaults to `true`.

#### Result

When matching files are updated in a PR, the action will automatically post a checklist containing items under that path's key.

See https://github.com/wyozi/contextual-qa-checklist-action/pull/10#issuecomment-565644854 for an example PR checklist

### Development

Initialize your work:

    yarn install

Build the distribution:

    yarn build-dist

Be sure to commit the contents of the `dist/` folder along with the source code.
