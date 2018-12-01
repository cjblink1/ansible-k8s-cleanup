# ansible-k8s-cleanup

ansible etcd -m shell -a "systemctl stop etcd.service" --become -K
ansible all -m shell -a 'sudo docker stop $(sudo docker ps -a -q)' --become -K
ansible all -m shell -a 'sudo docker kill $(sudo docker ps -a -q)' --become -K
ansible all -m shell -a 'sudo docker rm $(sudo docker ps -a -q)' --become -K
ansible all -m yum -a "name=docker-ce state=removed" --become -K
ansible etcd -m yum -a "name=etcd state=removed" --become -K
ansible masters -m yum -a "name=kubelet state=removed" --become -K
ansible workers -m yum -a "name=kubelet state=removed" --become -K
ansible all -m yum -a "name=glusterfs state=removed" --become -K
ansible all -m file -a "path=/etc/kubernetes state=absent"  --become -K
ansible all -m selinux -a "policy=targeted state=permissive" --become -K


