# Instructions on how to install Kubernetes on to a RaspberryPi Cluster

  This git repository serves as a guide on how to install Kubernetes on to a Pi-Cluster. At certain point in this instruction [page](https://kubecloud.io/setting-up-a-kubernetes-1-11-raspberry-pi-cluster-using-kubeadm-952bbda329c8), we were unable to get work. We created an alternative approach that did work for us, and hopefully can work for you.

1. Follow instructions on this website ***up until running the script "install.sh":***
          https://kubecloud.io/setting-up-a-kubernetes-1-11-raspberry-pi-cluster-using-kubeadm-952bbda329c8
2. Skip creating the configuration file, it is not needed.
3. Run Command: `sudo swapoff -a`
4. Add `Environment="KUBELET_EXTRA_ARGS=--fail-swap-on=false"'` to `'/etc/systemd/system/kubelet.service.d/10-kubelet.conf' path.`
5. Run Command: `sudo systemctl daemon-reload`
6. Run Command: `sudo systemctl restart kubelet`

#### For 5 and 6, you will want to give it some time until it is activated. Once it is activated you can proceed to the next step

7. Initialize kubernetes as root in only the "master" pi.
  - Run Command: `sudo kubeadm init --ignore-preflight-errors="all"`
8. Run Command: `mkdir -p .kube`
9. Run Command: `sudo cp -i /etc/kubernetes/admin.conf .kube/config`
10. Run Command: `sudo chown $(id -u):$(id -g) .kube/config`
11. Once it's initialized, you will want to create the weave for the Rasberry pi's to perform as a cluster.
  - Run Command: `kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"`
12. On "worker" pi's.
  - Run Command: `sudo kubeadm join $MASTERIP:6443 --token $TOKEN --discovery-token-ca-cert-hash $HASH --ignore-preflight-errors="all"`

#### For the last step, you will want to include the ip address of the "master" pi and it's generated Hash and token

## Revise to add purpose to each step
