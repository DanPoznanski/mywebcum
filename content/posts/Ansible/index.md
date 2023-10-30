---
title: "Ansible"
discription: ansible 
date: 2023-09-03T21:29:01+08:00 
draft: false
type: post
tags: ["Linux"]
showTableOfContents: true
--- 


![Ansible1](images/ansible1.svg)



Host

```
[frontend_servers]
reactjs_one ansible_host=192.168.1.200 ansible_user=ubuntu ansible_ssh_private_key=~/.ssh/id_rsa
```
```
ansible -i hosts frontend_servers -m ping
```



ansible.cfg
```
[defaults]
host_key_checking = False
inventory         = ./hosts

```

reactjs.yaml
```yaml
---
- hosts: frontend_servers
  
  vars:
    project_path: /var/www
    repository: gitlab.com/danpoznanski/example.git

  tasks: 

    - name: "Yarn | GPG"
      apt_key:
        url: https://dl.yarnpkg.com/debian/pubkey.gpg
        state: present
    
    - name: "Yarn | Ensure Debian sources list file exists"
      file: 
        path: /etc/apt/sources.list.d/yarn.list
        owner: root
        mode: 0644
        state: touch
    
    - name: "Yarn | Update APT cache"
      apt:
        update_cache: yes
    
    - name: Install the packages YARN, NPM, NodeJS, Nginx
      apt:
        pkg: ['yarn','npm','nodejs','ngnix']
    
    - name: Delete the html file
      file:
        path: "{{project_path}}/html"
        state: absent
    
    - name: Set some variable
      set_fact:
      release_path: "{{ project_path }}/releases/{{ lookup('pipe','date +%Y%m%d%H%M%S',) }}"
      current_path: "{{ project_path }}/html"
    
    
    - name: Create project path
      file: 
        dest={{ project_path }}
        mode=0755
        recurse=yes
        state=directory

    - name: Retrieve current release folder
      command: readlink -f html
      register: current_release_path
      ignore_errors: yes
      args:
        chdir: "{{ project_path }}"
    
    - name: Create Release folder
      file:
        dest={{ release_path }}
        mode=0755
        recurse=yes
        state=directory

    - name: Clone the repository
      git:
        repo: "{{ repository }}"
        dest: "{{ release_path }}"
    
    - name: YARN install dependencies
      command: yarn install
      args:
        chdir: "{{ release_path }}"
    
    - name: Yarn build
      command: yaen build
      args:
        chdir: "{{ release_path }}"
    
    - name: Update symlink
      file:
        scr={{ release_path }}/build
        dest={{ current_path }}
        state=link

```
run playbook `-b` or `--become` its sudo  
```
ansible-playbook -b reactjs.yaml -vv
```
delete files in instance
```
ansible ngnix_servers -m command -a "rm -rf <files>" -b
```



## Inventory Parameters

ansible_user - root/administrator

ansible_ssh_private_key 

ansible_connection - ssh/winrm/localhost

ansible_port - 22/5986

ansible_ssh_pass - Password 

hosts (ini format)
```
[frontend_servers]
reactjs_1 ansible_host=54.174.113.174 ansible_user=ubuntu ansible_ssh_private_key=~/.ssh/frontend_servers.pem
reactjs_2 ansible_host=54.174.1134.175 ansible_user=ubuntu ansible_ssh_private_key=~/.ssh/frontend_servers.pem

[db_servers]
db_1 ansible_host=database1.example.com ansible_user=ubuntu ansible_ssh_private_key=~/.ssh/database_servers.pem
db_2 ansible_host=database2.example.com ansible_user=ubuntu ansible_ssh_private_key=~/.ssh/database_servers.pem

[backend_servers]
ba_1 ansible_host=server1.example.com ansible_user=ubuntu ansible_ssh_private_key=~/.ssh/backend_servers.pem
ba_2 ansible_host=server2.example.com ansible_user=ubuntu ansible_ssh_private_key=~/.ssh/backend_servers.pem

[all_servers:children]
frontend_servers
db_servers
backend_servers
```
### Simple command group

```
ansible frontend_servers -a "uname -a"
```
```
ansible frontend_servers -m setup
```

