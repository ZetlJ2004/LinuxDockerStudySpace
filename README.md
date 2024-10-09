# Linux-Docker Examples

## Init

### Download 
> 1.ISO file<br>
2.Vbox<br>
3.Bitvise SSH Client
<hr>
### Creat Virtual machine
Use the ISO file in Vbox to make your local virtual machine.

### Check your network
```sh
ping www.baidu.com
```
<hr>
use this sentence to check your network,for me,I can't connect net.

### Save network
```sh
ls /etc/sysconfig/network-scripts/
```

use this sentence to check your network files,the first file is what you need.

![alt text](image.png)

Then open it and make ONBOOT to yes

![alt text](image-2.png)

>You can also change DNS,but it's not necessary.
<hr>

### Finish Qusetion
Now you have network to ping,you can change these:
>
```sh
vi /etc/yum.repos.d/CentOS-Base.repo
```
>
可以通过 vi 命令编辑 /etc/yum.repos.d/CentOS-Base.repo 文件，将其中的 mirrorlist 行用 # 号注释掉，并将 baseurl 行取消注释，并修改为其他可靠的镜像地址。
如将下图四个 baseurl 地址按顺序修改为阿里云的镜像地址：
```sh
baseurl=http://mirrors.aliyun.com/centos/$releasever/os/$basearch/
```
```sh
baseurl=http://mirrors.aliyun.com/centos/$releasever/updates/$basearch/
```
```sh
baseurl=http://mirrors.aliyun.com/centos/$releasever/extras/$basearch/
```
```sh
baseurl=http://mirrors.aliyun.com/centos/$releasever/centosplus/$basearch/
```
>*原文链接：https://blog.csdn.net/weixin_52597907/article/details/141113817*
<hr>

### connect SSH
**Open SSH service in Vbox**
Install SSH service:

```sh
sudo apt-get install openssh-server
```

Open SSH service:

```sh
sudo service ssh start
```

**端口转发**

Do like this
![alt text](image-3.png)
>Please on't leave the local and virtual ports the same,you can make it different,such as 10022 to 22.

<hr>

### Install docker
*congratulations!* You can install docker now!<br>
Now input these words:

**Set up the repository**

Install the package (which provides the utility) and set up the repository.

***yum-utils & yum-config-manager***

```sh
sudo yum install -y yum-utils
```

```sh
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

**Install Docker Engine**

1.Install Docker Engine, containerd, and Docker Compose:
To install the latest version, run:


'sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin'

If prompted to accept the GPG key, verify that the fingerprint matches , and if so, accept it.
`060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35`

This command installs Docker, but it doesn't start Docker. It also creates a group, however, it doesn't add any users to the group by default.
2.Start Docker.

```sh
sudo systemctl start docker
```

3.Verify that the Docker Engine installation is successful by running the image.hello-world

```sh
sudo docker run hello-world
```

This command downloads a test image and runs it in a container. When the container runs, it prints a confirmation message and exits.
<hr>

### 1. **Orchestration System？**

容器编排系统（Orchestration System）是指自动管理、协调和调度多个容器服务运行的系统。它能够处理容器的生命周期管理、服务发现、负载均衡、自动扩缩容、网络管理等。典型的容器编排系统包括 **Kubernetes (k8s)** 和 **Docker Compose**。

#### **为什么需要Docker Compose？**
当使用容器来部署复杂的多服务应用时，每个服务（如数据库、web服务器等）可能需要独立的容器。Docker Compose 提供了一种简化多容器管理的方式，它允许开发者定义多服务应用的容器、网络、卷等配置，并通过一个命令来启动所有相关服务。

#### **Docker Compose的优点：**
- **易于使用**：通过一个 `docker-compose.yml` 文件可以管理所有的容器配置。
- **跨平台支持**：Docker Compose 可以在不同的环境中使用（开发、测试、生产），确保环境一致性。
- **服务编排**：它能够自动设置服务之间的依赖关系，如启动顺序、健康检查等。
- **多服务管理**：允许将应用程序分解为多个独立的微服务，彼此通过网络进行交互。

### 2. **Kubernetes (k8s) 与 Docker Compose 的适用场景？**

- **Kubernetes (k8s)**：是一个企业级的容器编排系统，适合管理大规模、动态、多节点环境的微服务架构。它具有更丰富的功能，如自动扩容、滚动更新、服务发现、容器健康检查、自动恢复等，适合生产环境的高可用性需求。
  
- **Docker Compose**：主要用于本地开发、测试和小规模部署。它更适合简化开发人员的工作流程，提供了管理多容器应用程序的方便方式。Docker Compose 不提供 Kubernetes 那么多的高级功能，如集群管理、自动扩展等，但其简单性使得它非常适合小规模应用和开发阶段使用。

#### **适用场景对比：**
- **Kubernetes**：适用于分布式架构、需要高可用性、负载均衡和自动扩展的生产环境。
- **Docker Compose**：适用于开发、测试环境，或者中小型项目。它允许轻松地定义和管理多个容器，并简化本地环境的配置。

### 3. **编写 Docker Compose 文件的最大特点？**

Docker Compose 文件是以 `YAML` 格式编写的，最大的特点是简单、直观，并且将所有服务、网络、卷和环境变量等配置集中在一个文件中进行管理。这种集中管理的方式非常适合多服务的项目，它允许开发人员清晰地描述项目中的每个服务如何运行、相互之间如何依赖和通信。

例如，一个典型的 `docker-compose.yml` 文件如下：
```yaml
version: '3'
services:
  web:
    image: nginx:latest
    ports:
      - "80:80"
  database:
    image: mysql:8
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
    volumes:
      - ./data:/var/lib/mysql
