List of ansible modules used in regular

ansible command line options: 
------------------------------
$ ansible -m --> module
          -a --> module arguments
          -i --> inventory file
          -e --> extra-vars
          -u --> remote_user
          -s --> sudo 
          -b --> become
          --become-methord --> sudo | su | pbrun | pfexec | doas | dzdo | ksu | runas | pmrun | enable | machinectl

Playbook keywords:
------------------------------------------------------------------------------
hosts: 
tasks:
gather_facts: false/true
become: yes
vars:
   ansible_become_user:

------------------------------------------------------------------------------
Debug:
------------------------------------------------------------------------------
---
- name: print ansible_facts using debug
  hosts: all.
  gather_facts: no/yes
  tasks:
  - debug: 
      vars: ansible_facts

------------------------------------------------------------------------------
yum
------------------------------------------------------------------------------
---
- name: Install httpd
  hosts: all
  tasks:
  - name: Install apache        # yum 
    yum:
      name: httpd
      state: installed

------------------------------------------------------------------------------
apt
------------------------------------------------------------------------------
  - name: Install apache        # apt
    apt: 
      name: apache2
      state: latest

------------------------------------------------------------------------------
package
------------------------------------------------------------------------------
  - name: Install apache        # package
    package: 
      name: httpd
      state: present

------------------------------------------------------------------------------
service
------------------------------------------------------------------------------
  - name: Start httpd service   # service
    service:
      name: httpd
      state: started

------------------------------------------------------------------------------
firewalld
------------------------------------------------------------------------------
  - name: open firewall         # firewalld
    firewalld: 
      port: 8080/tcp
      service: http
      zone: public
      state: enabled
      permanent: yes
      immediate: yes

------------------------------------------------------------------------------
file
------------------------------------------------------------------------------
  - name: create a directroy    # file
    file: 
      path: /home/karna/newdir/
      state: directroy
  - name: create a file
    file: 
      path: /home/karna/newdir/index.html
      state: touch
      owner: app-user 
      group: apt-user
      mode: 0644

------------------------------------------------------------------------------
tags
------------------------------------------------------------------------------
- name: using tags              # tags
  hosts: all
  tasks: 
  - name: install httpd
    apt: name=apache2 state=latest 
    tags: install httpd
  - name: start httpd service
    service: name=apache2 state=started
    tags: start httpd service

          $ ansible-playbook test.yaml -i hosts -t "install httpd"

------------------------------------------------------------------------------
archive
------------------------------------------------------------------------------
---
- name: archive the files          # archive
  hosts: all
  tasks: 
  - name: compress a folder
    archive: 
      path: /home/karna/newdir/
      dest: /tmp/newdir.gz
      format: zip|tar|bz2|xz|gz

------------------------------------------------------------------------------
cron
------------------------------------------------------------------------------
---
- name: cron jobcreation            # cron
  cron: 
    name: cron job for testing
    job: sh /tmp/sample.sh
    month: 2
    day: 30
    hour:  12
    minute: 10

------------------------------------------------------------------------------
user
------------------------------------------------------------------------------
---
- name: create new users            #user
  hosts: all
  tasks:
  - name: create new users
    user:
      name: dev-user
      uid: 1001
      group: dev
      shell: /root/user1/

------------------------------------------------------------------------------
Firewalld:
------------------------------------------------------------------------------
---
- name: Add firewalld rule
  hosts: all
  tasks:
  - firewalld:
       port: 8080/tcp
       service: http
       source: 192.0.0.0/24
       zone: public
       state: enabled
       permanent: yes
       immediate: yes

