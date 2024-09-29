install: copy init
	chmod +x /usr/local/bin/k3s
	INSTALL_K3S_EXEC="--disable=traefik --data-dir=/data/rancher/k3s --service-cidr=10.68.0.0/16 --cluster-cidr=10.58.0.0/16 --cluster-dns=10.68.0.2 --cluster-domain=cluster.local --flannel-backend=host-gw --kube-apiserver-arg=service-node-port-range=20000-40000 --kube-apiserver-arg=authorization-mode=Node,RBAC --kube-apiserver-arg=allow-privileged=true --kubelet-arg=topology-manager-policy=single-numa-node --kubelet-arg=cpu-manager-policy=static --kubelet-arg=kube-reserved=cpu=1 --kube-proxy-arg=proxy-mode=ipvs --kube-proxy-arg=masquerade-all=true --kube-proxy-arg=metrics-bind-address=0.0.0.0" INSTALL_K3S_SKIP_DOWNLOAD=true INSTALL_K3S_FORCE_RESTART=true sh ./k3s-install.sh
copy: 
	mkdir -p /usr/local/bin && cp bin/k3s /usr/local/bin/k3s
	mkdir -p /etc/rancher/k3s && cp etc/registries.yaml /etc/rancher/k3s/registries.yaml
	mkdir -p /data/rancher/k3s/agent/etc/containerd && cp etc/config.toml.tmpl.gpu /data/rancher/k3s/agent/etc/containerd/config.toml.tmpl
	rm -rf /data/rancher/k3s/agent/images/* && mkdir -p /data/rancher/k3s/agent/images/ && cp -R images/* /data/rancher/k3s/agent/images/
	rm -rf /data/rancher/k3s/server/manifests/k3s-airgap/* && mkdir -p /data/rancher/k3s/server/manifests/k3s-airgap/ && cp -R manifests/* /data/rancher/k3s/server/manifests/k3s-airgap/
init: 
	echo "*       soft    nofile  655350" >> /etc/security/limits.conf
	echo "*       hard    nofile  655350" >> /etc/security/limits.conf
	echo "*       soft    nproc   262144" >> /etc/security/limits.conf
	echo "*       hard    nproc   262144" >> /etc/security/limits.conf
	echo "vm.swappiness=0" >> /etc/sysctl.conf
	echo "net.core.somaxconn=65535" >> /etc/sysctl.conf
	echo "net.ipv4.tcp_max_tw_buckets=20000" >> /etc/sysctl.conf
	echo "net.ipv4.tcp_max_syn_backlog=100000" >> /etc/sysctl.conf
	echo "fs.file-max=1100000" >> /etc/sysctl.conf
	echo "fs.nr_open=1100000" >> /etc/sysctl.conf
	echo "kernel.pid_max=655350" >> /etc/sysctl.conf