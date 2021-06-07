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

Step 2:
push the changes

#### Environment Varibales , Encryption and Expression & context

##### Default & Custom Environment Variables

Step 1:
https://docs.github.com/en/actions/reference/environment-variables

Step 2: env.yaml

```
name: ENV variables
on: push
env:
  WF_ENV: Avaliable to all jobs

jobs:
  log-env:
    run-on: ubuntu-latest
    env:
      JOB_ENV: Avaliabel to all steps in log-env job
    steps:
      - name: Log env variable 1
        env: 
          STEP_ENV: Avaliables to only this step
        run: |
          echo "WF_ENV: ${WF_ENV}"
          echo "JOB_ENV: ${JOB_ENV}" 
          echo "STEP_ENV: ${STEP_ENV}" 
      - name: Log env variable 2
        run: |
          echo "WF_ENV: ${WF_ENV}"
          echo "JOB_ENV: ${JOB_ENV}" 
          echo "STEP_ENV: ${STEP_ENV}" 

  log-default-env: 
    runs-on: ubuntu-latest
    steps:
      - name: Default ENV Variables 
        run: |
          echo "HOME: ${HOME}"
          echo "GITHUB_WORKFLOW: ${GITHUB_WORKFLOW}"
          echo "GITHUB_ACTION: ${GITHUB_ACTION}"
          echo "GITHUB_ACTIONS: ${GITHUB_ACTIONS}"
          echo "GITHUB_ACTOR: ${GITHUB_ACTOR}"
          echo "GITHUB_REPOSITORY: ${GITHUB_REPOSITORY}"
          echo "GITHUB_EVENT_NAME: ${GITHUB_EVENT_NAME}"
          echo "GITHUB_WORKSPACE: ${GITHUB_WORKSPACE}"
          echo "GITHUB_SHA: ${GITHUB_SHA}"
          echo "GITHUB_REF: ${GITHUB_REF}"
          echo "WF_ENV: ${WF_ENV}"
          echo "JOB_ENV: ${JOB_ENV}"
          echo "STEP_ENV: ${STEP_ENV}"
```

##### encrypting env Variables

Step 1:

Click on https://github.com/NikhilSharma-NS/githhub-actions/settings/secrets/actions/new

Step 2:
add the secret put key is WF_ENV and any value

Step 3:

```
name: ENV variables
on: push
env:
  WF_ENV: ${{secrets.WF_ENV}}

jobs:
  log-env:
    runs-on: ubuntu-latest
    env:
      JOB_ENV: Avaliabel to all steps in log-env job
    steps:
      - name: Log env variable 1
        env: 
          STEP_ENV: Avaliables to only this step
        run: |
          echo "WF_ENV: ${WF_ENV}"
          echo "JOB_ENV: ${JOB_ENV}" 
          echo "STEP_ENV: ${STEP_ENV}" 
      - name: Log env variable 2
        run: |
          echo "WF_ENV: ${WF_ENV}"
          echo "JOB_ENV: ${JOB_ENV}" 
          echo "STEP_ENV: ${STEP_ENV}" 

  log-default-env: 
    runs-on: ubuntu-latest
    steps:
      - name: Default ENV Variables 
        run: |
          echo "HOME: ${HOME}"
          echo "GITHUB_WORKFLOW: ${GITHUB_WORKFLOW}"
          echo "GITHUB_ACTION: ${GITHUB_ACTION}"
          echo "GITHUB_ACTIONS: ${GITHUB_ACTIONS}"
          echo "GITHUB_ACTOR: ${GITHUB_ACTOR}"
          echo "GITHUB_REPOSITORY: ${GITHUB_REPOSITORY}"
          echo "GITHUB_EVENT_NAME: ${GITHUB_EVENT_NAME}"
          echo "GITHUB_WORKSPACE: ${GITHUB_WORKSPACE}"
          echo "GITHUB_SHA: ${GITHUB_SHA}"
          echo "GITHUB_REF: ${GITHUB_REF}"
          echo "WF_ENV: ${WF_ENV}"
          echo "JOB_ENV: ${JOB_ENV}"
          echo "STEP_ENV: ${STEP_ENV}"
```


##### using the github_token secret for Authentication

Step 1:

```
name: ENV Variables 
on: push 
env: 
  WF_ENV: Available to all jobs 

jobs: 
  create_issue:
    runs-on: ubuntu-latest
    steps:
      - name: Create issue using REST API
        run: |
          curl --request POST \
          --url https://api.github.com/repos/${{ github.repository }}/issues \
          --header 'authorization: Bearer ${{ secrets.GITHUB_TOKEN }}' \
          --header 'content-type: application/json' \
          --data '{
            "title": "Automated issue for commit: ${{ github.sha }}",
            "body": "This issue was automatically created by the GitHub Action workflow **${{ github.workflow }}**. \n\n The commit hash was: _${{ github.sha }}_."
            }'
```

Step 2: 
push this it will create the issue.


Step 3: Creation of random file

