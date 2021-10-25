Enable iptables on both master and worker:
```
sudo iptables -F
sudo update-alternatives --set iptables /usr/sbin/iptables-legacy
sudo update-alternatives --set ip6tables /usr/sbin/ip6tables-legacy
```
  
  
  
Enable cgroup on bother master and worker:

add "cgroup_memory=1 cgroup_enable=memory" too /boot/cmdline.txt


Reboot both nodes


To install k3s server on master node:
```
pi@pi4-1:~ $ curl -sfL https://get.k3s.io | sh -
[INFO]  Finding release for channel stable
[INFO]  Using v1.21.5+k3s2 as release
[INFO]  Downloading hash https://github.com/k3s-io/k3s/releases/download/v1.21.5+k3s2/sha256sum-arm64.txt
[INFO]  Downloading binary https://github.com/k3s-io/k3s/releases/download/v1.21.5+k3s2/k3s-arm64
[INFO]  Verifying binary download
[INFO]  Installing k3s to /usr/local/bin/k3s
[INFO]  Creating /usr/local/bin/kubectl symlink to k3s
[INFO]  Creating /usr/local/bin/crictl symlink to k3s
[INFO]  Skipping /usr/local/bin/ctr symlink to k3s, command exists in PATH at /usr/bin/ctr
[INFO]  Creating killall script /usr/local/bin/k3s-killall.sh
[INFO]  Creating uninstall script /usr/local/bin/k3s-uninstall.sh
[INFO]  env: Creating environment file /etc/systemd/system/k3s.service.env
[INFO]  systemd: Creating service file /etc/systemd/system/k3s.service
[INFO]  systemd: Enabling k3s unit
Created symlink /etc/systemd/system/multi-user.target.wants/k3s.service → /etc/systemd/system/k3s.service.
[INFO]  systemd: Starting k3s
```
Config file on master node - /etc/rancher/k3s/config.yaml

Run the following commands to see if the installation was successful:
```
pi@pi4-1:~ $ sudo kubectl get nodes
NAME    STATUS   ROLES                  AGE    VERSION
pi4-1   Ready    control-plane,master   112s   v1.21.5+k3s2

pi@pi4-1:~ $ sudo kubectl cluster-info
Kubernetes control plane is running at https://127.0.0.1:6443
CoreDNS is running at https://127.0.0.1:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
Metrics-server is running at https://127.0.0.1:6443/api/v1/namespaces/kube-system/services/https:metrics-server:/proxy
```
  
  
  
Get the token on master. The optput of this command will be used on worker node for joining the cluster:
```
pi@pi4-1:~ $ sudo cat /var/lib/rancher/k3s/server/node-token
```

To install k3s agent and join the cluster, run the following command:
```
pi@pi4-2:~ $ curl -sfL https://get.k3s.io | K3S_TOKEN=K10570ef4b83a2fa330e102840af2aa92387e5869b98202f17543f05c44b8a5ccc3::server:3c3b1285b2610a6c3de5ee493836ba4a K3S_URL=https://192.168.29.201:6443 sh -
```
(K3S_TOKEN from the previous command output and K3S_URL is the server's IP)
  
  
  
Check for newly joined worker on master node: 
```
pi@pi4-1:~ $ sudo kubectl get nodes
NAME    STATUS   ROLES                  AGE    VERSION
pi4-1   Ready    control-plane,master   7m1s   v1.21.5+k3s2
pi4-2   Ready    <none>                 24s    v1.21.5+k3s2
```
