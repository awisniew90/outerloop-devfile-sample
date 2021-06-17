# outerloop-devfile-sample

Sample parent and child devfiles for various outerloop scenarios


## Variables

```
variables:
  outerloop-image: "https://github.com/OpenLiberty/application-stack/releases/download/outer-loop-0.5.1/Dockerfile"
  outerloop-scc: "true"
```

### Shared class cache for image build

The `outerloop-scc` variable can be set to true or false which will be the value for the OPENJ9_SCC build argument when building the outerloop image. 

### Outerloop Dockerfile

The `outerloop-image` variable is used to set the url to the outerloop Dockerfile.


## Outerloop image build component

```
- name: mybuildimage
    image:
      imageName: application-stack-intro
      dockerfile:
        location: "{{outerloop-image}}"
        args: [ "OPENJ9_SCC={{outerloop-scc}}" ]
```

## Outerloop deployment component

```
- name: myk8sdeploy
    kubernetes:
      uri: "https://github.com/OpenLiberty/application-stack/releases/download/outer-loop-0.5.1/app-deploy.yaml"
```

## Outerloop deployment command

```
- id: deployk8s
    apply:
      component: myk8sdeploy
      group:
        kind: deploy
        isDefault: true
    attributes: 
      - name: COMPONENT_NAME
        value: application-stack-intro
      - name: CONTAINER_IMAGE
        value: application-stack-intro
      - name: PORT
        value: 9080
```
