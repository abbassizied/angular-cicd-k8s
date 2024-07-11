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
$ npm install -D sonarqube-scanner
``` 
- 
- You can run sonarqube using the command ```npm run sonar-scanner```
- 
- 
- 

## Start a Minikube cluster

- Start minikube using ```$ minikube start --driver=hyperv --cpus=4 --memory=4000m``` or ```$ minikube start --driver=docker``` 
- To check the status of the cluster ```$ minikube status```
- To check that kubectl is properly configured: ```$ kubectl cluster-info```
- Next, we need to run another command to enable Ingress addon: ```$ minikube addons enable ingress```


## Edit hosts file

-
```
$ minikube ip
192.168.49.2
```
- To enter the angular app you need to config your hosts file - add following line:
```
192.168.49.2 angular.local.k8s.com 
```
- Location of hosts file on different OS:
	- [Windows 10](https://www.groovypost.com/howto/edit-hosts-file-windows-10/)
	- [Linux (Ubuntu)](http://manpages.ubuntu.com/manpages/trusty/man5/hosts.5.html)
	- [Mac](https://www.imore.com/how-edit-your-macs-hosts-file-and-why-you-would-want#page1)
- To access one of these addresses one last thing is needed - running following command:
```sh
$ minikube tunnel
```



## deploy with helm

- 
- 
- 
- 
```sh

``` 
- 
- 
```sh
$ helm create angular_chart
$ helm install angular_chart angular_chart
$ helm uninstall angular_chart
``` 
- 
- 
- 
- 
```sh

``` 
- 
- Remove or delete deployed setup from kubernetes cluster : ```$ helm uninstall angular_chart```
- Stop minikube using : ```$ minikube stop```


## Maintenance

- Minikube provides a Dashboard for entire cluster, after typing following command it will open
```sh
$ minikube dashboard
```
- To see a resource (CPU, memory) consumption of services you can enable metrics-server minikube addon (they will be visible on a dashboard):
```sh
$ minikube addons enable metrics-server
```

## useful commands


```sh
# To delete all deployments in all namespaces, use:
$ kubectl delete deployment --all --namespace=default

# To get all the resources.
$ kubectl get pods,services,deployments,jobs,daemonset

# Delete the resources like below:
$ kubectl delete deployments <deployment>
$ kubectl delete services <services>
$ kubectl delete pods <pods>
$ kubectl delete daemonset <daemonset>

$ minikube pause
$ minikube unpause

# Lists all available minikube addons as well as their current statuses (enabled/disabled)
$ minikube addons list

# minikube addons enable ADDON_NAME
$ minikube addons enable dashboard 
$ minikube addons enable ingress
# minikube addons disable ADDON_NAME
 
# minikube addons open ADDON_NAME
$ minikube addons open dashboard --url
``` 

## Configure Jenkins with Minikube:

- Install the Kubernetes plugin in Jenkins:
	- Go to Jenkins dashboard.
	- Click on “Manage Jenkins” > “Manage Plugins”.
	- Search for “Kubernetes” and install the plugin.
	- Click on the **Available plugins** tab and search for **SSH Agent**.
	- Click on the checkbox beside the SSH Agent plugin > Install button.
	- 
	- 
- 
- 
- 
- 
- 
- 
- 
- 
```sh

``` 

