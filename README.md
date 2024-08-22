# Red Hat Developer Hub on Red Hat Developer Sandbox with Tekton Plugin

## Moodle Component Example on Red Hat Developer Sandbox
<p align="left">
<img src="https://img.shields.io/badge/moodle-FF7F50?style=for-the-badge&logo=moodle&logoColor=white" alt="Moodle">
<img src="https://img.shields.io/badge/redhat-CC0000?style=for-the-badge&logo=redhat&logoColor=white" alt="Redhat">
<img src="https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white" alt="kubernetes">
<img src="https://img.shields.io/badge/docker-0db7ed?style=for-the-badge&logo=docker&logoColor=white" alt="Docker">
<img src="https://img.shields.io/badge/shell_script-%23121011.svg?style=for-the-badge&logo=gnu-bash&logoColor=white" alt="shell">
<a href="https://www.linkedin.com/in/maximiliano-gregorio-pizarro-consultor-it"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white" alt="linkedin">     
</p>

<p align="left">
  <img src="https://github.com/maximilianoPizarro/developer-hub-on-developer-sandbox/blob/main/screenshot/COVER.jpeg?raw=true" width="684" title="Run On Openshift">
</p>

[Developer Hub Official Documentation](https://docs.redhat.com/de/documentation/red_hat_developer_hub/1.2/html/configuring_plugins_in_red_hat_developer_hub)

# Pre-instalation

## Clone repo developer-hub-on-developer-sandbox
0. Clone repo on OpenShift Web Terminal.
   
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


## Complete URL, Token GitHub, OAuth from app-config-rhdh.yaml file.
0. Update app-config-rhdh.yaml file.

## Token GitHub 
[https://github.com/settings/tokens/new](https://github.com/settings/tokens/new)
```bash
-->app-config-rhdh.yaml
    ...
    integrations:
      github:
        - host: github.com
          token: <<TOKEN-GITHUB-REPO>>
    ...
```

## OAuth Client
[https://github.com/settings/developers](https://github.com/settings/developers)

```bash
        ...
        github:
          development:
            clientId: <<CLIENT-ID>>
            clientSecret: <<CLIENT-SECRET>>
        ...
```

## URL Base Developer Hub Base

```bash
      ...
      baseUrl: <<URL>> https://redhat-developer-hub-<NAMESPACE>.apps.sandbox-m2.ll9k.p1.openshiftapps.com/
      ...
```

1. Modify namespace from backstage-role-binding-service-account.yaml file.
   
```bash
-->backstage-role-binding-service-account.yaml
       ...
        kind: RoleBinding
        apiVersion: rbac.authorization.k8s.io/v1
        metadata:
          name: ' backstage-read-only'
          namespace: <<NAMESPACE>>
        subjects:
          - kind: User
            apiGroup: rbac.authorization.k8s.io
            name: 'system:serviceaccount:<<NAMESPACE>>:backstage-read-only'
       ...

```

## Cluster Router Base from values.yaml

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
## K8S_CLUSTER_URL from values.yaml

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


# Instalation and Deploy Moodle Component

<p align="left">
  <img src="https://github.com/maximilianoPizarro/developer-hub-on-developer-sandbox/blob/main/screenshot/developer-hub-running.PNG?raw=true" width="684" title="Run On Openshift">
</p>
<p align="left">
  <img src="https://github.com/maximilianoPizarro/developer-hub-on-developer-sandbox/blob/main/screenshot/developer-hub-running.PNG?raw=true" width="684" title="Run On Openshift">
</p>
<p align="left">
  <img src="https://github.com/maximilianoPizarro/developer-hub-on-developer-sandbox/blob/main/screenshot/developer-hub-running.PNG?raw=true" width="684" title="Run On Openshift">
</p>

2. Apply Manifest app-config-rhdh.yaml & backstage-role-binding-service-account.yaml 

```bash
oc apply -f developer-hub-on-developer-sandbox/app-config-rhdh.yaml 
oc apply -f developer-hub-on-developer-sandbox/backstage-role-binding-service-account.yaml   
```

```bash
Output:
bash-5.1 ~ $                        
configmap/app-config-rhdh configured
bash-5.1 ~ $ oc apply -f developer-hub-on-developer-sandbox/backstage-role-binding-service-account.yaml 
role.rbac.authorization.k8s.io/backstage-read-only configured
serviceaccount/backstage-read-only configured
rolebinding.rbac.authorization.k8s.io/ backstage-read-only configured
```

3. Install Developer Hub with Helm Cli

## Add repo OpenShift Helm Charts

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
4. Access to Developer Portal with GitHub Access.

<p align="left">
  <img src="https://github.com/maximilianoPizarro/developer-hub-on-developer-sandbox/blob/main/screenshot/developer-hub-github-access.PNG?raw=true" width="684" title="Run On Openshift">
</p>

5. Register Moodle Componet.

[https://github.com/maximilianoPizarro/moodle/blob/main/catalog-info.yaml](https://github.com/maximilianoPizarro/moodle/blob/main/catalog-info.yaml)

<p align="left">
  <img src="https://github.com/maximilianoPizarro/developer-hub-on-developer-sandbox/blob/main/screenshot/developer-hub-register-component.PNG?raw=true" width="684" title="Run On Openshift">
</p> 

<p align="left">
  <img src="https://github.com/maximilianoPizarro/developer-hub-on-developer-sandbox/blob/main/screenshot/developer-hub-register-catalog.PNG?raw=true" width="684" title="Run On Openshift">
</p> 

6. From OpenShift Web Terminal Apply and Deploy Moodle Component pipeline.

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

7. Install yq task from edit pipeline view.

<p align="left">
  <img src="https://github.com/maximilianoPizarro/botpress-server-v12/blob/master/examples/image/Captura3.PNG?raw=true" width="684" title="Run On Openshift">
</p>  

8. Run pipeline with parameter.

<p align="left">
  <img src="https://github.com/maximilianoPizarro/developer-hub-on-developer-sandbox/blob/main/screenshot/developer-hub-register-catalog.PNG?raw=true" width="684" title="Run On Openshift">
</p> 
   
9. From Developer Hub portal, look at the CI and Kubernetes seccion on Moodle Component

<p align="left">
  <img src="https://github.com/maximilianoPizarro/developer-hub-on-developer-sandbox/blob/main/screenshot/developer-hub-register-moodle-pipeline-CI.PNG?raw=true" width="684" title="Run On Openshift">
</p> 
<p align="left">
  <img src="https://github.com/maximilianoPizarro/developer-hub-on-developer-sandbox/blob/main/screenshot/developer-hub-register-moodle-pipeline-CI-LOG.PNG?raw=true" width="684" title="Run On Openshift">
</p>

<p align="left">
  <img src="https://github.com/maximilianoPizarro/developer-hub-on-developer-sandbox/blob/main/screenshot/developer-hub-register-moodle-pipeline-POD.PNG?raw=true" width="684" title="Run On Openshift">
</p>
<p align="left">
  <img src="https://github.com/maximilianoPizarro/developer-hub-on-developer-sandbox/blob/main/screenshot/developer-hub-register-moodle-pipeline-POD-RUNNING.PNG?raw=true" width="684" title="Run On Openshift">
</p>


   
