# Getting Started on Red Hat Enterprise Linux
This guide is tailored towards RHEL, but should work on other distros as well.  
You might need to 

## Change default shell to bash(Optional)
The latest Macbook comes with zsh as default shell. You may change it to bash if you would like to.

1. Launch the Terminal app
2. Change default shell to bash  
    ```
    % chsh -s /bin/bash
    ```
3. You’ll be prompt to enter your password. Then, close the terminal and reopen it. The terminal will come back with bash prompt "$".


## Install Development Tools

1. Launch the Terminal app
2. Run the command to install the group Development Tools:
    ```
    $ sudo dnf groupinstall 'Development Tools'
    ```

### Verify
1. Run this command: 
    ```
    $ gcc --version
    ```
2. Verify you see the following:
    ```
    gcc (GCC) 8.4.1 20200928 (Red Hat 8.4.1-1)
    ```


## Install Docker
1. Follow the directions [here](https://docs.docker.com/engine/install/centos/) which are also summarized below.  
    ```bash
    sudo yum install -y yum-utils
    sudo yum-config-manager \
        --add-repo \
        https://download.docker.com/linux/centos/docker-ce.repo
    sudo yum install docker-ce docker-ce-cli containerd.io
    sudo groupadd docker
    sudo usermod -aG docker $USER
    ```
2. Start the docker daemon and make it so that it start automatically at boot.  
    ```
    sudo systemctl enable docker
    ```
3. Restarting you computer may be needed in order for the next step to work.

### Verify
1. Run this command: 
    ```
    $ docker run --rm hello-world
    ```
2. Verify you see output something like the following: 
    ```
    (...)
    Hello from Docker!
    This message shows that your installation appears to be working correctly.
    (...)
    ```


## Install minikube

1. Follow the directions [here](https://www.unixarena.com/2019/05/how-to-deploy-kubernetes-minikube-on-rhel-centos.html/).  
The steps are also given below:
    ```bash
    sudo yum -y install qemu-kvm libvirt libvirt-daemon-kvm
    sudo systemctl enable libvirtd
    sudo systemctl start libvirtd
    
    # Optionally check that your laptop supports VT technology.
    virt-host-validate
    
    # Check that firewalld is running
    systemctl status firewalld
    ```

2. Create a file at /etc/yum.repos.d with the following contents
    ```ini
    [kubernetes]
    name=Kubernetes
    baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-$basearch
    enabled=1
    gpgcheck=1
    repo_gpgcheck=1
    gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
    ```
3. Install and set up kubectl and additional tooling
    ```
    sudo yum -y install kubectl
    wget https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 -O minikube
    wget https://storage.googleapis.com/minikube/releases/latest/docker-machine-driver-kvm2
    chmod 755 minikube docker-machine-driver-kvm2
    sudo mv minikube docker-machine-driver-kvm2 /usr/local/bin/
    ```


## Install Go
1. Please see [here](https://github.com/stackrox/rox/blob/master/EXPECTED_GO_VERSION) for the latest version used at StackRox that you should download.  
Follow the directions [here](https://golang.org/doc/install) to install Go.

### Verify
1. Run the command to inspect the version of Go that was just installed. It should be what you expect:
    ```
    $ go version

    go version go1.16.5 linux/amd64 # This should reflect the version you just installed
    ```


## Install RocksDB dependencies

1. Instructions for installing RocksDB dependencies can be found [here](https://github.com/facebook/rocksdb/blob/master/INSTALL.md). The steps for RHEL can be found below:
    ```bash
    sudo yum install snappy-devel

    # If this fails, you may need to install snappy manually, using the following commands
    wget http://mirror.centos.org/centos/8/PowerTools/x86_64/os/Packages/snappy-devel-1.1.8-3.el8.x86_64.rpm
    sudo yum install cmake-filesystem
    sudo rpm -Uvh snappy-devel-1.1.8-3.el8.x86_64.rpm
    ```

## Install RocksDB

1. Download & install rocksdb v6.15.4 
    ```
    wget https://github.com/facebook/rocksdb/archive/refs/tags/v6.15.4.zip
    unzip v6.15.4.zip && cd v6.15.4.zip
    sudo make shared_lib
    sudo make install-shared
    sudo make static_lib
    sudo make install-static
    CGO_CFLAGS="-I/usr/local/include" CGO_LDFLAGS="-L/usr/local/lib/librocksdb.a"   go get github.com/tecbot/gorocksdb
    ```

## Install npm
1. Install npm
    ```
    sudo yum install npm
    sudo npm install --global yarn
    ```
2. Install nvm
    ```
    curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash  
    source ~/.bashrc
    ```
3. Update node
    ```
    nvm install 12.13.0
    ```

### Verify
    ```
    node --version
    
    v12.13.0 # This should match the version you installed with nvm
    ```


## Install bats (optional)
[This](https://centos.pkgs.org/7/epel-x86_64/bats-0.4.0-1.20141016git3b33a5a.el7.noarch.rpm.html) is a helpful resource  

    ```
    wget https://download-ib01.fedoraproject.org/pub/epel/7/x86_64/Packages/b/bats-0.4.0-1.20141016git3b33a5a.el7.noarch.rpm
    sudo rpm -Uvh bats-0.4.0-1.20141016git3b33a5a.el7.noarch.rpm
    ```


## Install gradle

1. Install gradle 6.7.1. Get it from [here](https://gradle.org/releases/). Use the binary version. 
2. Add the following to your .bashrc file
    ```
    export PATH=$PATH:<PATH_TO_GRADLE>/gradle-6.7.1/bin
    source ~/.bashrc
    ```


## Install Helm
1. Follow the directions [here](https://helm.sh/docs/intro/install/) which are summarized below.
2. Download the correct version of helm [here](https://github.com/helm/helm/releases)
    ```
    $ tar -zxvf helm-v3.6.2-linux-amd64.tar.gz
    $ cd linux-amd64/ #Or wherever your helm binary is
    $ sudo cp helm /usr/local/bin/
    ```

### Verify
1. Run the command to inspect the version of Helm that was just installed
    ```
    $ helm version
    version.BuildInfo{Version:"v3.6.2", (...)}
    ```


## Install Workflow Scripts

1. Clone the workflow repository:
    ```bash
    # If you choose to follow the opinionated golang installation of putting everything into a "go" folder:
    $ mkdir -p ~/go/src/github.com/stackrox/
    $ cd ~/go/src/github.com/stackrox/
    $ git clone git@github.com:stackrox/workflow.git
    ```
2. Follow the steps in the [repo's README](https://github.com/stackrox/workflow/blob/master/README.md)

### Verify
1. Run roxhelp in the terminal to verify correct installation.
```
    $ roxhelp

    Usage:
    /Users/ross/go/src/github.com/stackrox/workflow/bin/roxhelp <command_name> (to print help for command <command_name>)
    /Users/ross/go/src/github.com/stackrox/workflow/bin/roxhelp --list-all (to list all available Rox commands)
```
