# Certified Kubernetes Administrator (CKA) 중요 개념 정리
## 1. Linux
1) Linux Command Line Essentials  
- pwd
- cd
- / vs ./ vs ../
- ~
- ls / ls -l (long) / ls -r (reverse) / ls -s (KB size)
- rm
- mkdir
2) Administrative privileges in terminal
- sudo (superuser do)
- !! (Do previous command)
- su (change to superuser)
3) Using the package manager
- apt-get install / remove / upgrade
- apt-cache search 
4) File permissions and ownerships
- -rw-r--r-- : Owner can read/write, Groups can only read, Public can only read.
- sudo chown (-R : Recursive) (change ownership) user:group
- sudo chmod (change permission) : 7 means readable & writeable & executable. 6 means readable & writeable, 4 means readable. 1 means executable. 
- You can also manage permission in this way : chmod ug+rw sample, chmod a-rwx sample (https://ko.wikipedia.org/wiki/Chmod)
5) Create/Move/Copy/Remove files using CLI
- touch
- rm [file] / rm -r [dir]
- cp [previous path] [next path]
- mv [previous path] [next path]
6) FIND command and its use (https://recipes4dev.tistory.com/156)
- find [path] -type f -name "*.php"
- -name is case-sensitive. If you use -iname, it becomes case-insesnsitive.
- -perm 0664 : Based on permission
- -size +100k
- -not
7) GREP command
- Find things in files.
- grep "[???]" [which file]
- grep -i : case-insensitive
- grep -n : gives you the line number of your string search
- You can use GREP with FIND command like this : find . -type f -iname "*.php" -exec grep -i -n "function" {} + | tee output.txt
8) TOP command
- top
- ps aux | grep [name of process]
- pgrep [name of process] : Getting PID
- kill -9 [PID]
- killall [name of process]
9) Services(Systemctl)
- sudo service [App name] start/stop/restart
- You can also use sudo systemctl start/stop/restart [App name]
10) CRONTABS to schedule tasks
- crontab -e : To save tasks.

## 2. YAML
1) What is YAML?
- YAML is not a markup language.
- YAML is a data serialization language.
- Common uses : Config files, Storing data
2) Syntax
- Spaces, not tabs
- Indent for structure
- Dashes(-) for lists
- Colons(:) for key-value
- Character support : Printable Unicode. UTF-32 mandatory for JSON compatibility.
3) YAML Styles
- Block styles & Flow styles
- Block styles : Better for humans, less compact
- Flow stlyes : Extension of JSON, "Folding" long lines of content, Tags and anchors
4) Mappings
- Associative arrays, hash tables, key-value pairs, collection
- Denoted with a colon and a space(: )
- No duplicate keys
- Mappings can be nested.
5) Sequences
- Lists, arrays, collections
- Denoted with a dash and a space (- )
- Sequences cannot be blank, nested with maaping.
6) Scalars
- String, numbers, boolean
- | (vertical bar) : multiple lines
- > : 여러 줄로 써도 한 줄로 합쳐줌.
7) Structure
- Multiple directives/documents in one file.
- Triple dashes(---) to mark the start of a file.
- Triple dots(...) to mark the end without closing the datastream.
8) Comments
- Define a comment with an octothorpe and whitespace (# )
- Blank lines function as commented lines.
9) Tags
- Used for setting a custom URI, setting local tags, setting a data type.
- Use the %TAG ! prefix.
- ! to set a local tag. ex) location: !PHL Philadelphia
- !! to define datatype. ex) cabinet: !!str 13 instead "13"
10) Anchors
- Store and reuse data
- Anchor names can be reused.
- Define anchor with &.
- Reference anchor data with *.

## 3. Docker
1) Why we use Docker?
- Conatinerize applications.
- Containers : Make them have own processes, network, mounts but share same OS Kernel.
- Containers vs VMs
- Container : HW -> OS -> Docker -> Container(Libs + Apps)
- VM : HW -> Hypervisor -> Virtual Machine(Each OS + Libs + Apps)
- VM is much heavier in Size, Utilization, Boot up time.
- VM은 Isolation 크므로 아예 다른 OS의 App 동시에 올릴 수 있음.
- VM과 Conatiner 동시에 쓸 수도 있음. HW -> Hypervisor -> VM(Each OS -> Docker -> Container)
2) Container vs Image
- Image : Things to create container
- Container : Created by Image
3) Docker commands
- docker ps : list containers, docker ps -a
- docker stop "NAMES"
- docker rm "NAMES"
- docker images
- docker rmi "Image"
- docker run "image" : Download & Run
- docker pull "image" : Only Download
- Container lives only as process inside is alive!
- docker run ubuntu하면 그래서 바로 container 죽는다. 안에 아무 process 없으니까.
- docker exec
- docker run -d : Background에서 돌게 만듬. / docker attach "ID 중 일부" 
4) RUN
- docker run redis:4.0 : This is TAG!
- TAG 설정 안 하면 latest.
- Input 넣고 싶으면 -it를 써서 interactive + terminal mode로 실행해야 함. ex) docker run -it kodekloud/simple-prompt-docker
- PORT mapping: docker run -p 80:5000 kodekoloud/simple-webapp
- Volume mapping: docker run -v /opt/datadir:/var/lib/mysql mysql 이렇게 해서 Data 옮김
- Inspect Container: docker inspect "container name"
- Container logs: docker logs "container name"
5) Environment variables
- ENV Variables in Docker: docker run -e APP_COLOR=blue simple-webapp-color
- Inspect Environment Variable: docker inspect "container name"
6) Images
- Dockerfile: Instruction / Argument
- Layered architecture
7) CMD vs ENTRYPOINT
- CMD는 정해진 그대로 실행됨.
- ENTRYPOINT는 실행될 명령어만 들고 있음. 실행 라인에서 받아들인 variable이랑 결합되어서 실행됨. ENTRYPOINT 밑에 CMD로 변수 넣어주면 Default값 됨.
8) Networking
- Default networks : bridge, host, none
- Inspect network: docker inspect "container name"
- Embedded DNS
9) Storage
- File system
- Cache 써서 중복된 Layered architecture 설치 x
- Image layer에 file 넣으면 read only. Container layer에 넣으면 Read/Write
- COPY-ON-WRITE : Image에 있는 것은 같지만 얘를 Container Layer로 복사해와서 Read & Write
- Volume mount vs Bind mount
- ex) docker run --mount type=bind, source=/data/mysql, target=/var/lib/mysql mysql
10) Compose
- With yaml file, we can create config file. Running multiple services.
- docker-compose up
- Why we make name on container? -> To link!
11) Registry
- docker login "private registry" before pulling private registry
- How to deploy private registry?: docker run -d -p 5000:5000 --name registry registry:2 / docker image tag my-image localhost:5000/my-image / docker push localhost:5000/my-image
12) Engine
- Docker Engine = Docker Deamon + REST API + Docker CLI. Docker CLI can be on different host(ex. laptop)
- Conatinerization : Namespace isolation
- cgroups

