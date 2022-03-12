#Install Kubernetes on Ubuntu 18.04.6 LTS
1. Spinn upp tre Linux servrar t.ex. som Virtual Machines på VirtualBox. Kör på Ubuntu 18.04.6 LTS (Bionic Beaver). Om möjligt kör på 4 vCPU, 8 GiB RAM, 30 GiB HDD på varje VM.
2. Uppdatera Ubuntu servrarna: `sudo apt update –y && sudo apt upgrade –y`
3. Installera vim och curl: `sudo apt install vim curl –y`
4. Installera docker:
    ```
    sudo apt install docker.io -y
    sudo systemctl enable docker.service
    sudo usermod -aG docker $USER
    ```

5. Fixa cgroups problem >1.22
   ```
   sudo cat << EOF | sudo tee /etc/docker/daemon.json
   {
   "exec-opts": ["native.cgroupdriver=systemd"],
   "log-driver": "json-file",
   "log-opts": {
   "max-size": "100m"
   },
   "storage-driver": "overlay2"
   }
   EOF
   ```

6. Lägg till repot för Kubernetes: 

   `echo 'deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main' | sudo tee -a /etc/apt/sources.list.d/kubernetes.list`

7. Ladda ned Google Cloud publika nyckel: 

   `sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg`

8. Installera kubernetes:

   ```
   sudo apt update –y
   sudo apt installkubeadm=1.21.1-00 kubelet=1.21.1-00 kubectl=1.21.1-00 –y
   sudo apt-mark hold kubelet kubeadm kubectl
   ```
9. Inaktivera swap temporärt: 

   `sudo swapoff –a`

11. Lägg till ip och hostname i filen /etc/hosts

   `<ip-till-nod> <hostname>` t.ex. `10.128.0.3 k8scp` 

12. Skapa en kubeadm-config.yaml fil och lägg till följande i filen:

   ```
   apiVersion: kubeadm.k8s.io/v1beta2
   kind: ClusterConfiguration
   kubernetesVersion: 1.21.1   #<-- Use the word stable for newest version
   controlPlaneEndpoint: "k8scp:6443"   #<-- Use the node alias not the IP
   networking:
     podSubnet: 192.168.0.0/16   #<-- Match the IP range from the Calico config file 
   ```
 