```
name: ENV Variables 
on: push 
env: 
  WF_ENV: Available to all jobs 

jobs: 
  create_issue:
    runs-on: ubuntu-latest
    steps:
      - name: Push a random file
        run: |
          pwd 
          ls -a 
          git init
          git remote add origin "https://$GITHUB_ACTOR:${{ secrets.GITHUB_TOKEN }}@github.com/$GITHUB_REPOSITORY.git"
          git config --global user.email "nkhlsharma050@gmail.com"
          git config --global user.name "NikhilSharma-NS"
          git fetch
          git checkout master
          git branch --set-upstream-to=origin/master
          git pull
          ls -a
          echo $RANDOM >> random.txt
          ls -a 
          git add -A
          git commit -m"Random file"
          git push
      - name: Create issue using REST API
        run: |
          curl --request POST \
          --url https://api.github.com/repos/${{ github.repository }}/issues \
          --header 'authorization: Bearer ${{ secrets.GITHUB_TOKEN }}' \
          --header 'content-type: application/json' \
          --data '{
            "title": "Automated issue for commit: ${{ github.sha }}",
            "body": "This issue was automatically created by the GitHub Action workflow **${{ github.workflow }}**. \n\n The commit hash was: _${{ github.sha }}_."
            }'
```


##### encrypting and decrypting files

Step 1:
Add PASSPHRASE in 
https://github.com/NikhilSharma-NS/githhub-actions/settings/secrets/actions/new

step 2:

```
name: ENV Variables 
on: push 
env: 
  WF_ENV: Available to all jobs 

jobs:
  decrypt:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v1
      - name: Decrypt
        run: gpg --quiet --batch --yes --decrypt --passphrase="$PASSPHRASE" --output $HOME/secret.json secret.json.gpg
        env: 
          PASSPHRASE: ${{ secrets.PASSPHRASE }}
      - name: Print our file content 
        run: cat $HOME/secret.json
```



##### Expression and Contexts

Step 1:

create context.yml

Step 2;

```
name: Context
on: push

jobs:
  one:
    runs-on: ubuntu-16.04
    steps:
      - name: Dump GitHub context
        env:
          GITHUB_CONTEXT: ${{ toJson(github) }}
        run: echo "$GITHUB_CONTEXT"
      - name: Dump job context
        env:
          JOB_CONTEXT: ${{ toJson(job) }}
        run: echo "$JOB_CONTEXT"
      - name: Dump steps context
        env:
          STEPS_CONTEXT: ${{ toJson(steps) }}
        run: echo "$STEPS_CONTEXT"
      - name: Dump runner context
        env:
          RUNNER_CONTEXT: ${{ toJson(runner) }}
        run: echo "$RUNNER_CONTEXT"
      - name: Dump strategy context
        env:
          STRATEGY_CONTEXT: ${{ toJson(strategy) }}
        run: echo "$STRATEGY_CONTEXT"
      - name: Dump matrix context
        env:
          MATRIX_CONTEXT: ${{ toJson(matrix) }}
        run: echo "$MATRIX_CONTEXT"
```


##### Using Functions in Expressions

Step 1:

```
name: Context
on: push

jobs:
  functions: 
    runs-on: ubuntu-16.04
    steps:
      - name: dump
        run: |
          echo ${{ contains( 'hello', '11' ) }}
          echo ${{ startsWith( 'hello', 'he' ) }}
          echo ${{ endsWith( 'hello', '1o' ) }}
          echo ${{ format( 'Hello {0} {1} {2}', 'World', '!', '!' ) }}
  one:
    runs-on: ubuntu-16.04
    steps:
      - name: Dump GitHub context
        env:
          GITHUB_CONTEXT: ${{ toJson(github) }}
        run: echo "$GITHUB_CONTEXT"
      - name: Dump job context
        env:
          JOB_CONTEXT: ${{ toJson(job) }}
        run: echo "$JOB_CONTEXT"
      - name: Dump steps context
        env:
          STEPS_CONTEXT: ${{ toJson(steps) }}
        run: echo "$STEPS_CONTEXT"
      - name: Dump runner context
        env:
          RUNNER_CONTEXT: ${{ toJson(runner) }}
        run: echo "$RUNNER_CONTEXT"
      - name: Dump strategy context
        env:
          STRATEGY_CONTEXT: ${{ toJson(strategy) }}
        run: echo "$STRATEGY_CONTEXT"
      - name: Dump matrix context
        env:
          MATRIX_CONTEXT: ${{ toJson(matrix) }}
        run: echo "$MATRIX_CONTEXT"
```


##### the if key and Job status check functions

Step 1:

```
name: Context
on: [push, pull_request]

jobs:
  functions: 
    runs-on: ubuntu-16.04
    steps:
      - name: dump
        run: |
          echo ${{ contains( 'hello', '11' ) }}
          echo ${{ startsWith( 'hello', 'he' ) }}
          echo ${{ endsWith( 'hello', '1o' ) }}
          echo ${{ format( 'Hello {0} {1} {2}', 'World', '!', '!' ) }}
  one:
    runs-on: ubuntu-16.04
    if: github.event_name == 'push'
    steps:
      - name: Dump GitHub context
        env:
          GITHUB_CONTEXT: ${{ toJson(github) }}
        run: eccho "$GITHUB_CONTEXT"
      - name: Dump job context
        if: failure()
        env:
          JOB_CONTEXT: ${{ toJson(job) }}
        run: echo "$JOB_CONTEXT"
      - name: Dump steps context
        env:
          STEPS_CONTEXT: ${{ toJson(steps) }}
        run: echo "$STEPS_CONTEXT"
      - name: Dump runner context
        if: always()
        env:
          RUNNER_CONTEXT: ${{ toJson(runner) }}
        run: echo "$RUNNER_CONTEXT"
      - name: Dump strategy context
        env:
          STRATEGY_CONTEXT: ${{ toJson(strategy) }}
        run: echo "$STRATEGY_CONTEXT"
      - name: Dump matrix context
        env:
          MATRIX_CONTEXT: ${{ toJson(matrix) }}
        run: echo "$MATRIX_CONTEXT"
```


#### Using strategy , Matrix and Docker Container in Jobs

##### Error and Timeout Minutes

Step 1:

```
name: Shell Commands 

on: [pull_request]

jobs:
  run-shell-command:
    runs-on: ubuntu-latest
    steps: 
      - name: echo a string
        run: echo "Hello World"
        timeout-minutes: 0
      - name: multiline script 
        run: |
           node -v 
           npm -v
      - name: python Command 
        run: |
          import platform 
          print
          (platform.processor())
        shell: python
  run-windwos-commands:
    runs-on: windows-latest
    needs: ["run-shell-command"]
    steps:
      - name: Directory PowerShell
        run: Get-Location 
      - name: Directory Bash 
        run: pwd 
        shell: bash 
```


##### Using the setup-node action

Step 1: Create the file matrix.yml

Step 2: 

```
name: Matrix 
on: push 

jobs: 
  node-version:
    runs-on: ubuntu-latest
    steps: 
      - name: Log node version 
        run: node -v
      - uses: actions/setup-node@v1
        with:
          node-version: 6
      - name: Log node version 
        run: node -v
```

##### Creating a matrix for Running a Job with Different Environment

Step 1:

```
name: Matrix 
on: push 

jobs: 
  node-version:
    strategy: 
      matrix:
        os: [macos-latest, ubuntu-latest, windows-latest] 
        node_version: [6, 8, 10]
    runs-on: ${{ matrix.os }}
    steps: 
      - name: Log node version 
        run: node -v
      - uses: actions/setup-node@v1
        with:
          node-version: ${{ matrix.node_version }}
      - name: Log node version 
        run: node -v
```

##### Including and Excluding Matrix Configurations

Step 1:
```
name: Matrix 
on: push 

jobs: 
  node-version:
    strategy: 
      matrix:
        os: [macos-latest, ubuntu-latest, windows-latest] 
        node_version: [6, 8, 10]
        include: 
          - os: ubuntu-latest
            node_version: 8
            is_ubuntu_8: "true"
        exclude:
          - os: ubuntu-latest
            node_version: 6
          - os: macos-latest
            node_version: 8
    runs-on: ${{ matrix.os }}
    env: 
      IS_UBUNTU_8: ${{ matrix.is_ubuntu_8 }}
    steps: 
      - name: Log node version 
        run: node -v
      - uses: actions/setup-node@v1
        with:
          node-version: ${{ matrix.node_version }}
      - name: Log node version 
        run:  | 
          node -v
          echo $IS_UBUNTU_8
```


##### Using Docker Containers in Jobs

Step 1: create container.yml file

```
name: Container
on: push

jobs: 
  node-docker:
    runs-on: ubuntu-latest
    container:
      image: node:13.5.0-alpine3.10
    steps:
      - name: Log node version  
        run: |
          node -v
          cat /etc/os-release
```

Step 2: 

push this file

##### An Overview of a Simple Dockerized NodeJS API

Step 1: Navigate on https://github.com/alialaa/simple-docker-nodejs-api

##### Running Multiple Docker services in our workflows


Step 1;

```
name: Container
on: push

jobs: 
  node-docker:
    runs-on: ubuntu-latest
    services:
      app:
        image: alialaa17/node-api
        ports:
          - 3001:3000
      mongo:
        image: mongo
        ports:
          - "27017:27017"
    steps:
      - name: Post a user
        run: "curl -X POST http://localhost:3001/api/user -H 'Content-Type: application/json' -d'{\"username\":\"nik\",\"address\":\"pune\"}'"
      - name: Get User
        run : curl http://localhost:3001/api/users
```

Step 2:

push the changes

##### Running Docker container in individual Steps

Step 1: 

```
name: Container
on: push

jobs: 
  docker-steps:
    runs-on: ubuntu-latest
    container:
      image: node:10.18.0-jessie
    steps:
      - name: Log node version  
        run: node -v
      - name: step with docker
        uses: docker://node:12.14.1-alpine3.10
        with:
          entrypoint: "/bin/echo"
          args: 'Hello World'
      - name: log node version
        uses: docker://node:12.14.1-alpine3.10
        with:
          entrypoint: /usr/local/bin/node
          args: -v 
               

```

Step 2:

push the changes 


##### Creating our own Executable file and Running it in Our Steps

Step 1:
