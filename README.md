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
