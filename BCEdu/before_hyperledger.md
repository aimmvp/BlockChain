## Prerequisites

http://hyperledger-fabric.readthedocs.io/en/release-1.1/prereqs.html

* useradd -m -s /bin/bash hyperledger
* echo "hyperledger ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers.d/90-cloud-init-users


* sudo apt update
* sudo apt dist-upgrade
### Docker install
  - sudo apt install -y apt-transport-https ca-certificates software-properties-common curl python unzip
  - curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
  - sudo add-apt-repository    "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
  - sudo apt update
  - sudo apt-get install -y docker-ce
  - sudo usermod -a -G docker hyperledger
  - sudo curl -L https://github.com/docker/compose/releases/download/1.19.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
  - sudo chmod +x /usr/local/bin/docker-compose
### Go language install
  - cat >> ~/.bashrc << _EOF_
  ```
  export GOPATH=$HOME/go
  export PATH=\$PATH:\$GOPATH/bin
  _EOF_
  ```
  - cat << _EOF_ |sudo tee /etc/profile.d/gopath.sh
  ```
  export GOROOT=/usr/lib/go-1.9
  export PATH=\$PATH:\$GOROOT/bin
  _EOF_
  ```
  - sudo add-apt-repository -y ppa:gophers/archive
  - sudo apt-get update
  - sudo apt-get install -y golang-1.9-go
### Node.js install
  - -- curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash - --
  - -- sudo apt-get install -y nodejs --
  - curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash
  - . ~/.bashrc
  - nvm install 8

