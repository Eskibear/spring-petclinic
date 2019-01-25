steps:
1. create AKS
```bash
az login
az account set --subscription <subscription guid>
az group create --name MyResourceGroup --location <region>
az aks create -g MyResourceGroup -n myAKS --location <region> --kubernetes-version 1.10.9 --enable-addons http_application_routing --generate-ssh-keys
```
2. create dev spaces
```bash
az aks use-dev-spaces -g MyResourceGroup -n MyAKS
```
3. copy all files in `dev-tools` into project root folder
4. install my private plugin vsix at root folder.
5. run vscode command:
```
Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces
```
6. make sure the `.dockerignore` file in project root folder doesn't contains `target`
7. make sure the `azds.yaml` contains the following configuration:
```
 build:
      dockerfile: Dockerfile.develop
      useGitIgnore: false
      useDockerIgnore: false
    container:
      syncTarget: /usr/src/app
      sync:
        - "**/target/classes/**"
        - "**/target/dependency/**/*.jar"
```
8. run `mvn wro4j:run compile dependency:copy-dependencies` and F5 and wait for start and navigate to the url in IE: `http://localhost:61137/vets.html`
9. Modify code in `VetController.java#showVetList` to remove the line `vets.getVetList().addAll(this.vets.findAll());` and wait the debug console showing the following changes:
 ```
2019-01-25 14:21:36.120  INFO 60 --- [  restartedMain] .ConditionEvaluationDeltaLoggingListener : Condition evaluation unchanged
```
10. You may find the the vet list is empty.


