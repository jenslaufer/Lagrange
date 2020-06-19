1. Setup a Digital Ocean Droplet
2. Install docker-compose and docker

``shell
sudo apt-get remove docker docker-engine docker.io containerd runc
sudo apt-get update

sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

 sudo apt-get update
 sudo apt-get install docker-ce docker-ce-cli containerd.io
```

```shell
curl -L "https://github.com/docker/compose/releases/download/1.26.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
ln -sf /usr/local/bin/docker-compose /usr/bin/docker-compose
```



3. Install Gitlab runner

```shell
curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh | sudo bash
sudo apt-get install gitlab-runner
```


```shell
usermod -aG docker gitlab-runner
sudo service docker restart
``` 

4. Register the GitLab Runner

shell runner


4. Setup gitlab ci pipeline