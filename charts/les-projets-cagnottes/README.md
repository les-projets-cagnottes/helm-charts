# Les Projets Cagnottes

[Les Projets Cagnottes](https://github.com/les-projets-cagnottes/) is an open sourced solution for collaborative project management.

## Introduction

This chart bootstraps an instance of 'Les Projets Cagnottes'.

## Prerequisites

- Kubernetes 1.14+

## Installing the chart

To install the chart:

```bash
$ helm repo add les-projets-cagnottes https://les-projets-cagnottes.github.io/helm-charts
$ helm install les-projets-cagnottes/les-projets-cagnottes
```

## Uninstalling the chart

To uninstall/delete the deployment:

```bash
$ helm list
NAME           REVISION    UPDATED                     STATUS      CHART                          NAMESPACE
kindly-newt    1           Tue May  4 11:08:43 2021    DEPLOYED    les-projets-cagnottes-1.0.0    default
$ helm delete kindly-newt
```

### Using external PostgreSQL database

1. Create the following ExternalName Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: db-postgres
spec:
  externalName: db.example.com
  type: ExternalName
```

2. Add the following Secret

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-lesprojetscagnottes
data:
  username: <Username in Base64>
  password: <Password in Base64>
```

3. Set the following values of the chart:

```yaml
db:
  url: jdbc:postgresql://db-lesprojetscagnottes:5432/lesprojetscagnottes
  existingSecret:
    name: db-lesprojetscagnottes
    usernameKey: username
    passwordKey: password

postgresql:
  enabled: false
```

### Add Microsoft connexion

1. Add the following Secret

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: microsoft-lesprojetscagnottes
data:
  FR_LESPROJETSCAGNOTTES_MICROSOFT_CLIENT_ID: <Client ID in Base64>
  FR_LESPROJETSCAGNOTTES_MICROSOFT_CLIENT_SECRET: <Client secret in Base64>
  FR_LESPROJETSCAGNOTTES_MICROSOFT_TENANT_ID: <Tenant ID in Base64>
```

2. Update the values

```yaml
web:
  config: |-
    {
      "microsoftEnabled": true,
      "microsoftTenantId": "<Tenant ID>",
      "microsoftClientId": "<Client ID>"
    }

microsoft:
  enabled: true
  existingSecret:
    name: microsoft-lesprojetscagnottes
```

