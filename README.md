# Red Hat Developer Hub on Developer Sandbox with Tekton Plugin
<p align="left">
<img src="https://img.shields.io/badge/backstage-%23121011.svg?style=for-the-badge&logo=backstage&logoColor=white" alt="Backstage">  
<img src="https://img.shields.io/badge/tekton-%23121011.svg?style=for-the-badge&logo=tekton&logoColor=white" alt="Tekton">  
<img src="https://img.shields.io/badge/redhat-CC0000?style=for-the-badge&logo=redhat&logoColor=white" alt="Redhat">
<img src="https://img.shields.io/badge/openshift-%23121011.svg?style=for-the-badge&logo=openshift&logoColor=white" alt="OpenShift">
<img src="https://img.shields.io/badge/moodle-FF7F50?style=for-the-badge&logo=moodle&logoColor=white" alt="Moodle">
<img src="https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white" alt="kubernetes">
<a href="https://www.linkedin.com/in/maximiliano-gregorio-pizarro-consultor-it"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white" alt="linkedin">     
</p>
<p align="left">
  <img src="https://github.com/maximilianoPizarro/developer-hub-on-developer-sandbox/blob/main/screenshot/developer-hub-moodle.png?raw=true" width="900" title="Run On Openshift">
</p> 
<p align="left">
  <img src="https://github.com/maximilianoPizarro/developer-hub-on-developer-sandbox/blob/main/screenshot/COVER.jpeg?raw=true" width="900" title="Run On Openshift">
</p>
  
## Try Deploy [Moodle Component](https://github.com/maximilianoPizarro/moodle) Example on Red Hat Developer Sandbox  
<p align="left">
  <img src="https://github.com/maximilianoPizarro/developer-hub-on-developer-sandbox/blob/main/screenshot/developer-hub-register-moodle-topology.PNG?raw=true" width="900" title="Run On Openshift">
</p> 


