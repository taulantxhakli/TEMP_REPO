# Docker Installation
https://docs.docker.com/engine/install/rhel/#set-up-the-repository

You can install Docker Engine in different ways, depending on your needs:

- You can
  [set up Docker's repositories](#install-using-the-repository) and install
  from them, for ease of installation and upgrade tasks. This is the
  recommended approach.

- You can download the RPM package,
  [install it manually](#install-from-a-package), and manage
  upgrades completely manually. This is useful in situations such as installing
  Docker on air-gapped systems with no access to the internet.

- In testing and development environments, you can use automated
  [convenience scripts](#install-using-the-convenience-script) to install Docker.

### Install using the rpm repository {#install-using-the-repository}

Before you install Docker Engine for the first time on any machine, you
need to set up the Docker repository. This guide, however, will detail
for the Red Hat Enterprise Linux only (using the default **yum** package manager).

#### Set up the repository

Install the `yum-utils` package (which provides the `yum-config-manager`
utility) and set up the repository.

```console
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo {{% param "download-url-base" %}}/docker-ce.repo
```

#### Install Docker Engine

1. Install Docker Engine, containerd, and Docker Compose:

   To install the latest version, run:

   ```console
   sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   ```

   If prompted to accept the GPG key, verify that the fingerprint matches
   `060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35`, and if so, accept it.

   This command installs Docker, but it doesn't enable nor start Docker. It also creates a
   `docker` group, however, it doesn't add any users to the group by default.

2. Enable and Start Docker.

   ```console
   sudo systemctl enable docker
   sudo systemctl start docker
   ```

3. Verify that the Docker Engine installation is successful by running the
   `hello-world` image.

   ```console
   sudo docker run hello-world
   ```

   This command downloads a test image and runs it in a container. When the
   container runs, it prints a confirmation message and exits.

You have now successfully installed, enabled and started the Docker Engine.


# Zabbix Git Repo with Docker Compose Installation
https://www.zabbix.com/documentation/current/en/manual/installation/containers

Based on the documentation, Zabbix officially provides:

* Separate Docker images for each Zabbix component to run as portable and self-sufficient containers.
* Compose files for defining and running multi-container Zabbix components in Docker.

For our case, we will only need the Docker Compose to begin monitoring locally.

#### Clone Zabbix's Docker Compose Git Repo
1. Create a directory to store the repository.
   
   ```console
   mkdir docker
   cd docker
   ```

3. While inside of the `docker` directory created, clone the repository.

   ```console
   git clone https://github.com/zabbix/zabbix-docker.git
   ```

4. If needed, switch to the required version of git being used for this repo.

   ```console
   git checkout 7.0
   ```

5. Compose the `yml` file to create and start containers.

   ```console
   docker compose -f ./docker-compose_v3_alpine_mysql_latest.yaml up
   ```

> [!WARNING]  
> This is not automated. Meaning, if the server is shutdown, step `5` will need to be repeated again in the directory created. 
For now, you can append a `-d` after the `up` to automate this process on start up if the system reboots.

#### Access Zabbix Dashboard

Now that Zabbix has been containerized using Docker, you can access the Dashboard front-end in the browser.

To do this, you need the local IP address of the RHEL machine, you can find this using this command.

```console
ip a
```

Look for the current NIC being used, usually located at `2` and take not of the IP address on the `inet` row.

Enter that address into the web browser, it should prompt a login screen. By default, Zabbix has credentials to log in.
* Username: Admin
* Password: zabbix

For more information on login configurations, below is the official documentation:

https://www.zabbix.com/documentation/current/en/manual/quickstart/login
