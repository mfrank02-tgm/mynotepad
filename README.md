# mynotepad
useful stuff for my work and study

Docker install Fedora

```
sudo dnf -y install dnf-plugins-core
sudo dnf config-manager \
    --add-repo \
    https://download.docker.com/linux/fedora/docker-ce.repo
sudo dnf config-manager --set-enabled docker-ce-nightly
sudo dnf config-manager --set-enabled docker-ce-test
sudo dnf install docker-ce docker-ce-cli containerd.io
```



Basic and Useful Commands:

```bash
docker search <name>
docker ps -a
docker ps
docker run <name>
docker exec -t -i <name> bash

docker run -d -p 9000:9000 --name portainer --restart always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer

docker run -d -p 5432:5432 --name <name> -e POSTGRES_PASSWORD=<password> postgres
docker exec -it postgres psql -U postgres
```



docker-compose install

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```



remote mysql

```
mysql --host=<hostname or ip> -u root -p
```