
## Ansible Configuration Management - Automate Project 7 to 10
===========================================================

Ansible is a modern configuration management tool that facilitates the task of setting up and maintaining remote servers, with a minimalist design intended to get users up and running quickly. Ansible uses an inventory file to keep track of which hosts are part of your infrastructure, and how to reach them for running commands and playbooks.

Most of the routine tasks are automated with [Ansible](https://en.wikipedia.org/wiki/Ansible_(software))  [Configuration Management](https://www.redhat.com/en/topics/automation/what-is-configuration-management#:~:text=Configuration%20management%20is%20a%20process,in%20a%20desired%2C%20consistent%20state.&text=Managing%20IT%20system%20configurations%20involves,building%20and%20maintaining%20those%20systems.). It also involves writing code using declarative language such as [YAML](https://en.wikipedia.org/wiki/YAML).

Ansible Client as a Jump Server (Bastion Host)
----------------------------------------------

A [Jump Server](https://en.wikipedia.org/wiki/Jump_server) (sometimes also referred as [Bastion Host](https://en.wikipedia.org/wiki/Bastion_host)) is an intermediary server through which access to the internal network can be provided. If you think about the current architecture you are working on, ideally, the webservers would be inside a secured network which cannot be reached directly from the Internet. That means, even DevOps engineers cannot SSH into the Web servers directly and can only access it through a Jump Server - it provide better security and reduces [attack surface](https://en.wikipedia.org/wiki/Attack_surface).

On the diagram below the Virtual Private Network (VPC) is divided into [two subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html) - Public subnet has public IP addresses and Private subnet is only reachable by private IP addresses.

![](https://lh4.googleusercontent.com/3REC4PSRHfmjcednqTHRAGYIu_m7y_VG_AP5pedtEPjarwEGBTkwCFD2RU7NbaruQKQhnUuCwOS5vFbwWIzlanCC6DHhAzJYUi3Z8YDf5cEM3QCkqQbIcj7ExPndELmlB2qf82wJ)

When you reach [Project 15](https://dareyio-pbl-expert.readthedocs-hosted.com/en/latest/project15.html), you will see a Bastion host in proper action. But for now, we will develop Ansible scripts to simulate the use of a Jump box/Bastion host to access our Web Servers.

Task
----

-   Install and configure Ansible client to act as a Jump Server/Bastion Host

-   Create a simple Ansible playbook to automate servers configuration

Step 1 - Install and configure Ansible on EC2 Instance
------------------------------------------------------

1.  Update Name tag on your Jenkins EC2 Instance to Jenkins-Ansible. We will use this server to run playbooks.

1.  In your GitHub account create a new repository and name it ansible-config-mgt.

1.  Install Ansible

sudo apt update

sudo apt install ansible

Check your Ansible version by running ansible --version

![](https://lh6.googleusercontent.com/dJeXb-HX8B6nr79HKosy1RUCKihWLOb-uNUIwJDXb1js1e-3glIFmU1b1bxuVXG8uLgTzvUMq8ZimyQfbsiSVP6MaC0-fl7VAYtAb6QVDIoEFVSKZA3NYRUmRFYtLw9m4xlzFdV8)

## Configure Jenkins build job to save your repository content every time you change it - this will solidify your Jenkins configuration skills acquired in [Project 9](https://professional-pbl.darey.io/en/latest/project9.html).

-   Create a new Freestyle project ansible in Jenkins and point it to your 'ansible-config-mgt' repository.

![](https://lh5.googleusercontent.com/HmDMpYJx_FCd1zQc3-dRj2TMKdxqop4WYY5VoE2roUmKu6UiW22m39pdvaM5umpB5YC4mvNQ7aPzt0ypmZIlWnVk1i-dWs7r-t2BzzIL2KcbyUs8mBr7WwdZLI4HhcKZuSVIA3t8)

![](https://lh6.googleusercontent.com/zDGnRbrIHmj7xoD3E3Cp_9iK8ayqsI09YCjq02xpeWIvjJE17OshveuIm4pjE8SjhQ81bwJiEjCSNgxFwSio7SHYE5pDiwMnwWnkmLvk7bgIudrUuwrUxsbid38D8mjXNhWfid7G)

![](https://lh6.googleusercontent.com/eqaLYIB_1Yo-SxM_YUcuVos86TRzW6y-m4lrbgsZvB_4PsVgSYEo8Bcb_PFuESacieyQ_289AJ_QjRCFrr1hFt7kYt6FluVoLuP7UAhP0hSWd-96oszSA05upORFo3p0gXD2F5PG)

![](https://lh6.googleusercontent.com/wW_XYVdENywh9ZKUy_YfV7iA1lgh_dl01tUXLDQv2ghIXql3NZxJrwabnnC4ZHnn7O0_2d0espCxWvZnQS-XespXoF5IpQAVlrnKvhmFHwPfMRFkuALaqRqsBHCNw6RheZ-M1hAP)

-   Configure Webhook in GitHub and set webhook to trigger ansible build.

## Enable webhooks in GitHub repository settings.

-   Go into the tooling github repository:

https://github.com/BimsDevs/tooling

-   Go to webhook in settings and add webhook:

-   Go to the Payload box and input the URL: http://jenkins_server_public_ip _address.github-webhook/

http://35.178.239.175:8080/github-webhook/

![](https://lh4.googleusercontent.com/_aEx5XTdAhXjpj7jPTP_7rc9oNJZPMAIpKIFH6N1DBHllV7i-BkprX0xhABGpV0vDSmdBX7tlvlbC3zHd3KlFL_27TepUFAm99aSTvrpuqhefpwvzPFeE67uHA_iRkyJ3TPGVVFN)

-   Configure a Post-build job to save all (**) files, like you did it in Project 9.

![](https://lh5.googleusercontent.com/rqdkat6q1dqnS7Ge8Nvl1MXJANF9DNWh_eK8kkOWwz_OiTs4a3fWIVrteXRB5z-YzLV6lsOUN0UNGBEuP9mA8m9gmx4EZd13UhpixGWXKd1C4JLhkQCQ5AWJMKVkyDy2AATvTE6I)

## Blocker: 

  Unable to successfully build the job.

  Solution:  Updated Jenkins URL with the new public IP address under Jenkins 

              Location. Also ensured that the file in Github was under the Master's record.

![](https://lh4.googleusercontent.com/CbdFYm83lOpqWPxWxoPgZLDCxLAwP_Wu92QZvp2wol0ooQ1PKOHrujs1-EhLBHulWjsn6scW_CPBeRwEwjVRZGm7QLgxBUSkFJuPIlHXbJNpJN6cbULG4P5n95Ia5QiXWGLOJsuE)

Test your setup by making some change in README.MD file in master branch and make sure that builds starts automatically and Jenkins saves the files (build artifacts) in following folder

ls /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/

![](https://lh6.googleusercontent.com/LFwdxGqmb0BGo_9s9tG0eqvNWvsJIHieyP07sgPch48Y1g4WNhWE9UBKHMzdfljcm1KefpnRO60VKuEQLQ_2VT48GiUmxBUedrwR_mFSxAloRsW4USiEPTjIwo4j-LFoyinaoKd1)

![](https://lh4.googleusercontent.com/z980L7amftSNs-Fx58fZr-k4o4hEIpuBaEYWonNwWnKz1be8OerrTzip2Of-jEJPwHcuXLOz_upABo0j505SQofBEkUIwTLMEZl_vAkC0Np8AkFftUgAW_uuGNAuKcgE8P0MIjmv)

![](https://lh5.googleusercontent.com/P-DstTulzTPHnNh21nWsKPanSyYtv7elqK1zZAGVu4KVZDjqXUWoFUgrpw3GaY5ukO5gZae3iY-JjW1AQLiEqP4wDx936fzfoh7j45Yvg7RFvH5hE5kqfn8AeuL-COieLD-ir5RN)

Note: Trigger Jenkins project execution only for /main (master) branch.

Now your setup will look like this:

![](https://lh3.googleusercontent.com/7o72Si-1TxPBML4HRCk6np_5PtUyd3fozXd3Gj8qSK0oPrfwjOcqwgyg2qZDy6KpPjHSc0AtVcnF5eVUc773i_2I8YXx4t3fxg8DGVWIWftoT1MgsCns-KAjTpNlP3W6ghV6yVIV)

## Allocate Elastic IP address to Jenkins - Ansible server

Tip Every time you stop/start your Jenkins-Ansible server - you have to reconfigure GitHub webhook to a new IP address, in order to avoid it, it makes sense to allocate an [Elastic IP](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html) to your Jenkins-Ansible server (you have done it before to your LB server in [Project 10](https://professional-pbl.darey.io/en/latest/project10.html)). Note that Elastic IP is free only when it is being allocated to an EC2 Instance, so do not forget to release Elastic IP once you terminate your EC2 Instance.

Steps:

1.  Open the Amazon EC2 console at <https://console.aws.amazon.com/ec2/>.

2.  In the navigation pane, choose Network & Security, Elastic IPs.

3.  Choose Allocate Elastic IP address.

4.  I have chosen Amazon's pool of IPv4 addresses, since the IPv4 address is to be allocated from Amazon's pool of IPv4 addresses.

5.  (Optional) Add or remove a tag.

6.  Choose Allocate.

7.  Click Allocate new address in the Elastic IPs page.

8.  Click on the row of the newly created elastic IP, select Actions and click on Associate elastic address.

9.  Choose the EC2 instance you are integrating.

10. Click on Associate

![](https://lh3.googleusercontent.com/9S8kiqfcvT78arOAa7Kk9YUxVMOBTSXbt0mGbDS_76Hj2lLxOklyvgJpkQWb8dD-uyduqLJMr_dWLAUv3idouGbk3jBpqrScJILvFraGiyDple9Xu9xNzvihHwGnxgE1YzNvvbR9)

# Elastic IP address: [18.132.219.41](https://eu-west-2.console.aws.amazon.com/ec2/v2/home?region=eu-west-2#ElasticIpDetails:AllocationId=eipalloc-047271799d471a966)

Step 2 - Prepare the development environment using Visual Studio Code
---------------------------------------------------------------------

Downloaded Visual Studio code as the [Integrated development environment (IDE)](https://en.wikipedia.org/wiki/Integrated_development_environment) or [Source-code Editor](https://en.wikipedia.org/wiki/Source-code_editor) that can be used to write some codes. Configured it to connect to Github with the steps below:

Steps to link Visual Studio Code to Github (https://code.visualstudio.com/docs/editor/github)

-   Open the remote window

-   On the bar space, select remote SSH - Open configuration file

-   Select file path to configuration file

-   Configure the host, host name, user and identity file

-   Ctrl+S to save. Then launch new terminal

![](https://lh3.googleusercontent.com/KJcVdzBteaSCqLGyvyVnF6FevbWeE8kkVfjI1W14TH6U4h9fOL9wD6OFHEM_AHO_fKutijBmn2Y2lnq7WEWaR-rSqZrq_q5Ajlbjg93dqiRsbcdHg0i9Q92a-7ppZsOMUONK2lPs)

![](https://lh5.googleusercontent.com/ZqS4-MoCBs9Ppx13EYPSO6LyHakovfjvdIw-kYXKo7YVbg9mEID5f489FkEx80tH4zFnadUwKTbGCB5KHewU5g0xR-veuu4lMOdfcwZn_Dc3s7ILVREAVGDS9pu8l98TcUxrdwT9)

![](https://lh6.googleusercontent.com/nDRJIcVDA0VBrym7lsb0SJd_TudJIBVBihKSxw40eyYXAJZKFjiIkaKsXWT8JJiJuXh7rOlq5JlR3KP5PKjlwdEO5zFigFxraAMiRq-mwUthtrgvWBU7FGQaQZ4__OAPIDpbpNM9)

Start by connecting the remote servers to the Ansible-Jenkins server:

To get started with ansible, you must configure the Ansible-Jenkins master controller to connect to your remote host servers, for best practice and security this is achieved using ssh.

All remote host servers would be stored in an inventory file with the below structure in different environments in this instance. 

Note: Ansible uses TCP port 22 by default, which means it needs to ssh into target remote/slave node servers from the Jenkins-Ansible host.

Two ways are:

1.  Copy your private (.pem) key and provide a path to it so Ansible could use it to connect. Do not forget to change permissions to your private key chmod 400 key.pem, otherwise EC2 will not accept the key.

2.  Generate a new ssh key, then print the output of the public key by running the below command and copy the public key of the Ansible-Jenkins into the authorization key file of each of the remote servers

Ansible-Jenkins Server:

sudo ssh-keygen

sudo cat .ssh/id_rsa.pub

or

cd .ssh

sudo cat id_rsa.pub

Copy the public authorisation key file and paste in the authorisation key file of the connecting servers.

![](https://lh6.googleusercontent.com/ZbTN5SitBprcZqreWExlHFeiem1H8NPmJwiXLVoNTV7-NNNMz_qCgXSL_AvO4VEFtp4gd8A6IpPH7vZXc3R5b4Yu70iy7H0-K9oCBwSWaU08VVO2dyrWgkjlis92WLjJCGNSSUWx)

![](https://lh6.googleusercontent.com/CD-vlhxm1Fbc15JUw3CpsEtWyvwfzggwuux5VDjV_ZTq8XMw2pbRiewuE-eqtWKlhf5m1vcJj9KUPuYoydMp4YkvTOYgE7n6t6_k14k-8n3Lrn5V53wbBrRPvIYhgMz17_5lGBil)

Connecting server and repeat for the other servers :  sudo vi .ssh/authorized_key

![](https://lh3.googleusercontent.com/XcadKcsa8PNRjbzeMEaIn0CTF8FrAmUBdYqGhQb2E6_wk1yKKMa8dWL6_Ihfvk40bhJJmwVi_jKxlYPoc5jrZr70Fp4tficMhVSB_YfVElr_gQtnZ3eSTWOcKIER-ouSfy69guzF)

Step 3 - Begin Ansible Development
----------------------------------

1.  In your ansible-config-mgt GitHub repository, a new branch was created. This will be used for the development of a new feature. (ansible-5511-feature01)

![](https://lh5.googleusercontent.com/zxNzHWWzeh32iE93YIcz8JywBUwCisUOXcezrpftomGhCSD8ushEniFqusDQk3yyHctfg22QuRnLHAIPyo9o81QwXkpq-P1LdQNAtVT1m6rbPu5hrs0L6aqDgu7FB9H80tYxOoq-)

 In the VS Code Integrated Terminal:

1.  Create a folder and name it ansible: mkdir ansible

2.  cd  into ansible: cd ansible

3.  Clone your repository into the server: 

git clone <https://github.com/BimsDevs/ansible-config-mgt.git>

![](https://lh5.googleusercontent.com/v1BSCW8X0T7PXmxhyf9OZidwxE0tAdkur2V3Nx77bRUEu5TSwXVIuAh9_KbPsVf45Xmf7G-DF7jMaxodX3OSVL62hfqckoKFrMglcvvpXtukltMYVTFsDfKzeOX06toqBAdtZ6ij)

4.  Checkout the newly created feature branch to your local machine and start building your code and directory structure

git checkout ansible-5511-feature01

![](https://lh4.googleusercontent.com/BPsMa72XRjqcKsj3msl2W3E-k9oW2idoQp32FAutBLdIvDG1he80LJu8kRj62ZVplriQfaysfnCxyMKR2NJ4vAGnzdvogqP2iCd1lN-14EnMSdVIWam3lOZN6KlzAeFt5ofJQhPf)

5.  Create a directory and name it playbooks - it will be used to store all the playbook files.

mkdir playbooks

6.  Within the playbooks folder, create your first playbook, and name it common.yml

cd playbooks

touch common.yml

7.  Create a directory and name it inventory - it will be used to keep the hosts organised.

mkdir inventory

8.  Within the inventory folder, create an inventory file (.yml) for each environment (Development, Staging  Testing and Production) dev, staging, uat, and prod respectively.

cd inventory

touch dev.yml staging.yml uat.yml prod.yml

![](https://lh5.googleusercontent.com/BExGmCCTl2MH5wfnP7oWmQzaIC2O9a1jOUVZtwO-iU4yZkFR6_dRJ9KYEFJVmYRuz0Xy1T-ilN0EYVSSqtdt55nnuwRSPCBQFOtM5x4dxju_CFHdFi23d9-ERyiepjrm-UTq5yWV)

### Step 4 - Set up an Ansible Inventory

An Ansible inventory file defines the hosts and groups of hosts upon which commands, modules, and tasks in a playbook operate. Since our intention is to execute Linux commands on remote hosts, and ensure that it is the intended configuration on a particular server that occurs. It is important to have a way to organize our hosts in such an Inventory.

Save below inventory structure in the inventory/dev file to start configuring your development servers. Ensure to replace the IP addresses according to your own setup.

Note: Ansible uses TCP port 22 by default, which means it needs to ssh into target servers from Jenkins-Ansible host - for this you need to copy your private (.pem) key and provide a path to it so Ansible could use it to connect. Do not forget to change permissions to your private key chmod 400 key.pem, otherwise EC2 will not accept the key. Also notice, that your Load Balancer user is ubuntu and user for RHEL-based servers is ec2-user.

## IP Addresses (private)

NFS : 172.31.7.178

Webserver 1: 172.31.16.162

Webserver 2: 172.31.16.41

Database: 172.31.18.63

Load balancer: 172.31.46.227

## Update the inventory/dev.yml file with this snippet of code and save (ctrl+s):

[nfs]

<NFS-Server-Private-IP-Address> ansible_ssh_user='ec2-user' ansible_ssh_private_key_file=<path-to-.pem-private-key>

[webservers]

<Web-Server1-Private-IP-Address> ansible_ssh_user='ec2-user' ansible_ssh_private_key_file=<path-to-.pem-private-key>

<Web-Server2-Private-IP-Address> ansible_ssh_user='ec2-user' ansible_ssh_private_key_file=<path-to-.pem-private-key>

[db]

<Database-Private-IP-Address> ansible_ssh_user='ec2-user' ansible_ssh_private_key_file=<path-to-.pem-private-key>

[lb]

<Load-Balancer-Private-IP-Address> ansible_ssh_user='ubuntu' ansible_ssh_private_key_file=<path-to-.pem-private-key>

## Updated version

[nfs]

172.31.7.178 ansible_ssh_user='ec2-user' 

[webservers]

172.31.16.162 ansible_ssh_user='ec2-user' 

172.31.16.41 ansible_ssh_user='ec2-user' 

[db]

172.31.18.63 ansible_ssh_user='ubuntu' 

[lb]

172.31.46.227 ansible_ssh_user='ubuntu'

## Output

![](https://lh4.googleusercontent.com/R5I_6f3cmKg_F8MW28eaENhD_90NwzOOWY6cclOT48vwLMm3Xa_egRnBnD8sum2Ejryt7O0QKNfwkRiw4lIwS-91x_KJS11RllJywrGEXQCdnUfNLkl38bAgL-uXl41j09d7ywLE)

![](https://lh3.googleusercontent.com/i59-RsAhPDS6c07Nq4jHb2hB7_n9jGqKxquicjeyzoQ-h89iRuYhlkOa6q5et2QuREeTngpsfxMmvnq947BRUgOUZIpbFXgg19iqR0CjzPP6mTQjYr1EK8D4iE0lfXy6yujG-JtJ)

### Step 5 - Create a Common Playbook

It is time to start giving Ansible the instructions on what needs to be performed on all servers listed in inventory/dev.

In the common.yml playbook you will write configuration for repeatable, re-usable, and multi-machine tasks that is common to systems within the infrastructure.

## Update your playbooks/common.yml file with following code:

---

- name: update web, nfs and db servers

  hosts: webservers, nfs, db

  remote_user: ec2-user

  become: yes

  become_user: root

  tasks:

  - name: ensure wireshark is at the latest version

    yum:

      name: wireshark

      state: latest

- name: update LB server

  hosts: lb

  remote_user: ubuntu

  become: yes

  become_user: root

  tasks:

  - name: ensure wireshark is at the latest version

    apt:

      name: wireshark

      state: latest

![](https://lh3.googleusercontent.com/IEbgo_uu2odWl2WLDXdlMryZW4ioFZ1Pa7tikcDIkAJi_8PCCF2sQglg6izSMaqecyrN-_90LeG5FYltkws6iJ2hul4AIFrdAg7Lk_J9nJqoJ4FcfrvtdpRLeXFN629GIspiMNTI)

![](https://lh5.googleusercontent.com/WYJGyYeOjf9PtXsy7P7ABoCiXylBzR2K5XB4OQqfGyMv3C5FIHZqM8GWaUyPMCtHck5KBU_b4c8yyX9nAC6_xwOkjpiuGA-QWE_aFfb8m_vqBQmc3vIWGvxTohH4UG59yHewjsR0)

Examine the code above and try to make sense out of it. This playbook is divided into two parts, each of them is intended to perform the same task: install [wireshark](https://en.wikipedia.org/wiki/Wireshark) utility (or make sure it is updated to the latest version) on your RHEL 8 and Ubuntu servers. It uses root user to perform this task and respective package manager: yum for RHEL 8 and apt for Ubuntu.

Run ansible: 

ansible-playbook -i inventory/dev.yml playbooks/common.yml

![](https://lh6.googleusercontent.com/S8_aieb59N8uuHL_pbU9Tuyzj1Om_2lX2s-7ZIuyQuao1jt72AEm0wUFvaSrO4BT_0JWPSxw7OADu2Vo_Jldqy3UhavYUQsdy1siK4Nsx8cfbIJ0aGfl0uwTlTFohN0uilfvIHkk)

Feel free to update this playbook with following tasks:

-   Create a directory and a file inside it

-   Change timezone on all servers
-   
## 
name: Set timezone to Asia/Tokyo

Hosts: ubuntu

  timezone:

    name: Asia/Tokyo

-   Run shell Script etc. ..

- hosts: all

  tasks:

  - name: Creating a new directory and set time zone with a simple script

    file:

      path: "/your path"  

      state: directory
      
      
 ## Updated version:
 

- name: Creating a directory and set time zone with a simple script 

  hosts: all 

  become: true 

  tasks:

  - name: Creating a directory 

    file: 

      path: /playbooks/new-folder  

      state: directory

-  name: Creating a new file inside the new directory

   file:

     path: /playbooks/new-folder/new-file

     state: touch

- name: Set timezone to Europe/London

   timezone: 

    name: Europe/London

- name: Status 

  command: echo "All tasks completed"

![](https://lh3.googleusercontent.com/ui6qH2NrlpChPNiXk7-z7u7xCD1RfvGAn8M-r6dEXExtqQYp8bWVOknDVrTS0T2MzpdMltatcpZVyx38qyz2sJeZojLqU05CzsOM2B5XEXj6wdYO2etvio3RdJNhF1E2nBZA7BiQ)

![](https://lh5.googleusercontent.com/i4TjAJj6pgMaBoQu6kzTfi9oZ--Q6VpcQSVcV9Q279ZZ_3_m4ZyU8mll5JO5GDIRVilugGHdbC_mBj2bS9mwkvGjTqd3J_x5t3XMVPQ21SeTyoc4luV_4cQNEmHezpUOrmXc9rwv)

Overall Outputs:

![](https://lh4.googleusercontent.com/OeEsObl5wR0tNJ0m6dZeEhtlA5KhX1MJjbECE5_V38JZD28ZzbezwS5R9jbKy7jWJUSl3yLQVtSX_efyLYy44vcGQkjqQz6q7c24GLomt-Ft1dQRPlecwiijId-3LfsD3MEqxc5q)

![](https://lh4.googleusercontent.com/HThdfOBiyV32VQneJrauD6yvJLbW26xTt5uUg8rd1ZoLrNF3X7ArURwoBO3r5nceV77ZjjD9yHUg0dOoH30V7rBRPCJre2_5_TtZc3l_n89bLWpSwrfeEHH2FC39qx1hE6ii5JUc)

![](https://lh6.googleusercontent.com/k-hO1WSUnZA0zLkKRLw3A-gQcbYrkm0cpTEKgy0XHT-A211_GPZfgpd3InC4otzU3kTCk8hfl9Dgzk2kyqnS5hSgZuNHBROi-P5edVk1zsRw_YXem_rcDnf5vyDowzVSlHjsvagj)

![](https://lh4.googleusercontent.com/Gp6-1QoNEgjGeb1GCoreody3CpYgqjXypwWoqt15pXKxl28XWTwO9qFfxyXsJ1p9zb1l12TQ1XF2W2AmQc4_90qiGbwJ094cr9A2JOdtpxBVoYD31v_RxEIGupoyRJx3LueLjdSo)

![](https://lh5.googleusercontent.com/8y74QD_DwSAHJRl90ZyeBGE0fh3GYv3qtTnrJ70ARB75XeAekzQKTRydly85gJSvn4IvdyZIL7Xr153orrn9INEac0KvEwtiVsprUzt1MeFH4RMbdKIOfhgG9Qzvtdrn6zW2CDpq)

For a better understanding of Ansible playbooks - [watch this video from RedHat](https://youtu.be/ZAdJ7CdN7DY) and read [this article](https://www.redhat.com/en/topics/automation/what-is-an-ansible-playbook).


### Step 6 - Update GIT with the latest code

Now all of your directories and files live on your machine and you need to push changes made locally to GitHub.

In the real world, you will be working within a team of other DevOps engineers and developers. It is important to learn how to collaborate with the help of GIT. In many organisations there is a development rule that do not allow you to deploy any code before it has been reviewed by an extra pair of eyes - it is also called "Four eyes principle".

Now you have a separate branch, you will need to know how to raise a [Pull Request (PR)](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests), get your branch peer reviewed and merged to the master branch.

Commit your code into GitHub:

# Use git commands to add, commit and push your branch to GitHub.

git status

git add <selected files>

           git add inventory/

           git add playbooks/

Or 'git add . ' to add all files / changes

git commit -m "commit message"

git commit -m "Updated playbooks with new timezone and created new directory and a file inside it"

![](https://lh4.googleusercontent.com/dav1_5Rfs6YVjMBe7ruyypyJ6rWj52CbUYkcOjC-ghIdWFB5To59asq9S-JPq_M1VYzXwBZHQw0lBIpWHyBANBPV2-MT2mcxGCC-I85HGQIFNOYDwjnmEn61mCKLRN-C9GYSirnI)

1.  Create a Pull request (PR) from terminal to github repository

git status

git push

    ![](https://lh6.googleusercontent.com/D3OMw82kSzqiNBZ2ity7ULNna77GSktvty1Atp2pdd2DBm4wC_g0lGP3v7EtUyHx7OvDR00qd_1aNDMkACTdpjEJxUYz2J1klXXxJzy-GnmqmCJKjVg31tBAMSFzzEeUcHUP4FFW)

. Wear a hat of another developer for a second, and act as a reviewer.

. If the reviewer is happy with your new feature development, merge the code to the master branch.

# Steps:

Go into the branch which is to be merged to the master in github

Click on Compare and Pull request tab (green)

Click on Merge Pull Request 

Confirm Merge

![](https://lh4.googleusercontent.com/Six1rEDoErLjzKpOSI59TKH4NGmGARXZI1GJkK91TuQ3fPO0mM0baz5-CPgc_oyWDpnoB1X-YEOhU09CvKWgLipJzLJpTYCmBSF5fDGBTW2opKYE_CqDiNYrhO0eeFRkEwRD36TA)

![](https://lh6.googleusercontent.com/XyCql-m8HzTdiRPR0kaB0dFyKBxII7iJRelLFX1DVSpKEaSpWl6XBT3xf6MqJzkkxVaXNhneauCRu2C1OcaHwuEtdn8fsLTGhpC16gyUyaR1HNRRs44fEV_9npO_ByCxXzpHoGLo)

![](https://lh5.googleusercontent.com/ivSYBPO9BjUio7wTj_1J-YQxxT5KLqugDAx0cEhz15r15nLvXaVLFq323YWaCnaxnJPgpaha-Ko4e142ELiHy_sQQh2sTV8moMBD7FR1TB3h-NfVPhTeOcAgspuSZwt6Sfs7QLLU)

![](https://lh6.googleusercontent.com/o9hmS2XnrElDHO_anjFjCefvhPOVqPtg9zDvQ-xJ04i92_Bg0yo2Y54-hp6ZrAS37477lme2swxe0Vq2ri13XypncXKWO9NEUBT8tbeAahn8Gcno861s2YB7gOhIF3Fz_bzk2CQK)

1.  Head back on your terminal, checkout from the feature branch into the master, and pull down the latest changes.

git checkout master

git pull

![](https://lh6.googleusercontent.com/Oca-3baxoVPdFLeFJUcPCa_A1SCIBh9s9X2eqemA8YlipHfy1kavcBIVoTjptMl425xgiuli1Po6UWK8JAouUHEHgJ7zRMBQgzu9VYYvsVOjBTRbGAz8y9TGfHRUjz1mQL-5giVS)

Once your code changes appear in master branch - Jenkins will do its job and save all the files (build artifacts) to /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/ directory on Jenkins-Ansible server.

![](https://lh3.googleusercontent.com/SEEUJL4aQUZigEkXX4i0YDp-APx2_TPMOAPTH4GtVvFq96jaaBU6oTlphX3i4BCS4L3cPvDJC61gba_VhOou3xz1X1-vmg5oDcj4AN89Q-9oCWhaCYnIC9AaToqq5pIQxvDZ38z7)

Step 7 - Run first Ansible test
-------------------------------

Now, it is time to execute ansible-playbook command and verify if your playbook actually works:

sudo ansible-playbook -i /var/lib/jenkins/jobs/ansible/builds/<build-number>/archive/inventory/dev.yml /var/lib/jenkins/jobs/ansible/builds/<build-number>/archive/playbooks/common.yml

sudo ansible-playbook -i /var/lib/jenkins/jobs/ansible/builds/11/archive/inventory/dev.yml /var/lib/jenkins/jobs/ansible/builds/11/archive/playbooks/common.yml

Output

![](https://lh6.googleusercontent.com/z8OiT5WJ1qkPUOjH6fYHaiDQ4z3R1lgyB-WJLiPhb-nHrOhAhk4kcxOAHv3qT0ISAvTzS-HOEUb1gBHYSyOVPKzJsb0522Xqb9kyKIs5D71eB5SaA9kdiM4to8ERWIcD9UFOqZ1Z)

![](https://lh4.googleusercontent.com/U1S8A8wLuB2UoBNSuy6wHPIimy_1tD734QS1QHPag3JY-y7nXz6yiFVX8kX3YFX2azhEch-2BnRglbI7p1F-ISNQvVDnwkwO5eXhx51Wr-ySpwXY19-XXTNLiAkn003NfNJ2gVHa)

## Blocker:

The below ansible playbook syntax was run but gave an error.

sudo ansible-playbook -i /var/lib/jenkins/jobs/ansible/builds/11/archive/inventory/dev.yml /var/lib/jenkins/jobs/ansible/builds/11/archive/playbooks/common.yml

Solution: 'sudo' was removed and it worked as per above output.

ansible-playbook -i /var/lib/jenkins/jobs/ansible/builds/11/archive/inventory/dev.yml /var/lib/jenkins/jobs/ansible/builds/11/archive/playbooks/common.yml

![](https://lh6.googleusercontent.com/RaFc-YYje6h5LvpvDL1XI01kDnRKuZHV47XU2zaNNmV9vFrApwhHVmagFhJALka6G5-qEQeKIG9Bw6Ats1EvC6mFjetEeVTSCQTsuZisApaJH37259oCwkps8CueY0yKvHvoRItK)

You can go to each of the servers and check if wireshark has been installed by running which wireshark or wireshark --version

![](https://lh3.googleusercontent.com/iEnvSQw7XaQbqbiMjhOvEbLInL8_T6pcf6xPrxPVK0lHipguwPpp3Gfsj8q3GhYWwtMSGg6ZdIb_B5prNaE5mQn4U_ukPyzSemUE7m8ylY0tJxQtG6nCRWmuJ2aNmUoLQHwO6xhx)

Your updated with Ansible architecture now looks like this:

![](https://lh4.googleusercontent.com/TeboZuUY1l7MqmVgmH99vgAPSjlvhi3ro4EYsoNFaOBNnCe1713QeDo0xqP_53hYdBKk23HYJkI1w9sAPiNzClHAc9H1iFse0uUn9yWO7tpklvNxJKdSZPO8x82g3gbKCeNMZ-uc)

Repeat once again
-----------------

Update your ansible playbook with some new Ansible tasks and go through the full checkout -> change codes -> commit -> PR -> merge -> build -> ansible-playbook cycle again to see how easily you can manage a servers fleet of any size with just one command!

-   Checkout into branch: git checkout ansible-5511-feature01

-   Create a new file: touch test

-   Push to github: 

-   Add random comments into the test file in github

-   Commit change (github)

-   Compare and Pull request (github)

-   Merge and confirm merge

-   Ensure that the (elastic) IP address in the webhook is correct.

-   Confirm new build and changes are success in Jenkins (Jenkins)

-   Execute ansible:

ansible-playbook -i /var/lib/jenkins/jobs/ansible/builds/11/archive/inventory/dev.yml /var/lib/jenkins/jobs/ansible/builds/11/archive/playbooks/common.yml  

-   Checkout into branch: git checkout ansible-5511-feature01

Created a new file: touch test

![](https://lh4.googleusercontent.com/DZSq3veV2DeBIVWjBvzzeqyMqCp5IVGnZ4_9qI7IhK64NCp0IAW9ydm7fh6hHvp4XHfJf58n6bR1f9DFs_CiwmjKaKdO8nVmbBl213cVZTcqnCHFcgR24X_Aw_EjDIw8bO22FjcB)

![](https://lh6.googleusercontent.com/4u5v47uaOMZtRefvq8ezE3pYQc513ARaQe76gkmXXZAiHqMgFevYv99R7daY-dPNqwaIqgPufOXravWlFS0uA2ye584wXo-YlPoRlS8inhBU7LRzgauOYninBuJ-5pJ4W0fSfOLC)

-   Push to github: git push

![](https://lh5.googleusercontent.com/JkVRoNh83QapTljLfOIjUL0g5M686zl779HZVDluFM-oMdQneIfdOnRQz0slGg5z6LBBDJ5k6FqIF5KA7qnoT-Ftf5YeEYanZMvEyDYxYLNaeEgomMJTtMdliOtrn0HRg4kWXili)

-   Added random comments into the test file in github

Commit change (github)

Compare and Pull request (github)

Merge and confirm merge

![](https://lh6.googleusercontent.com/tVXFYgXVg088JIo2aKQ3Iwn8Jk-mbM38kUfxrR38STK6HNC2HN2UtzP-xPaSSoqzljYiM6IfFDqB9oIqbckaZqBGlRix1AKDBpsOu7D7mee5895ohcGGDZm1aLX4JzU76yKqvQim)

-   Ensure that the (elastic) IP address in the webhook is correct.

![](https://lh3.googleusercontent.com/vorIzk8eZnQq6QzUhfjhTyoL2oDVmKZ5WW4Ml8RB_9Ne6F2y9v1JFyRQA7H0wJIgwJGg1cnbtK3UhFJYZh2A_ef1rPHzbo8gxg9CpIJ6i2RjqHOgCD4oBTK5aBfFwEqrU4g40-nQ)

-   Ensure that the (elastic) IP address in the webhook is correct.

   Confirm new build and changes are success in Jenkins (Jenkins)

![](https://lh3.googleusercontent.com/4PRIlCQJGqqQaW4l_ahlE8zqtX819_14EL7lDXGdWdTGKsHZvdKNfDV7UepPV3UzNLaV1cwQ6Nvwuosme5XmAUd4urOx1DFYl04hck6VZG_vh7dOjcTCPuGLBMjNkSnperdmyhHb)

![](https://lh3.googleusercontent.com/lBRtCtxt2BA6bUnKUzTEAyeeZVWf9yB-wf4p9lZnFkfUK0ge2dn6ZzzOMEIm-Zw0WRMgP-LQd28P1KLD7kvOqj6ZoR9E_gocE-EmNgnWkjhmn6GMdAae7LB5r3ebvgAZPdHjba_T)

-   Execute ansible:

ansible-playbook -i /var/lib/jenkins/jobs/ansible/builds/11/archive/inventory/dev.yml /var/lib/jenkins/jobs/ansible/builds/11/archive/playbooks/common.yml  

![](https://lh5.googleusercontent.com/ed4TyNEdwkBVLzbzXxpP-6eS9EjWDKEJ2wVRWKUiKR_4e0X7n3L6slf7NHeIho-p_e6qirgCwpUsUm2FesyTr0XXUQXGu_DNGWBl0uvsOZrtNXxtZl3CixxP91fhxknO8J7M59-q)

![](https://lh6.googleusercontent.com/N5U8dBQVoAWarfv2_N7L-JUPw2-41UZUmYEPdbpESWococzNsrS-UsAc9XVd4qOG-P133qdVxI8EKdIoctYdmN5-xDwy4CSUfmWi0IOOwMHiL_Zt20PRmu5rduun_eTatckjvClc)

The above shows the execution of automated routine tasks by implementing this Ansible project.



## Credits:

Darey.io

<https://phoenixnap.com/kb/ansible-create-file> 

<https://docs.ansible.com/ansible/latest/collections/community/general/timezone_module.html>

<https://www.mydailytutorials.com/ansible-create-files/>

<https://linuxize.com/post/how-to-set-or-change-timezone-in-linux/>