[Red Hat Developer Hub](https://developers.redhat.com/learn/openshift/install-and-configure-red-hat-developer-hub-and-explore-templating-basics)

# Pre-installation

To install, you'll need to provision [Developer Sandbox](https://developers.redhat.com/developer-sandbox) for 30 days and a GitHub Account.

## Clone Repo
0. From OpenShift Dedicated, Open OpenShift Web Terminal and clone repo.
   
```bash
git clone https://github.com/maximilianoPizarro/developer-hub-on-developer-sandbox
```
```bash
Output
Welcome to the OpenShift Web Terminal. Type "help" for a list of installed CLI tools.
bash-5.1 ~ $ git clone https://github.com/maximilianoPizarro/developer-hub-on-developer-sandbox
Cloning into 'developer-hub-on-developer-sandbox'...
remote: Enumerating objects: 41, done.
remote: Counting objects: 100% (41/41), done.
remote: Compressing objects: 100% (34/34), done.
remote: Total 41 (delta 10), reused 0 (delta 0), pack-reused 0 (from 0)
Receiving objects: 100% (41/41), 79.82 KiB | 3.19 MiB/s, done.
Resolving deltas: 100% (10/10), done.
```


## Complete Parameters from files.
0. Update the app-config-rhdh.yaml file.

### Token GitHub 
[https://github.com/settings/tokens/new](https://github.com/settings/tokens/new)
```bash
-->app-config-rhdh.yaml
    ...
    integrati0ons:
      github:
        - host: github.com
          token: <<TOKEN-GITHUB-REPO>>
    ...
```

### OAuth GitHub Client
[https://github.com/settings/developers](https://github.com/settings/developers)

```bash
-->app-config-rhdh.yaml
        ...
        github:
          development:
            clientId: <<CLIENT-ID>>
            clientSecret: <<CLIENT-SECRET>>
        ...
```

### Base URL

```bash
-->app-config-rhdh.yaml
      ...
      baseUrl: <<URL>> https://redhat-developer-hub- <NAMESPACE> .apps.sandbox-m2.ll9k.p1.openshiftapps.com/
      ...
```
```bash
Example:
      ...
      baseUrl: <<URL>> https://redhat-developer-hub-maximilianopizarro5-dev.apps.sandbox-m2.ll9k.p1.openshiftapps.com/
      ...
```

### Namespace Role Bindiging
1. Update the backstage-role-binding-service-account.yaml file.
   
```bash
-->backstage-role-binding-service-account.yaml
       ...
        apiVersion: v1
        kind: ServiceAccount
        metadata:
          name: backstage-read-only
          namespace: <<NAMESPACE>> 
       ...

```
```bash
Example:
       ...
        apiVersion: v1
        kind: ServiceAccount
        metadata:
          name: backstage-read-only
          namespace: maximilianopizarro5-dev  
       ...

```

```bash
-->backstage-role-binding-service-account.yaml
       ...
        kind: RoleBinding
        apiVersion: rbac.authorization.k8s.io/v1
        metadata:
          name: 'backstage-read-only'
          namespace: <<NAMESPACE>>
        subjects:
          - kind: User
            apiGroup: rbac.authorization.k8s.io
            name: 'system:serviceaccount: <<NAMESPACE>> :backstage-read-only'
       ...

```
```bash
Example:
       ...
        kind: RoleBinding
        apiVersion: rbac.authorization.k8s.io/v1
        metadata:
          name: 'backstage-read-only'
          namespace: maximilianopizarro5-dev
        subjects:
          - kind: User
            apiGroup: rbac.authorization.k8s.io
            name: 'system:serviceaccount:maximilianopizarro5-dev:backstage-read-only'
       ...

```



## Chart Values
2. Update the values.yaml file.
   
### Cluster Router Base

```bash
-->values.yaml
      ...
        global:
          clusterRouterBase: <<CLUSTER_ROUTER_BASE>>
      ...
```

```bash
Example:
      ...
        global:
          clusterRouterBase: apps.sandbox-m2.ll9k.p1.openshiftapps.com
      ...
```
### K8S_CLUSTER_URL

```bash
-->values.yaml
      ...
      - name: K8S_CLUSTER_URL
        value: <<K8S_CLUSTER_URL>>
      ...
```

```bash
Example:
      ...
      - name: K8S_CLUSTER_URL
        value: 'https://api.sandbox-m2.ll9k.p1.openshiftapps.com:6443'
      ...
```


# Installation and Deploy Moodle Component

<p align="left">
  <img src="https://github.com/maximilianoPizarro/developer-hub-on-developer-sandbox/blob/main/screenshot/developer-hub-running.PNG?raw=true" width="900" title="Run On Openshift">
</p>

3. Apply Manifest the app-config-rhdh.yaml and the backstage-role-binding-service-account.yaml files in your namespace.

```bash
oc apply -f developer-hub-on-developer-sandbox/app-config-rhdh.yaml 
oc apply -f developer-hub-on-developer-sandbox/backstage-role-binding-service-account.yaml   
```

```bash
Output:
bash-5.1 ~ $ oc apply -f developer-hub-on-developer-sandbox/app-config-rhdh.yaml                        
configmap/app-config-rhdh configured
bash-5.1 ~ $ oc apply -f developer-hub-on-developer-sandbox/backstage-role-binding-service-account.yaml 
role.rbac.authorization.k8s.io/backstage-read-only configured
serviceaccount/backstage-read-only configured
rolebinding.rbac.authorization.k8s.io/ backstage-read-only configured
```

4. Install Developer Hub with Helm Cli

## Add OpenShift Helm Charts repo
Open OpenShift Web Terminal and run.
```bash
helm repo add openshift-helm-charts https://charts.openshift.io/
```

```bash
Output:
bash-5.1 ~ $ helm repo add openshift-helm-charts https://charts.openshift.io/
WARNING: Kubernetes configuration file is group-readable. This is insecure. Location: /home/user/.kube/config
WARNING: Kubernetes configuration file is world-readable. This is insecure. Location: /home/user/.kube/config
"openshift-helm-charts" has been added to your repositories
```

## Deploy Developer Hub using Helm Charts Values
Open OpenShift Web Terminal and run.
```bash
helm install redhat-developer-hub openshift-helm-charts/redhat-developer-hub -f developer-hub-on-developer-sandbox/values.yaml --version 1.2.2
```

```bash
Output:
bash-5.1 ~ $ helm install redhat-developer-hub openshift-helm-charts/redhat-developer-hub -f developer-hub-on-developer-sandbox/values.yaml --version 1.2.2
WARNING: Kubernetes configuration file is group-readable. This is insecure. Location: /home/user/.kube/config
WARNING: Kubernetes configuration file is world-readable. This is insecure. Location: /home/user/.kube/config
NAME: redhat-developer-hub
LAST DEPLOYED: Thu Aug 22 22:44:39 2024
NAMESPACE: maximilianopizarro5-dev
STATUS: deployed
REVISION: 1
```
5. Access to Developer Portal with GitHub Access.

<p align="left">
  <img src="https://github.com/maximilianoPizarro/developer-hub-on-developer-sandbox/blob/main/screenshot/developer-hub-github-access.PNG?raw=true" width="900" title="Run On Openshift">
</p>

6. Register Moodle Componet.

[https://github.com/maximilianoPizarro/moodle/blob/main/catalog-info.yaml](https://github.com/maximilianoPizarro/moodle/blob/main/catalog-info.yaml)

<p align="left">
  <img src="https://github.com/maximilianoPizarro/developer-hub-on-developer-sandbox/blob/main/screenshot/developer-hub-register-component.PNG?raw=true" width="900" title="Run On Openshift">
</p> 

<p align="left">
  <img src="https://github.com/maximilianoPizarro/developer-hub-on-developer-sandbox/blob/main/screenshot/developer-hub-register-catalog.PNG?raw=true" width="900" title="Run On Openshift">
</p> 

## Deploy Moodle Component with Tekton Pipelines
7. Open OpenShift Web Terminal Apply and Deploy Moodle Component with Tekton Pipelines.

```bash
oc apply -f https://raw.githubusercontent.com/maximilianoPizarro/moodle/master/pipeline.yaml
```

```bash
Output:
bash-5.1 ~ $ oc apply -f https://raw.githubusercontent.com/maximilianoPizarro/moodle/master/pipeline.yaml
persistentvolumeclaim/moodle-workspace created
task.tekton.dev/s2i-php-74 created
pipeline.tekton.dev/moodle created
```

8. Install yq Task from Edit Pipeline View. See the README.md file in the Moodle Component repository for more information about this.

<p align="left">
  <img src="https://github.com/maximilianoPizarro/botpress-server-v12/blob/master/examples/image/Captura3.PNG?raw=true" width="900" title="Run On Openshift">
</p>  
<p align="left">
  <img src="https://github.com/maximilianoPizarro/developer-hub-on-developer-sandbox/blob/main/screenshot/developer-hub-register-catalog.PNG?raw=true" width="900" title="Run On Openshift">
</p> 

9. From OpenShift, run the Moodle pipeline with parameters. 

<p align="left">
  <img src="https://github.com/maximilianoPizarro/developer-hub-on-developer-sandbox/blob/main/screenshot/developer-hub-register-moodle-pipeline.PNG?raw=true" width="900" title="Run On Openshift">
</p> 

   
10. Open the Red Hat Developer Hub portal. Look at the CI and Kubernetes section in the Moodle component.

## Red Hat Developer Hub CI
Look at the pipeline.
<p align="left">
  <img src="https://github.com/maximilianoPizarro/developer-hub-on-developer-sandbox/blob/main/screenshot/developer-hub-register-moodle-pipeline-CI.PNG?raw=true" width="900" title="Run On Openshift">
</p>
Review task pipeline logs.
<p align="left">
  <img src="https://github.com/maximilianoPizarro/developer-hub-on-developer-sandbox/blob/main/screenshot/developer-hub-register-moodle-pipeline-CI-LOG.PNG?raw=true" width="900" title="Run On Openshift">
</p>

## Red Hat Developer Hub Kubernetes
Check the health of the POD.
<p align="left">
  <img src="https://github.com/maximilianoPizarro/developer-hub-on-developer-sandbox/blob/main/screenshot/developer-hub-register-moodle-pipeline-POD.PNG?raw=true" width="900" title="Run On Openshift">
</p>
See the log from POD running.
<p align="left">
  <img src="https://github.com/maximilianoPizarro/developer-hub-on-developer-sandbox/blob/main/screenshot/developer-hub-register-moodle-pipeline-POD-RUNNING.PNG?raw=true" width="900" title="Run On Openshift">
</p>

Red Hat Developer Hub is amazing!
Build Here. Go anywhere.
   
