#  deploy flux
k8s_yaml('dev/flux-ns.yaml')
flux = local('./get-flux.sh')
k8s_yaml(flux, allow_duplicates=True)

# deploy minio
load('ext://helm_remote', 'helm_remote')
helm_remote('minio',
            repo_name='minio',
            repo_url='https://charts.min.io/',
            namespace='flux-system',
            values='dev/minio-values.yaml')
k8s_resource(workload='minio', 
            port_forwards=[9000,9001]
            )
# set up the local bucket
k8s_yaml([
  'dev/flux-bucket.yaml',
  'dev/flux-bucket-secret.yaml',
  'dev/flux-kustomization.yaml'
])

# run minio
docker_compose('./docker-compose.yaml')
dc_resource(name='minio-client',resource_deps=['minio'])