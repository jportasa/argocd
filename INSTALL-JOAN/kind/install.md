
# Create cluster


cat <<EOF | kind create cluster --name dev --config=-
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  kubeadmConfigPatches:
  - |
    kind: InitConfiguration
    nodeRegistration:
      kubeletExtraArgs:
        node-labels: "ingress-ready=true"
  extraPortMappings:
  - containerPort: 80
    hostPort: 80
    listenAddress: 127.0.0.1  # omit for 0.0.0.0
    protocol: TCP
  - containerPort: 443
    hostPort: 443
    listenAddress: 127.0.0.1  # omit for 0.0.0.0
    protocol: TCP
EOF

# Install ingress controller in namespace ingress-nginx:

kubectl apply -f https://kind.sigs.k8s.io/examples/ingress/deploy-ingress-nginx.yaml