```
ansible frontend_servers -m shell "ss -tlpn" 
```
The `-b` flag stands for "become" when you run an ansible playbook with this flag it allows you to become another user typically a superuser like root 
```
ansible frontend_servers -m apt -a 'name="net-tools"' -b 
```

### AWS and Ansible

Install module
```
ansible-galaxy collection install community.aws
```
#### Pipe json format 

``` 
ansible localhost  -m community.aws.ec2.instance_info -a 'region=us-east-1 filters=tag:Name="WebServer in Auto Scaling Group"'| jq
```
before if command not work
```
ANSIBLE_LOAD_CALLBACK=true
ANSIBLE_STDOUT_CALLBACK=json
```



## Ansible Ad-Hoc commands
```
ansible ngnix_servers -m user -a "name=ansible group=admin" -b

```

## Ansible modules

apt 

file

service 

template

```
ansible_doc module_name
```


## Playbook

ansible_users.yaml
```
---
- host: all
  vars:
  - users:
    - ansible
    - aakilin
  tasks:
  - name: "Ensure the ansible account exist"
    users:
      name: "{{ item }}" 
      groups: ["admin,sudo"]
      shell: /bin/bash
      loop: "{{ users }}" 
      tags: create_user_accounts
  - name: "Ensure authorized keys created"
   autorized_key:
     user: "{{ item }}"
     key: "{{ lookup('file', 'files/'+item+'.pub') }}"
     loop: "{{ users }}"
  - name: "Allow sudo   users to  witchout password"
    lineinfile:
      dest: "/etc/sudoers"
      state: "present"
      regexp: "^sudo"
      line: "%sudo  ALL=(ALL:ALL) NOPASSWORD:ALL"
  - name: "Disable password authentication"
    lineinfile:
      dest="/etc/ssh/ssh_config"
      regexp="^PasswordAuthentication no"
      state="present"
      backup="yes"
    notify:
      - restart ssh
  handlers:
  - name:
    service:
      name=ssh
      state=restarted    
```

```
ansible-playbook ansible_user.yaml --syntax-check 
```
no change in servers
```
ansible-playbook ansible_user.yaml --check 
```

hosts
```
[frontend_servers]
reactjs_2 ansible_host=54.174.113.174 ansible_user=ubuntu ansible_ssh_private_key=~/.ssh/id_rsa
reactjs_3 ansible_host=54.198.81.171 ansible_user=ubuntu ansible_ssh_private_key=~/.ssh/id_rsa
```

create reactjs.yaml
```
---
- hosts: frontend_servers

  vars:
    project_path: /var/www
    repositiry: https://gitlab.com/asomirl/skillbox-deploy-blue-green.git 
    packages:
          - yarn
          - npm
          - nodejs
  tasks:
    - name: "Yarn | GPG"
      apt_key:
        url: https://dl.yarnpkg.com/debian/pubkey.gpg
        state: present

    - name: "Yarn | Ensure Debian sources list file exists"
      file:
        path: /etc/apt/sources.list.d/yarn.list
        owner: root
        mode: 0644
        state: touch

    - name: "Yarn | Ensure Debian package is in sources list"
      lineinfile:
        dest: /etc/apt/sources.list.d/yarn.list
        regexp: 'deb http://dl.yarnpkg.com/debian/ stable main'
        line: 'deb http://dl.yarnpkg.com/debian/ stable main'
        state: present

    - name: "Yarn | Update APT cache"
      apt:
        update_cache: yes

    - name: Install the packages YARN, NPM, NodeJS, Nginx
      apt: 
        pkg: " {{ item }}
      loop: " {{ packages }}"
      tags: install_packages

    - name: Remove Nginx
      apt:
        name: nginx
        state: absent
      tags: remove_nginx 

    - name: Stop service Nginx
      ansible.builtin.systemd:
        name: nginx
        state: stopped

    - name: Delete the html file 
      file:
        path: "{{ project_path }}/html"
        state: absent

    - name: Set some variable
      set_fact:
        release_path: "{{ project_path }}/releases/{{ lookup('pipe','date +%Y%m%d%H%M%S') }}"
        current_path: "{{ project_path }}/html"
      tags: start_yarn

    - name: Create project path
      file:
        dest={{ project_path }}
        mode=0755
        recurse=yes
        state=directory

    - name: Retrieve current release folder
      command: readlink -f html
      register: current_release_path
      ignore_errors: yes
      args:
        chdir: "{{ project_path }}"

    - name: Create Release folder
      file:
        dest={{ release_path }}
        mode=0755
        recurse=yes
        state=directory

    - name: Clone the repository
      git:
        repo: "{{ repositiry }}"
        dest: "{{ release_path }}"


    - name: Add IP address of instance to main site 
      replace:
        path: "{{ release_path }}/src/App.js"
        regexp: 'Test of revert'
        replace: '{{ ansible_default_ipv4.address }}'
        backup: yes

    - name: Install packages based on package.json.
      npm:
        path: "{{ release_path }}"

    - name: npm install
      command: "npm install"
      args:
        chdir: "{{ release_path }}"


    - name: Start application
      command: "nohup yarn start &"
      args:
        chdir: "{{ release_path }}"
      environment:
        PORT: 80
      async: 1000
      poll: 0
      tags: start_yarn
```

