# Red Hat Developer Hub on Red Hat Developer Sandbox with Tekton Plugin

# Moodle Component Example on Red Hat OpenShift Dedicated
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

## Complete URL, Token GitHub, OAuth from app-config-rhdh.yaml file.

0. Update app-config-rhdh.yaml file.

#Token GitHub [https://github.com/settings/tokens/new](https://github.com/settings/tokens/new)

```bash
app-config-rhdh.yaml
    ...
    integrations:
      github:
        - host: github.com
          token: <<TOKEN-GITHUB-REPO>>
    ...
```

#OAuth Client [https://github.com/settings/developers](https://github.com/settings/developers)

```bash
        ...
        github:
          development:
            clientId: <<CLIENT-ID>>
            clientSecret: <<CLIENT-SECRET>>
        ...
```

#URL Developer Hub Base

```bash
      ...
      baseUrl: <<URL>> https://redhat-developer-hub-<NAMESPACE>.apps.sandbox-m2.ll9k.p1.openshiftapps.com/
      ...
```

1. Modify namespace from backstage-role-binding-service-account.yaml file.
   
```bash
backstage-role-binding-service-account.yaml
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

2. Open Web Terminal from Developer Sandbox

```bash
oc apply -f app-config-rhdh.yaml
oc apply -f backstage-role-binding-service-account.yaml
```
```bash
Output:
Welcome to the OpenShift Web Terminal. Type "help" for a list of installed CLI tools.
bash-4.4 ~ $ oc apply -f app-config-rhdh.yaml
bash-4.4 ~ $ oc apply -f backstage-role-binding-service-account.yaml
persistentvolumeclaim/moodle-workspace created
task.tekton.dev/s2i-php-74 configured
pipeline.tekton.dev/moodle configured
```