```
通过这种方式，所有的服务及其配置都可以集中定义，使用 `docker-compose up` 命令即可启动。

### 4. **MySQL 示例：**

#### **步骤**：
- **创建 MySQL 服务目录**：在宿主机的 `/home/services/mysql/` 目录下创建用于存放 MySQL 数据的 `data` 目录。
  ```bash
  mkdir -p /home/services/mysql/data
  ```

- **编写 docker-compose.yml**：
  ```yaml
  version: '3'
  services:
    mysql:
      image: mysql:8
      environment:
        MYSQL_ROOT_PASSWORD: yourpassword
        TZ: "Asia/Shanghai"
      volumes:
        - ./data:/var/lib/mysql
      ports:
        - "3306:3306"
  ```

- **运行服务**：
  将 `docker-compose.yml` 复制到服务器，运行以下命令：
  ```bash
  docker-compose up -d
  ```
  MySQL 服务会自动在容器中启动，并映射到宿主机的 3306 端口，数据会持久化到宿主机的 `./data` 目录。

### 5. **Tomcat 示例：**

#### **步骤**：
- **创建项目**：使用 Idea 创建一个基于 `java:21` 和 `tomcat:10` 的 Maven 项目，并生成 `.war` 文件。

- **编写 docker-compose.yml 文件**：
  ```yaml
  version: '3'
  services:
    tomcat:
      image: tomcat:10
      ports:
        - "8080:8080"
      volumes:
        - ./app.war:/usr/local/tomcat/webapps/ROOT.war
      environment:
        TZ: "Asia/Shanghai"
  ```

- **执行**：
  将 `.war` 文件和 `docker-compose.yml` 复制到服务器，运行：
  ```bash
  docker-compose up -d
  ```
  通过访问服务器的 `IP:8080` 来访问应用。

### 6. **MySQL + Tomcat 整合：**

在使用 `Docker Compose` 管理多个服务时，最重要的是网络配置。Docker Compose 默认为每个项目创建独立的网络，所有服务可以通过服务名称直接相互访问。

#### **步骤**：
- **编写 docker-compose.yml 文件**：
  ```yaml
  version: '3'
  services:
    mysql:
      image: mysql:8
      environment:
        MYSQL_ROOT_PASSWORD: yourpassword
      ports:
        - "3306:3306"
    tomcat:
      image: tomcat:10
      ports:
        - "8080:8080"
      volumes:
        - ./app.war:/usr/local/tomcat/webapps/ROOT.war
      environment:
        TZ: "Asia/Shanghai"
      depends_on:
        - mysql
  ```

- **运行服务**：
  MySQL 和 Tomcat 将在同一个网络下运行，Tomcat 应用可以通过 `mysql:3306` 访问 MySQL 服务。

### 7. **nginx/openjdk/mysql 的组合：**

#### **编写 docker-compose.yml 文件**：
```yaml
version: '3'
services:
  nginx:
    image: nginx:latest
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
  openjdk:
    image: openjdk:11
    volumes:
      - ./app.jar:/app.jar
    command: "java -jar /app.jar"
  mysql:
    image: mysql:8
    environment:
      MYSQL_ROOT_PASSWORD: yourpassword
    ports:
      - "3306:3306"
```
通过这种方式可以定义一个复杂的多容器应用，且各个服务通过 Docker 内部网络相互通信。

### 总结：
Docker Compose 简化了多服务应用的管理。无论是数据库、web 服务还是其他组件，Docker Compose 都能通过一个统一的文件集中管理，适合用于开发、测试和小规模部署场景。