## Roles

/etc/ansible/roles
```
[defaults]

host_key_checking = False
inventory         = ./hosts
roles_path        = ./roles
```

for create roles
```
ansible-galaxy init <name_role> -p .
```

ansible tree 
```
ansible-inventory --graph
```
ansible tree 
```
ansible-inventory --graph --vars
```

### See Setup
```
ansible reactjs_servers -m setup
```

ngnix.yaml
```yaml
---
- hosts: ngnix_servers
  vars:
    ngnix_vhosts:
    - listen: "80
    service_name: "react.akilin.com"
    state: "present"
    template: " {{ ngnix_vhost_template }}
    filename: "react.akilin.com.conf"
    extra_parameters: |
      location / {
          proxy_pass http://172.31.93.89:80 # http://{{ hostvars['reactjs1']['ansible_default_ipv4']['address']}}:80;
          proxy_set_header X-Forwarded-Host $host;
          roxy_set_header X-Forwarded-Server $host;
          roxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      }
    pre_tasks:
      - name: Ensure that the default ngnix symlink is absent
        file:
          path: "/etc/ngnix/sites-enabled/default"
          state: absent
    roles:
      - role: ngnix
```










### Group vars

group_vars
```

```
host_vars


## Template

template:
```j2
server {
    listen {{ http_port }};
    server_name {{ server_name }};
}
    location / {
        proxy_pass htt://{{ ip }}:{{ port }}/;
    }
}
```
Variables
```
http_port=80
server_name=react.myweb.com

ip=75.60.230.172 port=80
```

Results:
```
server {
    listen 80;
    server_name react.myweb.com;
}
    location / {
        proxy_pass http://75.60.230.172:80/;
    }
}
```

### work with lines

```
I love {{ language }} => I love Python
I love {{ language | lower }} I love python
I love {{ language | upper }} I love PYTHON
I love {{ language | replace("Python", "Java") }} I love Java
I love {{ language }} {{ version | default("3.9") }} => I love Python 3.9
{{ ["I", "love", "Python"] | join(" ") }} => I love Python
```


## Ansible Test

```
tasks:
 - service:
    name: foo
      state: started
      enabled: yes
```

```
roles:
 - webserver
tasks:
 - script: verify.sh
       check_mode: no 
```

test port
```
tasks:
 - wait_for:
      host: "{{ inventory_hostname }}"
      port: 22
      delegate_to: localhost 
```

test url
```
tasks:
 - action: uri=http://www.example.com return_content=yes
       register: webpage
 - fail:
       msg: 'service in not happy'
       when: "'AWESOME' not in webpage.content"      
```

test with `stat` files
```
tasks:
  - stat:
    path: /path/to/something
    registry: p
  - assert:
    that:
    - p.stat.exits and p.stat.isdir
```
test with `assert` files
```
tasks:
 - shell: /usr/bin/some-command --parameter value
   register: cmd_result
 - assert:
    that:
    - "'not ready' not in cmd_result.stderr"
    - "'gizmo enabled' in cmd_result.stdout"
```



### Molecule 

Molecule - test your role

