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
      - name: Simple Js Action
        steps:
          - name: Simple JS Action
            uses: actions/hello-world-javascript-action@v1
            
```

Step 3: 