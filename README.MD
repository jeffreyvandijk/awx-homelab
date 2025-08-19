Operator & Base:

- kubectl apply -k operator
- AWX_HOST="awx.example.com"
- openssl req -x509 -nodes -days 3650 -newkey rsa:2048 -out ./base/tls.crt -keyout ./base/tls.key -subj "/CN=$- {AWX_HOST}/O=${AWX_HOST}" -addext "subjectAltName = DNS:${AWX_HOST}"
- change passwords(base/kustomization.yaml) & ingress(base/awx.yaml) vars
- kubectl apply -k base
- kubectl -n awx logs -f deployments/awx-operator-controller-manager

Registry:

- REGISTRY_HOST="registry.example.com"
- openssl req -x509 -nodes -days 3650 -newkey rsa:2048 -out ./registry/tls.crt -keyout ./registry/tls.key -subj - "/CN=${REGISTRY_HOST}/O=${REGISTRY_HOST}" -addext "subjectAltName = DNS:${REGISTRY_HOST}"
- change ingress vars(registry/ingress.yaml)
- kubectl run htpasswd -it --restart=Never --image httpd:2.4 --rm -- htpasswd -nbB <user> <password>
- change user/password vars(registry/configmap.yaml)
- kubectl apply -k registry