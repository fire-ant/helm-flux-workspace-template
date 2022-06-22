### Flux-Helm GitOps Lab

This is a local harness for validating the delivery of Flux resources without the need to operate/manage the resources through a Git repository. It was inspired by [Flux/Cues](https://github.com/fluxcd/cues/tree/main/tools/install) and tries to enhance that expereince by wrapping the deployment process in [tilt](https://tilt.dev) and devcontainers so that it can be portable and reproducible. The idea is that it is easier to get to grips with the different flux resources and GitOps model without needing to do more than make changes to a local file structure.

### How it works
The included Devcontainer has all the necessary tools to start working locally, including Flux. You only need to bring Docker/Docker-Compose and VSCode to get started.

The tilt file deploys Flux through the community managed [Flux helm chart](https://github.com/fluxcd-community/helm-charts). In addition to this Minio is deployed and configured as a local bucket source in cluster. Once the cluster is built there is an adjacent minio-client preconfigured to watch the [resources] folder in the repo which contains the popular 'podinfo' microservice as an example. you can remove this and place what ever flux resources you wish here.

Once the minio-client is up, it will automatically sync the minio bucket contents to this folder and source-controller will retrieve the contents of preconfigured bucket source (its on a low 10s interval but you may need to be patient).

### devcontainer local setup

If you are developing on VSCode the accompanying devcontainer containers all the necessary CLI's/packages to build ansible-operators and run a local development setup.

To build the local cluster you can use [ctlptl](https://github.com/tilt-dev/ctlptl) to spin up  a [kind](https://kind.sigs.k8s.io) cluster:
```
ctlptl apply -f kind.yaml
```
to install the dependencies you can use the tiltfile with [tilt](https://tilt.dev):
```
tilt up
```

Once in play you should be able to navigate to the VSCode tab and use the 'Sources' and 'Workloads' tabs to identify that pod-info has been deployed per the resources included under the [resources](./resources/) folder.

## shutdown:
```
tilt down
ctlptl delete -f kind.yaml
```


