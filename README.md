The [`copy-trigger-file.yaml` CI of this repository](https://github.com/philipp-baumann/ci-test-source-cp-file/blob/main/.github/workflows/copy-trigger-file.yaml)
creates an empty **trigger artefact file** with time stamp and pushes that to
the `target_cp` branch of the target repository at
[`philipp-baumannci-test-target-cp-file`/(https://github.com/philipp-baumann/ci-test-target-cp-file).

This test setup was motivated by the custom [the CI in`r-daily-source` branch at
`b-rodrigues/rixpkgs`](https://github.com/b-rodrigues/nixpkgs/blob/r-daily-source/.github/workflows/r-daily.yml),
which runs when pushing to a specific branch. The pushing to a specific branch
can e.g. be done by a source repository with a push trigger CI like this one.

The action is used is based on
[https://github.com/cpina/github-action-push-to-another-repository](cpina/github-action-push-to-another-repository).
Pushing the trigger file of format `_trigger-file/_trigger_$(date "+%Y-%m-%d_%H%M%S")`
from source to the root directory of the target repo is configured via a SSH
deployment key. More details and an example can be found in the [docs of the 
action](https://cpina.github.io/push-to-another-repository-docs/setup.html#setup-using-ssh-deploy-keys).

- Trigger source file:
  - `_trigger-file/_trigger_$(date "+%Y-%m-%d_%H%M%S")`
  - Created every given CRON-job interval, pushed to target, and then the folder
    and file is deleted in this source repo. Hence, the target repo will create
    only the last successful push file in the latest commit.
  