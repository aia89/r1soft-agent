# r1soft-agent
Install r1soft agent
The R1Soft Server Backup Manager is a backup application for Linux and Windows machines that runs nearly continuously and is developed by R1Soft. This application allows users to schedule disk-based backups of their server that essentially create a virtual disk image, which is then stored online.

This yml will show you how to setup your CentOS 6 server with an R1Soft Agent so that you can start taking advantage of the security of near-continuous backups.

  

 Fisrts it will install dependency, othervise it gonna give an error to install it. roles for installing all packages you can find inside tasks file
     roles:
       - r1soft
#r1soft repository
    - copy:   #copy and paste r1soft.repo , i will put configuration file on r1soft.repo file
        src: /etc/ansible/roles/r1soft-agent/files/r1soft.repo  #change it for you own preference 
        dest: /etc/yum.repos.d
        owner: root
        group: root
        mode: 0644
#firewall
    - name: ensure iptables is configured to allow ssh traffic (port 1167/tcp)
      lineinfile:
        dest=/etc/sysconfig/iptables
        state=present
        regexp="^.*INPUT.*tcp.*1167.*ACCEPT"
        insertafter="^.*INPUT.*ESTABLISHED,RELATED.*ACCEPT" line="-A INPUT -m state --state NEW -m tcp -p tcp --dport 1167 -j ACCEPT"
        backup=yes

    - name: restart iptables
      service: name=iptables state=restarted



     - name: get module
      command: r1soft-setup --get-module
      ignore_errors: True     #in my case i have got an error but it will work anyway and i just ignore an error

    - name: restart service    # agent service restart
      shell: /etc/init.d/cdp-agent restart
    - name: get a key 
      command: r1soft-setup --get-key http://ip:8080






