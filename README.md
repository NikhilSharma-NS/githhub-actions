# githhub-actions
githhub-actions


#### Download log archive

Step 1: click on Actions and work flow and 3 dots.

https://github.com/NikhilSharma-NS/githhub-actions/actions/runs/911558007/workflow

Step 2: click Download log archive

##### Add the Secrets 

Step1 : Navigate on
https://docs.github.com/en/actions/managing-workflow-runs

Step 2:  click on enable debug and logging
https://docs.github.com/en/actions/managing-workflow-runs/enabling-debug-logging

Step3: Navigate on secrets of repo
https://github.com/NikhilSharma-NS/githhub-actions/settings/secrets/actions/new

Step 4: add the secrets 
ACTIONS_RUNNER_DEBUG
true

ACTIONS_STEP_DEBUG 
true

##### Using Different Shells for Each Step

Step 1: Nvigate on
https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions
https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#using-a-specific-shell

Step 2: added python section

```
name: Shell Commands 

on: [push]

jobs:
  run-shell-command: 
     runs-on: ubuntu-latest
     steps: 
       - name: echo a string
         run: echo "Hello World"
       - name: multiline script
         run: | 
           node -v
           npm -v
       - name: python Command
         run: |
           import platform
           print(platform.processor())
         shell: python 



```


Step 3: added windows actions

```
name: Shell Commands 

on: [push]

jobs:
  run-shell-command: 
     runs-on: ubuntu-latest
     steps: 
       - name: echo a string
         run: echo "Hello World"
       - name: multiline script
         run: | 
           node -v
           npm -v
       - name: python Command
         run: |
           import platform
           print(platform.processor())
         shell: python 
  
  run-windows-commands:
    runs-on: windows-latest
    steps:
      - name: Directory powerShell
        run: Get-Location
      - name: Directory bash
        run: pwd
        shell: bash



```

Step 5:
for run not parallel(sequential) add needs tag and previous job id

```
name: Shell Commands 

on: [push]

jobs:
  run-shell-command: 
     runs-on: ubuntu-latest
     steps: 
       - name: echo a string
         run: echo "Hello World"
       - name: multiline script
         run: | 
           node -v
           npm -v
       - name: python Command
         run: |
           import platform
           print(platform.processor())
         shell: python 
  
  run-windows-commands:
    runs-on: windows-latest
    needs: ["run-shell-command"]
    steps:
      - name: Directory powerShell
        run: Get-Location
      - name: Directory bash
        run: pwd
        shell: bash



```

##### using the Simple Action


Step 1: Navigate on

https://github.com/actions/hello-world-javascript-action

Step 2:  create the actions.yml

```
name: Actions Workflow

on : [push]

jobs:
  run-github-actions:
    runs-on: ubuntu-latest
    steps:
      - name: Simple JS Action
        id: greet
        uses: actions/hello-world-javascript-action@v1
        with: 
          who-to-greet: Nikhil
      - name: Log Greeting Time
        run: echo "${{ steps.greet.outputs.time }}"
            
```

Step 3: Push the code

##### The Checkout Action

Step 1: List the files
```
name: Actions Workflow

on : [push]

jobs:
  run-github-actions:
    runs-on: ubuntu-latest
    steps:
      - name: List Files
        run: |
          pwd
          ls
      - name: Simple JS Action
        id: greet
        uses: actions/hello-world-javascript-action@v1
        with: 
          who-to-greet: Nikhil
      - name: Log Greeting Time
        run: echo "${{ steps.greet.outputs.time }}"
```
Step 2: Navigate on 

https://github.com/actions/checkout

step 3:

```
name: Actions Workflow

on : [push]

jobs:
  run-github-actions:
    runs-on: ubuntu-latest
    steps:
      - name: List Files
        run: |
          pwd
          ls
      - name: Checkout
        uses: actions/checkout@v1
      - name: List Files After Checkout 
        run: |
          pwd
          ls
      - name: Simple JS Action
        id: greet
        uses: actions/hello-world-javascript-action@v1
        with: 
          who-to-greet: Nikhil
      - name: Log Greeting Time
        run: echo "${{ steps.greet.outputs.time }}"
```

Step 4: Manually checkout the data

```
name: Actions Workflow

on : [push]

jobs:
  run-github-actions:
    runs-on: ubuntu-latest
    steps:
      - name: List Files
        run: |
          pwd
          ls -a
          echo $GITHUB_SHA
          echo $GITHUB_REPOSITORY
          echo $GITHUB_WORKSPACE
          echo "{{github.token}}"
          # git clone git@github.com:$GITHUB_REPOSITORY
          # git checkout $GITHUB_SHA
      - name: Checkout
        uses: actions/checkout@v1
      - name: List Files After Checkout 
        run: |
          pwd
          ls
      - name: Simple JS Action
        id: greet
        uses: actions/hello-world-javascript-action@v1
        with: 
          who-to-greet: Nikhil
      - name: Log Greeting Time
        run: echo "${{ steps.greet.outputs.time }}"
```

Step 5 : Action markeplace for refernce

https://github.com/marketplace?type=actions

#### Events ,Schedules,External Events and Filters that can Trigger a workflow Run

###### Trigger a work flow with Github Events and Activity Types

Step 1:
add the pull request in actions.yml

```
name: Actions Workflow

on : [push,pull_request]

jobs:
  run-github-actions:
    runs-on: ubuntu-latest
    steps:
      - name: List Files
        run: |
          pwd
          ls -a
          echo $GITHUB_SHA
          echo $GITHUB_REPOSITORY
          echo $GITHUB_WORKSPACE
          echo "{{github.token}}"
          # git clone git@github.com:$GITHUB_REPOSITORY
          # git checkout $GITHUB_SHA
      - name: Checkout
        uses: actions/checkout@v1
      - name: List Files After Checkout 
        run: |
          pwd
          ls
      - name: Simple JS Action
        id: greet
        uses: actions/hello-world-javascript-action@v1
        with: 
          who-to-greet: Nikhil
      - name: Log Greeting Time
        run: echo "${{ steps.greet.outputs.time }}"
```

Step 2:
Create a branch raise the pull request to master

Step 3: we can see all the checks are running

Step 4: Now navigate on https://docs.github.com/en/actions/reference/events-that-trigger-workflows

