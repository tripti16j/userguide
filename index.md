# Developer Portal User Guide
## How to create catalog-info.yaml file?

>Create a file called `catalog-info.yaml` in the root of your GitHub repo and add the following YAML to it.
```yaml
---
apiVersion: backstage.io/v1alpha1
kind: System   #mandatory
metadata:
    name: OCS
spec:
    owner: credit  #mandatory and should exist in portal
---
apiVersion: backstage.io/v1alpha1
kind: Component
metadata:
    name: CreditOCSService   #name of component (kind)
    description: OCSService APIs  #optional
    labels:
        example.com/custom: custom_label_value
    annotations:  #optional
        jenkins.io/job-full-name: Creditrating/CIR/OCSService/Int/build-image
        jira/project-key: CRD   
        backstage.io/code-coverage: scm-only
        backstage.io/techdocs-ref: dir:.
        newrelic.com/dashboard-guid: Mjg0NjIxOXxWSVp8REFTSEJPQVJEfGRhOjM2MTM0MTM  #id of project in newrelic
    tags:     #optional
        - java
        - gradle
    links:  #optional
        - url: https://goyubi.atlassian.net/wiki/spaces/CRED/overview
        title: OCS Confluence
        icon: dashboard
        type: ocs-confluence-dashboard
spec:
    type: service
    owner: group:credit
    lifecycle: production
    system: OCS
    providesApis:
        - flex-api  # if writing this, make sure flex-api component exists in portal
    consumesApis:
        - 3p-apis
    dependsOn:
        - resource:ocs-db
        - component:3p-component
        - component:platform-auth-service
---
apiVersion: backstage.io/v1alpha1
kind: API
metadata:
    name: flex-api
spec:
    type: openapi
    lifecycle: production
    owner: group:credit
    system: OCS
    definition:
        $text: https://cir-ocs-qa.go-yubi.in/v2/api-docs
---
apiVersion: backstage.io/v1alpha1
kind: API
metadata:
    name: 3p-apis
spec:
    type: openapi
    lifecycle: production
    owner: thirdparty
    definition:
        $text: https://3p-cir-qa.go-yubi.in/v2/api-docs
---
apiVersion: backstage.io/v1alpha1
kind: Resource
metadata:
    name: ocs-db
    description: Stores OCS data in Mongo db
spec:
    type: database
    owner: credit
    system: OCS
    lifecycle: production
---
apiVersion: backstage.io/v1alpha1
kind: Component
metadata:
    name: 3p-component
    description: Third party APIs service
spec:
    type: service
    lifecycle: production
    owner: thirdparty
    providesApis:
        - 3p-apis
```

## How to import component yaml file in Backstage?
>Components can be manually added to Backstage by using the catalog importer available at `/catalog-import`.
To do this, simply copy/paste the URL of the YAML file into the importer. 
It would look something like this.

```https://github.com/credavenue/organization_data/blob/main/catalog-info.yaml```

## Errors while importing component
>If you get an error like shown below, it means your `catalog-info.yaml` file does not have all the required properties.
![](https://user-images.githubusercontent.com/45359109/158843668-1fb75cc8-4512-45a5-9247-2dd2be1556df.png)