------------------------------------------------------------------------------
cron:
------------------------------------------------------------------------------
---
- name: 
  hosts: all
  any_errors_fatal: true --> if any task failed it will delete 
  max_fail_percentage: 30
  tasks:
  - name: cron job schedule
    cron: 
      name: 
      job: sh /opt/script/health.check
      month: 2/*
      day: 19/*
      hour: 8
      minute: 10
      weekday: 1
      
------------------------------------------------------------------------------      
users and groups:
------------------------------------------------------------------------------
tasks:
- name:
  user:
    name: 
    uid:
    group:
    shell:
- name: 

------------------------------------------------------------------------------
authorized_key:
------------------------------------------------------------------------------


------------------------------------------------------------------------------
uri: URL check 
------------------------------------------------------------------------------
---
- name: Verify the Apache service
  hosts: localhost
  tasks:
    - name: Ensure the webserver is reachable
      uri:
        url: http://app1.${GUID}.internal
        status_code: 200




==============================================================================================================
Conditional tasks:
==============================================================================================================

------------------------------------------------------------------------------
when
------------------------------------------------------------------------------

---
- name: install list of packages on Debian                      # When
  hosts: all
  vars:
     list:
     - firewalld
     - apache2
     - nginx
  tasks:
   - name: install list on Ubuntu
     yum: 
        name: "{{ item }}" 
        state: latest
     when: ansible_os_family == "Ubuntu"
     loop: "{{ list }}"
   - name: install list on Debian
     apt: 
        name: "{{ item }}" 
        state:  latest
     when: ansible_os_family == "Debian"
     loop: "{{ list }}"

------------------------------------------------------------------------------
vars
------------------------------------------------------------------------------
---
- name: install list of packages on Debian                      # vars
  hosts: all
  vars:
     list:
     - name: firewalld
       value: firewalld
     - name: install apache2
       value: apache2
     - name: nginx
       value: nginx
  tasks:
   - name: install list on Ubuntu
     yum: 
        name: "{{ item.value }}" 
        state: latest
     when: ansible_os_family == "Ubuntu"
     loop: "{{ list }}"
   - name: install list on Debian
     apt: 
        name: "{{ item.value }}" 
        state:  latest
     when: ansible_os_family == "Debian"
     loop: "{{ list }}"

------------------------------------------------------------------------------
Previous task result
------------------------------------------------------------------------------ 
---
- name: Conditional Task Example                                #  Conditional based on Previous Task Result
  hosts: localhost
  gather_facts: false

  tasks:
    - name: Check if a file exists
      stat:
        path: /path/to/some/file
      register: file_check

    - name: Display a message if the file exists
      debug:
        msg: "The file exists."
      when: file_check.stat.exists

---------------------------------------------------------      
---
- name: email service                       # register
  hosts: all
  tasks: 
  - name: check status
    command: service httpd status
    register: result.rc
-------------------------------------------------------
---
- name: install package                  # jinga vars   
  hosts: all
  become: true
  vars:
    users:
    - apache
    - nginx
  tasks:
  - name: create users
    user: name="{{ item }}"
    loop: "{{ users }}"
  - command: id "{{ item }}"
    loop: "{{ users }}"
------------------------------------------------------------
---
- name: using blocks in the program                    # blocks & Rescue & allways
  hosts: all
  tasks: 
  - block:
    - name: install apache
      apt: name=apache2 state=latest
    - name: start service
      service: name=apache2 state=started    
    become_user: apache
    when: ansible_os_family == "Debian"
    rescue: 
      - mail: 
          to: karunakarrao.devops@gmail.com
          subject: Install fail
          body: apache install fail {{ ansible_failed_task.name }}
  - block:
    - name: install nginx
      apt: name=nginx state=latest
    - name: start nginx service
      service: name=nginx state=started
    become_user: nginx
    when: ansible_os_family == "Debian"
---------------------------------------------------------   

=================================================================================================================
Include: include_vars, include_tasks, include_roles, Include
=================================================================================================================

vars.yaml
-----------------------------------
package:
- apache2
- nginx
- redis
- mangodb
-----------------------------------
- name: install                             # include_vars
  hosts: all
  tasks:
  - include_vars:
      file: /root/vars.yaml
      name: package
  - include_tasks:
      file: /root/install.yaml
      name: Install
      
  - name: install package
    apt: name="{{ item }}" state=latest
    loop: "{{ package.package }}"

-----------------------------------------------------
--- 
- name: Install                             # include_tasks
  hosts: all
  tasks:
  - include_tasks: /root/install-db.yaml
  - include_tasks: /root/install-app.yaml
  - include_tasks: /root/install_web.yaml
  - include_tasks: /root/start-service_db.yaml
  
----------------------------------------------------


======================================================================================================================
Error Handling:
======================================================================================================================

---------------------------------------------------
# Stop the playbook on all servers, if any once task failed on the play on any of the servers. 
---
- name: install                 #
  hosts: all
  any_error_fatal: true
  tasks:

--------------------------------------------------
# if more than 30% servers are failed then quit the play 
---
- name: install                 #
  hosts: all
  max_fail_percentage: 30
  tasks:
  
---------------------------------------------------
# To ignore errors for a task we can use this `ignore_errors`, so the task is igonred if fail/pass.
---
- name: install                 #
  hosts: all
  tasks:
  - name: install 
    apt: name=apache2 state=latest
  - mail: 
      to: 
      subject:
      body:
    ignore_errors: true
    
---------------------------------------------------
# if errors found in the server log, we can make the playbook fail. 
---
- name: install
  hosts: all
  any_error_fatal: true 
  tasks: 
  - command: cat /var/log/server.log
    register: command_output
    failed_when: '"ERROR"  in command_output.stdout'

-----------------------------------------------------
# incase failure in the block it will trigger the rescue section. 
---
- name: install & service
  hosts: all
  tasks:
  - blocks:
      - name: install apache2
        apt: name=apache2 state=latest
      - name: start apache
        servcie: name=apache2 state=started
    rescue:
    - mail:
        to: dev-group@hcl.com
        subject: failed playbook
        body: 
    always:
    - mail: 
        to: 
        subject:
        body:

-------------------------------------------------
# how to use blocks and Handling the errors.
---
- name: install
  hosts: all
  tasks: 
    - block:
      - name: install mysql
        yum: name=mysql state=present
      - name: start mysql servvice
        service: name=mysql-serverr state=started
      become_user: db-user
      when: ansible_facts['distribution'] == 'centos'
      rescue:
       - mail:
           to:
           subject: 
           body: DB install failed at {{ ansible_failed_task.name }} 
      always:
      - mail:
          to: 
          subject
          body: 
       ignore_errors: yes

======================================================================================================================
Ansible - Strategy:
======================================================================================================================
There are 2 types of Strategies in Ansible. 
    1. Linear (Defalut) - You don't need to specify.
    2. Free - 

Linear strategy: is the default strategy used by ansible, if we are using linear strategy the ansible tasks execution will be done parallel accross all servers. if one server is completed first it will wait for the other servers to complete. if it got failed then it will also get failed accross all servers. 

Free strategy: when you use Free strategy, it will complete the task execution independently with out witing for other servers to complete.

change the strategy as below. 
----------------------------------------------------
# free will install tasks on the servers independently with out waiting for other servers.
---
- name: install
  hosts: all
  strategy: free 
  tasks: 
  - name: 

======================================================================================================================
Ansible - Serial:
======================================================================================================================
by default how many servers ansible can perform playbook changes on remote servers is 5. Ansible can create 5 forks by Defalut. this is configured in `ansibile.cfg` file as "forks = 5". how ever we can change this behaviour using to execute tasks BATCH wise on host servers, we use the "serial" directive. it means, it will performs playbook execution on 3 servers.
---
- name: install
  hosts: all
  serial: 3
  tasks:
  - name:
  
-----------------------------------------------------

Note: by default how many servers ansible can perform playbook changes is 5. Ansible can create 5 forks by Defalut. this is configured in ansibile.cfg file as "forks = 5". how ever we can change this behaviour using ansible configuration file. 

=============================================================================================================
Playbook-tasks execution order:
=============================================================================================================
Roles are executed before the playbook tasks. To execute tasks before the roles, we can use the directives like pre_tasks,  post_tasks. 

---------------------------------------------
---
- name: install 
  hosts: all
  tasks:
  - name: install apache2
    apt: name=apache2 state=latest
  post_tasks:
  - name: start servcie
    service: name=apache2 state=started    
  pre_tasks:
  - name: check os version
    command: cat /etc/*release*
    
=============================================================================================================
handlers:
=============================================================================================================
Handlers in Ansible are used to trigger actions in response to events during the execution of playbooks. Typically, handlers are defined to perform tasks like restarting a service or taking some other action only when notified by a specific task. 

---
- name: install 
  hosts: all
  tasks: 
    - name: install
      apt: 
        name:
          - apache2
          - nginx
        state: latest
      notify: start service
  handlers:
    - name: start service
      service:              # The service module handles services individually, so you should provide a single service name.
        name: "{{ item }}"
        state: started
      loop:
        - apache2
        - nginx
        
------------------------------------------------------------------------------------------------------


Sample application demo  setup:
=======================================================================================================================

$ sudo yum install firewalld
$ sudo service firewalld start
$ sudo systemctl enable firewalld 

$ sudo yum install mariadb-server
$ sudo vi /etc/my.cnf 
$ sudo service mariadb start
$ sudo systemctl enable mariadb
$ sudo firewall-cmd --permanent --zone=public --add-port=3306/tcp
$ sudo sirewall-cmd --reload

$ mysql
mariadb> CREATE DATABASE ecomdb;
mariadb> CREATE USER 'ecomuser'@'localhost' IDENTIFIED BY 'ecompassword';
mariadb> GRANT ALL PRIVILEGES ON *.* TO 'ecomuser'@'localhost';
mariadb> FLUSH PRIVILEGES;

