# pttgc-devops-lab01
Introduction to Github Action (pttgc-devops-lab01)
This module will provide the basic knowledge about Github Actions and all attendees will have hands-on experience with creating the basic workflows from scratch. This module will also include the workflow syntax, the triggers, and other building blocks of Github Actions that will be used later during the course

## Exercise 1 - First workflow
- Create new github repository
- Create `.github/workflows` directory
- Create new file for first workflow, name `first-workflow.yaml`
- Commit and push the code

Your workflow should contain these following section


### Name of the workflow
```yaml
name: <Name of your workflow>
```

### Event to trigger the workflow
```yaml
on:
  push:
    branches:
        - "main"
```

### Define jobs
```yaml
jobs:
    lab-job:
        run-ons: ubuntu-latest
        steps:
            - name: Start job
              run: echo 'Hello DevOps'   
```

*NOTE: Command line for git commit and push*
```sh
git add .
git commit -m "Add first workflow"
git push
```

## Exercise 2 - Parallelism and Job dependency

- From the `first-workflow.yaml` (from Exercise 1), add one more step to the existing job `lab-job` in the workflow
```yaml
jobs:
    lab-job:
        run-ons: ubuntu-latest
        steps:
            - name: Start job
              run: echo 'Hello DevOps'
            - name: Second Step
              run: echo 'This is second step'
```
- Commit and push the code and check the `Action` UI to see how the workflow is executed
- Next is to add another job to the existing workflow
```yaml
jobs:
    lab-job:
        run-ons: ubuntu-latest
        steps:
            - name: Start job
              run: echo 'Hello DevOps'
            - name: Second Step
              run: echo 'This is second step'
    second-job:
        run-ons: ubuntu-latest
        steps:
            - name: Start Second job
              run: echo 'Hello Second Job'
```
- Commit and push the code and check the `Action` UI to see how the workflow is executed

## Exercise 3 - Create Job dependency
- From the `first-workflow.yaml`, we will create jobs dependency
- Add `needs` configuration to `second-job` to make it depends on `lab-job`, For example
```yaml
jobs:
    lab-job:
        run-ons: ubuntu-latest
        steps:
            - name: Start job
              run: echo 'Hello DevOps'
            - name: Second Step
              run: echo 'This is second step'
    second-job:
        run-ons: ubuntu-latest
        needs: lab-job
        steps:
            - name: Start Second job
              run: echo 'Hello Second Job'
```
- Commit and push the code and check the `Action` UI to see how the workflow is executed

## Exercise 4 - Actions
- Create new workflow name `use-action.yaml`
```yaml
name: <Name of your workflow>
on:
  push:
    branches:
        - "main"
jobs:
    prepare:
        run-ons: ubuntu-latest
        steps:
            - uses: actions/checkout@v3
            - uses: actions/setup-dotnet@v2
            - run: dotnet --info
```