dependency: Pull dependencies from ansible-galaxy if the role requires them.
lint: Check all the YAML files with yamllint.
cleanup: Executes the cleanup.yml playbook if exists.
destroy: If there is a VM with the same name running, destroy it.
syntax: Check the role with ansible-lint.
create: Create the VM.
prepare: Executes the prepare.yml playbook, which brings the host to a specific state.
converge: Executes the converge.yml playbook, which runs the role.
idempotence: Molecule runs the playbook a second time to check for idempotence.
verify: Run the tests defined in the tests directory.
cleanup: Executes the cleanup.yml playbook if exists.
destroy: Destroy the VM.

install python and packets 
```
sudo apt update
sudo apt install -y python3-pip libssl-dev
```
pip install molecule 
```
python3 -m pip install --user molecule
```
```
python3 -m pip install --user molecule ansible-lint
```
for check version
```
molecule-version
```
**Happy way init role**
```
molecule init role myrole -d delegated
```
**Default Catalog**
```
cd myrole/molecule/default/&& ls -l
converge.yml
molecule.yml
verify.yml
```


test configuration for virtualbox
```
vagrant:
  platforms:
        - name: ubuntu
        box:
ubuntu/trusty32
  providers:
        - name:
  virtualbox 
        type: virtualbox
        options:
              memory:
512
```



### Molecule AWS roles 

install 
```
pip install molecule-ec2
```
**Create a scenario**
With a new role
```
molecule init role -d ec2 my-role
```
Example
```
---
dependency:
   name: galaxy
driver:
   name: ec2
platforms:
  - name: instance
    image_owner: "099720109477"
    image_name: ubuntu/images/hvm-ssd/ubuntu-bionic-18.04-amd64-server-*
    instance_type: t2.micro
    vpc_subnet_id: <your-aws-vpc-subnet-id>
    tags:
      Name: molecule_instance
provisioner:
  name: ansible
verifier:
  name: ansible
```
```
molecule test
```

```
cd tasks

nano main.yml
```

```
---
- name: Ensure ngnix installed
package: 
        name: ngnix
        state: present 
```
```
cd defaults
nano verify.yml
```

```
- name: Verify
  hosts: all
  tasks:
        - name: Check ngnix binary
          stat:
                  path: "/usr/bin/ngnix"
          register: this
          failed_when: "not this.stat.exists"
```
```
mol converge
```



## KARATE

https://github.com/karatelabs/karate/wiki/Get-Started:-Maven-and-Gradle#github-template

karate test api 


Installing Maven:
```bash
sudo apt update
sudo apt install maven
```

Install Java Development Kit (JDK):
```bash
sudo apt update
sudo apt install default-jdk
```

Setting Up a Maven Project with Karate:

```bash
mvn archetype:generate -DgroupId=com.example -DartifactId=karate-demo -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

```
Configure `pom.xml` for Karate Dependencies:
```
<dependencies>
    <dependency>
        <groupId>com.intuit.karate</groupId>
        <artifactId>karate-apache</artifactId>
        <version>1.2.0</version> <!-- Use the latest version from Karate's GitHub releases -->
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>com.intuit.karate</groupId>
        <artifactId>karate-junit4</artifactId>
        <version>1.2.0</version> <!-- Use the latest version from Karate's GitHub releases -->
        <scope>test</scope>
    </dependency>
</dependencies>

```


Run the Tests:
```bash
mvn test
```

### Karate Test Example:
```
Feature: Get Tesin on reqres.in
  Scenario: Get Users List
    Given path 'https://reqres.in' + '/api/users' + '?page=2'
    When method GET
    Then status 200
```
```
mvn clean test
```
Feature: Get Tesin on reqres.in
Background:
* def urlBase = 'https://reqres.in' 
* def usersPath = 'api/users'
  Scenario: Get users list
    Given path urlBase + 'usersPath' + '?page=2'
    When method GET
    Then status 200
```
```
mvn clean test
```
```
Feature: Get Tesin on reqres.in
Background:
* def urlBase = 'https://reqres.in' 
* def usersPath = 'api/users'
  Scenario: Get users list and check value in field
    Given path urlBase + 'usersPath' + '?page=2'
    When method GET
    Then status 200
And match $..first_name contains 'Emma'
And match $..id contains '#notnull' 
```