## 4. Kubernetes Basics
1) What is Kubernetes?
- Container orchestration tool
- Increased usage of containers due to Microservices -> proper way of managing containers
- Offers High availability, Scalability, Disaster recovery
2) Kubernetes Components
- 1. Node and Pod 
  - Node : Simple server
  - Pod : Smallest unit of K8s. Abstraction over container. You only interact with the Kubernetes layer. Usually 1 application per Pod. Each Pod gets its own IP. When Pod dies, new one get created and new IP address on re-creation.
- 2. Service and Ingress
  - Service : Permanent IP address. Lifecycle of Pod and Service Not connected. (Pod 죽어도 Service 그대로)
  - Ingress : Forward them to service. (DNS 처럼 동작)
- 3. ConfigMap and Secret
  - Database URL usually in the built application. RE-build -> push it to repo -> pull it to your pod. This is bad.
  - ConfigMap : External config of your application. Don't put credentials into ConfigMap.
  - Secret : Similar to ConfigMap. Used to store secret data. base64 encoded.
- 4. Volumes
  - If DB restarts, data is gone. So use Volumes.
  - It can be storage on local machine, or remote, outside of the K8s cluster.
  - K8s doesn't manage data persistance!
- 5. Deployment and stateful set
  - We replicate everything on multiple nodes and connected to Services(Permanent IP)
  - Deployment: Blueprints for pods. Abstraction of Pods.
  - But DB can't be replicated via Deployment! Avoiding Data incompetencies.
  - StatefulSet : for STATEFUL apps (주로 DB)
  - DB are often hosted outside of K8s cluster.
3) Kubernetes Architecture
- 1. Node process
  - each Node has multiple Pods on it.
  - 3 processes must be installed on every Node. (Container runtime, Kubelet, Kube proxy)
  - Worker Nodes do the actual work.
  - Kubelet interacts with both the container and node. Kubelet starts the pod with a container inside.
  - Kube Proxy, Communication via Services
- 2. Master processes
  - 4 processes run on every master node. (API server, Scheduler, Controller manager, etcd)
  - API server : cluster gateway. Acts as a gatekeeper for authentication.
  - Scheduler : Load balancing
  - Controller manager : Detects cluster state changes
  - etcd : Cluster brain. Key-value storing. Cluster changes get stored in the key value store. Application data is not stored in etcd!
4) Kubectl
- Create : We don't create pod directly. We usually create Deployment. replicaset is auto-created.

5) Namespace
- Virtual cluster inside cluster
- 4 namespaces for default
- kube-system / kube-public / kube-node-lease / default
- Why to use namespace? : Resources grouped in Namespaces. Many teams, same application. Resource sharing. 
- You can't access most resources from another Namespace.
- Components, which can't be created within a Namespace. (Live globally in a cluster)

6) Kubernetes Ingress
- External request -> Ingress -> Internal Service -> Pod
- Ingress controller : evaluate all rules, manage redirections, entrypoint to cluster

7) Helm
- Package manager for K8s. (To package yaml file)
- Helm charts : Bundle of YAML files.
- Templating engine of similar microservices.

8) Volumes
- Persistent volume

9) StatefulSet
- Tracks state. (cf) stateless : do not keep record of state. each request is completely new
- stateless app : deployed using Deployment. stateful app : deployed using StatefulSet
- Stateful app has Pod Identity. Difficult to deploy. Same specification but not interchangeable.
- Pod identity : Deployment는 random hash, StatefulSet는 정해진 순서대로.
- Container is not good for stateful apps.

10) Services
- Stable IP address, loadbalancing
- ClusterIP Services : default type. 
- Headless Services : Pods want to talk directly with specific Pod
- NodePort Services
- LoadBalancer Services
