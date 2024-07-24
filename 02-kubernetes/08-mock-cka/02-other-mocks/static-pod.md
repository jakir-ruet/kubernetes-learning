Find the static pod path and copy the location to `/doc/static-pod-path-location.txt`.
`Answer`
```bash
kubectl get nodes
node01 ssh
sudo su or sudo -i
ps -aux | grep kubelet
cat /var/lib/kubelet/config.yaml | grep staticPodPath
echo /etc/kubernetes/manifests > /doc/static-pod-path-location.txt
cat /doc/static-pod-path-location.txt
```