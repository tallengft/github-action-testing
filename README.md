# github-action-testing

# Git Hub Workflow Approval
Example of a simple workflow that simulates using GitHub environments with an approval step:

![approve-wrokflow](./docs/approve-wrokflow.png)

Adding an `environment` variable to each specifies the envrioment that the job references. You can provide the environment as only the environment `name`, or as an environment object with the `name` and `url`. The URL maps to environment_url in the deployments API.

```yml
environment: staging_environment
```

```
environment:
  name: production_environment
  url: https://github.com
```

The URL can be an expression and can use any context except for the secrets context. For more information about expressions, see ["Context and expression syntax for GitHub Actions."](https://docs.github.com/en/actions/reference/context-and-expression-syntax-for-github-actions)

If the environment does not exist it will be created automatically, you can manually create/edit environment via GitHub by going to the repositories `settings` page and selecting `Environments`:

![approve-environment](./docs/approve-environment.png)

If you wish to add an approval for a environment, such as production, you can manually specify this via the github environment page to add up to 6 reviewers:

![approve-environment-settings](./docs/approve-environment-settings.png)

This will require one of the users to manually approve the workflow before it can proceed with the GitHub Action job using that environment, the approvers will receive an email notifying them of a new approval request:

![approve-approval](./docs/approve-approval.png)

---

# Permissions for workflow_dispatch
It is possible to disable workflow jobs with an IF statement on each job, such as

```yml
jobs:
  PR-Comment:
    if: github.actor != 'kingthorin' && github.actor != 'wstgbot' && github.actor != 'ThunderSon' && github.actor != 'rejahrehim' && github.actor != 'victoriadrake'  
    runs-on: ubuntu-latest
    steps:
    - name: PR Comment
```

---

# Create Release

This job produces a semantic version for a repository using the repository's git history utilizing the [Git Semantic Version](https://github.com/marketplace/actions/git-semantic-version) action. 

This action is designed to facilitate assigning version numbers during a build automatically while publishing version that only increment by one value per release. To accomplish this, the next version number is calculated along with a commit increment indicating the number of commits for this version. The commit messages are inspected to determine the type of version change the next version represents. Including the term (MAJOR) or (MINOR) in the commit message alters the type of change the next version will represent.

If there is no file changes and the workflow is run manualy then the Semantic Version action will not produce a new version, to work around this the Create Release steps have an additional `continue-on-error: true` added. 

---

# Only run on paths

**Example including paths**

If at least one path matches a pattern in the `paths` filter, the workflow runs. To trigger a build anytime you push a JavaScript file, you can use a wildcard pattern.

```yml
on:
  push:
    paths:
    - '**.js'
```

``yml
on:
  push:
    paths:
      - docs/*

```

---