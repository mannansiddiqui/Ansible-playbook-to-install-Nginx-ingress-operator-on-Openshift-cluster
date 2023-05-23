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

It's created.

![9](https://github.com/mannansiddiqui/Ansible-playbook-to-install-Nginx-ingress-operator-on-Openshift-cluster/assets/74168188/17225368-95d9-4ed9-ba07-c0eb343cfdfe)

Now, it's picked up also

![10](https://github.com/mannansiddiqui/Ansible-playbook-to-install-Nginx-ingress-operator-on-Openshift-cluster/assets/74168188/92e5fcd4-5dad-4649-ba6f-cb3e9a382b26)

#### Step-2: Create Openshift cluster

To create Openshift cluster you can use CRC i.e. [Code Ready Container](https://console.redhat.com/openshift/create/local) or you can use [Openshift payground](https://developers.redhat.com/courses/explore-openshift/openshift-playground)

I am using Openshift payground as it's free and quick to create but it's for 1 hour only. After 1 hour it will be destoyed but you can create again.

In Openshift plyground we get Terminal as well as Web Console.

![11](https://github.com/mannansiddiqui/Ansible-playbook-to-install-Nginx-ingress-operator-on-Openshift-cluster/assets/74168188/06e43792-27e0-4940-a21b-28e932cfdd7a)

It will ask for username and password to open web console enter **admin** as username and **admin** as password to login.

![12](https://github.com/mannansiddiqui/Ansible-playbook-to-install-Nginx-ingress-operator-on-Openshift-cluster/assets/74168188/716651fb-223b-4fca-8911-50d6abd92945)

![13](https://github.com/mannansiddiqui/Ansible-playbook-to-install-Nginx-ingress-operator-on-Openshift-cluster/assets/74168188/8c55d5ff-d48f-4329-a5a0-b6beadf5d4b4)

#### Step-3: Configure Ansible to run playbook on Openshift cluster

Create a inventory file i.e. ```/etc/ansible/hosts```

![14](https://github.com/mannansiddiqui/Ansible-playbook-to-install-Nginx-ingress-operator-on-Openshift-cluster/assets/74168188/adda1c5d-67ec-4243-8dd1-5c9246f15597)

We will run ansible playbook on same host on which ansible is installed that will interact with Openshift cluster as we can't ssh to instance running Openshit cluster. When we run ansible playbook on same host it will interact with Openshift cluster using OC command line tool.

#### Step-4: Write Ansible playbook

Firstly, we need to login inside openshift cluster. To login inside Openshift cluster, ansible has **k8s_auth** module or we can use **k8s** but if we use **k8s** module then everytime we need to pass **kubeconfig** file as parameter that we can get easily from Terminal provided by Openshift Playground.

Let's first use **k8s_auth** module.

![15](https://github.com/mannansiddiqui/Ansible-playbook-to-install-Nginx-ingress-operator-on-Openshift-cluster/assets/74168188/627fa465-1c00-4da0-a37a-ad57cbe2c65d)

```
---
- hosts: openshift
  vars:
    openshift_api_url: "https://api.crc.testing:6443" 
    openshift_username: "admin"
    openshift_password: "admin"
  tasks:

    - k8s_auth:
        host: "{{ openshift_api_url }}"
        username: "{{ openshift_username }}"
        password: "{{ openshift_password }}"
        validate_certs: no
      register: k8s_auth_results
```
Before running this Ansible playbook we need to configure local DNS that will point **api.crc.testing** to IP of Terminal instance provided by Openshift playground in which Openshift cluster is running.

To find public IP of Terminal instance run ```curl ifconfig.me```.

![16](https://github.com/mannansiddiqui/Ansible-playbook-to-install-Nginx-ingress-operator-on-Openshift-cluster/assets/74168188/f70b4ae2-077d-4f3b-8759-49778f158887)

Now, for local pointing edit **/etc/hosts** file.

![17](https://github.com/mannansiddiqui/Ansible-playbook-to-install-Nginx-ingress-operator-on-Openshift-cluster/assets/74168188/9870ca95-d737-4522-b0b6-caf46300709c)

![18](https://github.com/mannansiddiqui/Ansible-playbook-to-install-Nginx-ingress-operator-on-Openshift-cluster/assets/74168188/c156d24e-f25e-42cb-a560-3d24ff247c31)

save and exit by pressing **Esc** key and enter **:wq**

Now let's run ansible playbook ```ansible-playbook openshift.yml```.

![19](https://github.com/mannansiddiqui/Ansible-playbook-to-install-Nginx-ingress-operator-on-Openshift-cluster/assets/74168188/44236d9e-6b8f-47be-b5b3-9a10ae28c28c)

We got an dependency error. It's asking to install sshpass. Let's install sshpass in Ansible control node.

![20](https://github.com/mannansiddiqui/Ansible-playbook-to-install-Nginx-ingress-operator-on-Openshift-cluster/assets/74168188/3de96aa3-ef87-4708-9c99-7b612dfa6c6f)

Now, Let's try to run again.

![21](https://github.com/mannansiddiqui/Ansible-playbook-to-install-Nginx-ingress-operator-on-Openshift-cluster/assets/74168188/9ab23ef8-503f-4cf2-a95f-61fc48d8df9e)

Again got an error. It's saying that we are using SSH password instead of key and its not possible as Host key checking is enabled. So to diable host key checking we can add **ansible_ssh_common_args='-o StrictHostKeyChecking=no'**

![22](https://github.com/mannansiddiqui/Ansible-playbook-to-install-Nginx-ingress-operator-on-Openshift-cluster/assets/74168188/60917b42-d53c-48a4-ae1e-02e89bc9458e)

Now, Let's again try to run Ansible playbook.

Again there is dependency error. Asking to install **requests-oauthlib** python library. Let's add this in playbook.

![23](https://github.com/mannansiddiqui/Ansible-playbook-to-install-Nginx-ingress-operator-on-Openshift-cluster/assets/74168188/2e8a268e-e4b7-4c31-9dc3-ec442cc6e166)

Now, Let's again try to run Ansible playbook.

