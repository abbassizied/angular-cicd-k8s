# angular-cicd-k8s

- 
- 
```sh
$ git clone https://github.com/abbassizied/angular-cicd-k8s.git
$ cd angular-cicd-k8s
$ ng new angular-demo
$ echo > Jenkinsfile
``` 

- and
```sh 
$ git add .
$ git commit -m "first commit"
$ git push -u origin main 
```

## Setting up SonarQube with Angular


- Create a sonar-project.properties file in your project root:
```
sonar.host.url=SONAR-QUBE-URL
sonar.projectKey=Project_Key # Must be unique (Primary key)
sonar.projectName=PROJECT_NAME # name that will be displayed on the dashboard
sonar.projectVersion=1.0
sonar.sourceEncoding=UTF-8
sonar.sources=src
sonar.exclusions=**/node_modules/**
sonar.tests=src # Where to pickup test files from
sonar.test.inclusions=**/*.spec.ts # only collect these for test-coverage
sonar.javascript.lcov.reportPaths=src/coverage/PROJECT_NAME/lcov.info
sonar.testExecutionReportPaths=reports/sonarqube_report.xml
``` 

- 
- 
- 
```sh

``` 
- 
- You can run sonarqube using the command ```npm run sonar-scanner -- -Dsonar.login=${SONARQUBE_REPORTER_TOKEN}```
- 
- 
- 





















