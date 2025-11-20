# Migration-of-Ingress-to-gateway-api
“Migration of Kubernetes traffic routing from NGINX Ingress to Gateway API using NGINX Gateway Fabric for improved scalability, security, and modern routing capabilities.”
1)  kind create cluster --name ingress-gw --config kind-config.yaml
2)  kubectl create namespace ingress-migration

# Deploy app + existing ingress
3)  kubectl apply -f nginx-app.yaml
4)  kubectl apply -f app-ingress.yaml

# Install Gateway API CRDs
5)  kubectl apply -f https://github.com/kubernetes-sigs/gateway-api/releases/latest/download/standard-install.yaml

# Install NGINX Gateway Fabric controller
6)  helm install nginx-gateway oci://ghcr.io/nginxinc/charts/nginx-gateway-fabric \
--namespace nginx-gateway --create-namespace --set service.type=NodePort

# Create Gateway API resources (parallel with ingress)
7)  kubectl apply -f gatewayclass.yaml
8) kubectl apply -f gateway.yaml
9) kubectl apply -f httproute.yaml

# Test Gateway API path
10) kubectl port-forward svc/nginx-gateway-nginx-gateway-fabric -n nginx-gateway 8080:80


# After verification → Remove old ingress
11) kubectl delete ingress nginx-ingress -n ingress-migration
