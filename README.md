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

```
name: Actions Workflow

on: 
  push:
  pull_request:
    types: [closed,assigned,opened,reopned]

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

Step 5 : shedule after every 5 min.

```
name: Actions Workflow

on: 
  #push:
  schedule:
    - cron: "0/5 * * * *"

  pull_request:
    types: [closed,assigned,opened,reopned]

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

##### Triggering workflow Manually using the repository Dispatch Event

Step 1: https://docs.github.com/en/actions/reference/events-that-trigger-workflows

Step 2:

sent the dispatch request 

```
POST /repos/NikhilSharma-NS/githhub-actions/dispatches HTTP/1.1
Host: api.github.com
Accept: application/vnd.github.baptiste-preview+json
Content-Type: application/json
Cache-Control: no-cache
Postman-Token: b7e135d8-02c8-0b56-5098-aee788cde6cb

{
	"event_type":"build"
}
```

Step 3:

Navigate on : https://github.com/settings/apps

and generate the tocken 

Step 4 : use the token in the request with Basic Auth 

Step 5 : Now we will 204 response of step 2

Step 6: Update in action.yml

```
name: Actions Workflow

on: 
  repository_dispatch: 
    types: [build]

  pull_request:
    types: [closed,assigned,opened,reopned]

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

Step 7:
```
POST /repos/NikhilSharma-NS/githhub-actions/dispatches HTTP/1.1
Host: api.github.com
Accept: application/vnd.github.baptiste-preview+json
Content-Type: application/json
Authorization: Basic Token
Cache-Control: no-cache
Postman-Token: 37e93ef2-4bd5-e3cb-f04b-aa859d7dfda9

{
	"event_type":"build"
}
```

Step 8 :

hit the above api and it will be triggered

Step 9 : Add the payload and access in workflow

```
name: Actions Workflow

on: 
  repository_dispatch: 
    types: [build]

  pull_request:
    types: [closed,assigned,opened,reopned]

jobs:
  run-github-actions:
    runs-on: ubuntu-latest
    steps:
      - name: payload 
        run: echo ${{github.event.client_payload.env}}
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

Step 10 : hit below api

```
POST /repos/NikhilSharma-NS/githhub-actions/dispatches HTTP/1.1
Host: api.github.com
Accept: application/vnd.github.baptiste-preview+json
Content-Type: application/json
Authorization: Basic Token
Cache-Control: no-cache
Postman-Token: 6121b2b5-0824-e9af-4799-eab1ece4e4d9

{
	"event_type":"build",
	"client_payload":{
		"env":"prod"
	}
}
```


##### Filtering workflows by Branches,tag and paths
https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#filter-pattern-cheat-sheet

Step 1: 

```
name: Actions Workflow

on:
  push: 
    branches:
      - master
      - 'feature/*'  # matches feature/abc,feature/abbb, Not feature/a/b
      - 'feature/**'  # matches  feature/a/b
    tags: 
      - v1.*
    paths:
      - '**.js'
    # paths-ignore:
    #    - 'docs/**'
    
jobs:
  run-github-actions:
    runs-on: ubuntu-latest
    steps:
      - name: payload 
        run: echo ${{github.event.client_payload.env}}
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

