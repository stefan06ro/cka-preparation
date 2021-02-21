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
