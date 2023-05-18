# Ansible-playbook-to-install-Nginx-ingress-operator-on-Openshift-cluster

#### Description:

  - Create a VM/Instance and install Ansible on it.
  - Create Openshift cluster
  - Configure Ansible

#### Step-1: Create a VM/Instance and install Ansible on it

Firstly, create a VM/Instance. I am using ACloudGuru PlayGround to create a VM.

![1](https://github.com/mannansiddiqui/Ansible-playbook-to-install-Nginx-ingress-operator-on-Openshift-cluster/assets/74168188/2cf49cb0-d429-47cf-9ca5-b48b31a8db2a)
![2](https://github.com/mannansiddiqui/Ansible-playbook-to-install-Nginx-ingress-operator-on-Openshift-cluster/assets/74168188/9965e6d3-6e80-4afe-84e9-ebc4a758f97b)

SSH to VM/Instance from terminal using:
```
ssh <USERNAME>@<PUBLIC_IP>
```
![3](https://github.com/mannansiddiqui/Ansible-playbook-to-install-Nginx-ingress-operator-on-Openshift-cluster/assets/74168188/338ea54a-7332-4238-bd42-61003c9e69f7)
![4](https://github.com/mannansiddiqui/Ansible-playbook-to-install-Nginx-ingress-operator-on-Openshift-cluster/assets/74168188/16a6256d-d9ac-409c-8849-063c5d1fc596)

It will prompt for password. Enter password and we are successfully logged in.

Install pip to install Ansible:
```
sudo dnf install -f python3-pip
```

![5](https://github.com/mannansiddiqui/Ansible-playbook-to-install-Nginx-ingress-operator-on-Openshift-cluster/assets/74168188/8b72afae-4802-430c-a343-cc36a50a121e)

Now, install Ansible:
```
pip3 install ansible
```

![6](https://github.com/mannansiddiqui/Ansible-playbook-to-install-Nginx-ingress-operator-on-Openshift-cluster/assets/74168188/e27471f3-b671-4285-9cb6-6270a650601f)

To check ansible is successfully installed or not run:
```
ansible --version
```
![7](https://github.com/mannansiddiqui/Ansible-playbook-to-install-Nginx-ingress-operator-on-Openshift-cluster/assets/74168188/2fdf5eb9-dd5a-4958-b104-c54054d6653b)

As you can see in above screenshot **config file = None** so we have to generate Ansible config file. When we install Ansible using pip3 Ansible config file is not generated.

First, create a ```/etc/ansible``` directory using ```sudo mkdir /etc/ansible```.

![8](https://github.com/mannansiddiqui/Ansible-playbook-to-install-Nginx-ingress-operator-on-Openshift-cluster/assets/74168188/0811428b-13b2-4c3f-b526-4c34c01f9cce)

Now, to generate Ansible config file run ```ansible-config init --disabled | sudo tee /etc/ansible/ansible.cfg```

