Vagrant.configure("2") do |config|
  config.vm.define "kubemaster" do |master|
    master.vm.box = "centos/7"
    master.vm.provider :virtualbox do |vb|
      vb.memory = 2048
      vb.cpus = 2
      vb.name = "kubemaster"
    end    
    master.vm.hostname = 'kubemaster'
    master.vm.network :private_network, ip: "192.168.1.99"
    master.vm.provision "shell", inline: <<-SHELL
    sudo sed -i '/PasswordAuthentication/d' /etc/ssh/sshd_config
    sudo echo 'PasswordAuthentication yes' >> /etc/ssh/sshd_config
    sudo systemctl reload  sshd    
    sudo swapoff -a 
    sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
    sudo yum install epel-release -y
    sudo yum update -y
    echo "192.168.1.99  kubemaster" >> /etc/hosts
    echo "192.168.1.109  kube2" >> /etc/hosts    
    echo "192.168.1.167  kube3" >> /etc/hosts   
    #echo '1' > /proc/sys/net/bridge/bridge-nf-call-iptables    
    touch  /etc/yum.repos.d/kubernetes.repo
    echo "[kubernetes]" >> /etc/yum.repos.d/kubernetes.repo
    echo "name=Kubernetes" >> /etc/yum.repos.d/kubernetes.repo
    echo "baseurl=http://yum.kubernetes.io/repos/kubernetes-el7-x86_64" >> /etc/yum.repos.d/kubernetes.repo
    echo "enabled=1" >> /etc/yum.repos.d/kubernetes.repo
    echo "gpgcheck=1" >> /etc/yum.repos.d/kubernetes.repo
    echo "repo_gpgcheck=1" >> /etc/yum.repos.d/kubernetes.repo
    echo "gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg" >> /etc/yum.repos.d/kubernetes.repo
    touch /etc/sysctl.d/k8s.conf
    echo "net.bridge.bridge-nf-call-ip6tables = 1" >> /etc/sysctl.d/k8s.conf
    echo "net.bridge.bridge-nf-call-iptables = 1" >> /etc/sysctl.d/k8s.conf    
    sysctl --system
    sed -i --follow-symlinks 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
    systemctl disable firewalld
    systemctl stop firewalld
    yum install -y yum-utils device-mapper-persistent-data lvm2
    yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    yum install -y docker-ce kubelet kubeadm kubectl kubernetes-cni    
    systemctl enable docker && systemctl start docker
    systemctl enable kubelet && systemctl start kubelet
    sed -i 's/cgroup-driver=systemd/cgroup-driver=cgroupfs/g' /etc/systemd/system/kubelet.service.d/10-kubeadm.conf
    systemctl daemon-reload
    systemctl restart kubelet    
    SHELL
  end

  config.vm.define "kube2" do |node1|
    node1.vm.box = "centos/7"
    node1.vm.provider :virtualbox do |vb|
      vb.name = "kube2"
      vb.memory = 2048
      vb.cpus = 2
    end
    node1.vm.hostname = 'kube2'
    node1.vm.network :private_network, ip: "192.168.1.109"
    node1.vm.provision "shell", inline: <<-SHELL
    sudo sed -i '/PasswordAuthentication/d' /etc/ssh/sshd_config
    sudo echo 'PasswordAuthentication yes' >> /etc/ssh/sshd_config
    sudo systemctl reload  sshd    
    sudo swapoff -a 
    sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab    
    sudo yum install epel-release -y
    sudo yum update -y
    echo "192.168.1.99  kubemaster" >> /etc/hosts
    echo "192.168.1.109  kube2" >> /etc/hosts    
    echo "192.168.1.167  kube3" >> /etc/hosts   
    touch /etc/sysctl.d/k8s.conf
    echo "net.bridge.bridge-nf-call-ip6tables = 1" >> /etc/sysctl.d/k8s.conf
    echo "net.bridge.bridge-nf-call-iptables = 1" >> /etc/sysctl.d/k8s.conf    
    sysctl --system 
    touch  /etc/yum.repos.d/kubernetes.repo
    echo "[kubernetes]" >> /etc/yum.repos.d/kubernetes.repo
    echo "name=Kubernetes" >> /etc/yum.repos.d/kubernetes.repo
    echo "baseurl=http://yum.kubernetes.io/repos/kubernetes-el7-x86_64" >> /etc/yum.repos.d/kubernetes.repo
    echo "enabled=1" >> /etc/yum.repos.d/kubernetes.repo
    echo "gpgcheck=1" >> /etc/yum.repos.d/kubernetes.repo
    echo "repo_gpgcheck=1" >> /etc/yum.repos.d/kubernetes.repo
    echo "gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg" >> /etc/yum.repos.d/kubernetes.repo
    sed -i --follow-symlinks 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
    systemctl disable firewalld
    systemctl stop firewalld
    yum install -y yum-utils device-mapper-persistent-data lvm2
    yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    yum install -y docker-ce kubelet kubeadm kubectl kubernetes-cni    
    systemctl enable docker && systemctl start docker
    sed -i 's/cgroup-driver=systemd/cgroup-driver=cgroupfs/g' /etc/systemd/system/kubelet.service.d/10-kubeadm.conf
    systemctl daemon-reload
    systemctl restart kubelet    
    SHELL
  end





  config.vm.define "kube3" do |node2|
    node2.vm.box = "centos/7"
    node2.vm.provider :virtualbox do |vb|
      vb.name = "kube3"      
      vb.memory = 2048
      vb.cpus = 2
    end
    node2.vm.hostname = 'kube3'
    node2.vm.network :private_network, ip: "192.168.1.167" 
    node2.vm.provision "shell", inline: <<-SHELL
    sudo sed -i '/PasswordAuthentication/d' /etc/ssh/sshd_config
    sudo echo 'PasswordAuthentication yes' >> /etc/ssh/sshd_config
    sudo systemctl reload  sshd    
    sudo yum install epel-release -y
    sudo swapoff -a 
    sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab    
    sudo yum update -y
    echo "192.168.1.99  kubemaster" >> /etc/hosts
    echo "192.168.1.109  kube2" >> /etc/hosts    
    echo "192.168.1.167  kube3" >> /etc/hosts  
    touch /etc/sysctl.d/k8s.conf
    echo "net.bridge.bridge-nf-call-ip6tables = 1" >> /etc/sysctl.d/k8s.conf
    echo "net.bridge.bridge-nf-call-iptables = 1" >> /etc/sysctl.d/k8s.conf    
    sysctl --system 
    touch  /etc/yum.repos.d/kubernetes.repo
    echo "[kubernetes]" >> /etc/yum.repos.d/kubernetes.repo
    echo "name=Kubernetes" >> /etc/yum.repos.d/kubernetes.repo
    echo "baseurl=http://yum.kubernetes.io/repos/kubernetes-el7-x86_64" >> /etc/yum.repos.d/kubernetes.repo
    echo "enabled=1" >> /etc/yum.repos.d/kubernetes.repo
    echo "gpgcheck=1" >> /etc/yum.repos.d/kubernetes.repo
    echo "repo_gpgcheck=1" >> /etc/yum.repos.d/kubernetes.repo
    echo "gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg" >> /etc/yum.repos.d/kubernetes.repo
    sed -i --follow-symlinks 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
    systemctl disable firewalld
    systemctl stop firewalld
    yum install -y yum-utils device-mapper-persistent-data lvm2
    yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    yum install -y docker-ce kubelet kubeadm kubectl kubernetes-cni    
    systemctl enable docker && systemctl start docker
    sed -i 's/cgroup-driver=systemd/cgroup-driver=cgroupfs/g' /etc/systemd/system/kubelet.service.d/10-kubeadm.conf
    systemctl daemon-reload
    systemctl restart kubelet
    SHELL
  end


end
