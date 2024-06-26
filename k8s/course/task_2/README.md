# Replace port number 6433 with 6443 in the kube config file
sed -i 's/6433/6443/' /root/.kube/config

# Replace ca-authority.crt with ca.crt in the kube-apiserver manifest file
sed -i 's/ca-authority.crt/ca.crt/' /etc/kubernetes/manifests/kube-apiserver.yaml

# Restart kubelet service
systemctl restart kubelet

# Update the image of the coredns deployment
kubectl set image deployment/coredns -n kube-system \
    coredns=registry.k8s.io/coredns/coredns:v1.8.6

# Allow scheduling of pods on node01
kubectl uncordon node01

# Copy all images from the directory '/media' on the controlplane node to '/web' directory on node01
scp /media/* node01:/web

# Apply the Persistent Volume and Persistent Volume Claim manifests
kubectl apply -f manifests/data-pv.yml
kubectl apply -f manifests/data-pvc.yml

# Deploy the file server pod and service
kubectl apply -f manifests/gop-fileserver-pod.yml
kubectl apply -f manifests/gop-fs-service.yml