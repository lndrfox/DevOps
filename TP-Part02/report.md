# TP PART 02

## Test containers

Test  containers are java librairies that allows us to run docker containers and use them for automated integration testing. 

## Github Action configuration

A firectory .github/workflows need to be created at the root of the project. Then in this directory main.yml can be created to configure your CI pipeline. Here is the state of my main.yml at this stage with comments to explain what it's doing.

``` yml
name: CI devops 2023 # name of the workflow that will appear in the github actions tab
on:
  #to begin you want to launch this job in main and develop
  push: #the workflow will be started on push events
    branches: [main,develop] # it will consider the branches main and develop for the push events
  pull_request:

jobs: # here we declare the various jobs of the workflow, they will run in parralel unless specifed otherwise
  test-backend: # here we declare the job test-backend
    runs-on: ubuntu-22.04 # we specify the distribution it will run on, here ubuntu version 22.04
    steps: # finally we declare the steps of the job 
     #checkout your github code using actions/checkout@v2.5.0
      - uses: actions/checkout@v2.5.0 # this step will run with v 2.5.0 of the checkout action, it will checkout my repository on the runner so that other scripts/actions can be run against my code 

     #do the same with another action (actions/setup-java@v3) that enable to setup jdk 17
      - name: Set up JDK 17 # name of the step
        uses : actions/setup-java@v3 # will use version 3 of setup-java action which sets up a jdk and build tools such as maven 
        with: # specifying options for the action
          java-version: 17 # we use java 17 
          distribution: 'corretto' # the distribution needs to be specified

     #finally build your app with the latest command
      - name: Build and test with Maven # name of the step
        run: mvn clean verify --file ./TP-Part01/Backend/simpleapi/pom.xml # we run clean to clear the cache, then verify will build and finally run unit and integration tests, since the pom.xml is not at the root of the repository we specify it's path with --file




```

### Quality gate configuration 

To configure the quality gate I used the following for the run line of the test job in the github actions main.yml file. 

```yml
        run: mvn -B verify sonar:sonar -Dsonar.projectKey=lndrfox_DevOps -Dsonar.organization=lndrfox -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=${{ secrets.SONAR_TOKEN }}  --file ./TP-Part01/Backend/simpleapi/pom.xml

```
