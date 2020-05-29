# Deploy To Kubernetes wit CD Job

## Task 1 : Get Configurations of Kubernetes CLuster

- Get CA Base64 value for
- Get API URL
- Get Token
- GoTo CI/CD Settings > Variables
- Add Variable **CERTIFICATE_AUTHORITY_DATA** value _Base64 Cert Value_ from step 1
- Add Variable **SERVER** value _APIURL_ from step 2
- Add Variable **USER_TOKEN** value _TokenValue_ from step 3
## Task 2 : Add Yaml in Lab12.2

## Task 3: Add Kube Job

- Add below code snippet in Pipeline file
  ```yaml
  deploycontainer:
  stage: deploytokube
  image: dtzar/helm-kubectl
  script:
    - kubectl config set-cluster k8s --server="${SERVER}"
    - kubectl config set clusters.k8s.certificate-authority-data ${CERTIFICATE_AUTHORITY_DATA}
    - kubectl config set-credentials gitlab --token="${USER_TOKEN}"
    - kubectl config set-context default --cluster=k8s --user=gitlab
    - kubectl config use-context default
    - sed -i "s/<VERSION>/${CI_COMMIT_SHORT_SHA}/g" deployment.yaml
    - kubectl apply -f deployment.yaml
  ```