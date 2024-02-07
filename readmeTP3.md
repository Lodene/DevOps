j'ai crée un fichier contenant :
    all:
    vars:
    ansible_user: centos
    ansible_ssh_private_key_file: ./../../../.ssh/ssh_key
    children:
    prod:
        hosts: antoine.adjamidis.takima.cloud

j'ai précisé ici l'utilisateur (centos), où trouver le clé ssh ./../../../.ssh/ssh_key. Pour terminé je précise l'host que vous nous avez envoyé par mail.

on peut voir ensuite que la commande : "ansible all -i inventories/setup.yml -m ping" fonctionne :
    antoine.adjamidis.takima.cloud | SUCCESS => {
        "ansible_facts": {
            "discovered_interpreter_python": "/usr/bin/python"
        },
        "changed": false,
        "ping": "pong"
    }

je recois la réponse "pong"

Ensuite, pour recevoir la distribution de votre OS, je dois faire cette commande :
    ansible all -i setup.yml -m setup -a "filter=ansible_distribution*"
qui me return :
    antoine.adjamidis.takima.cloud | SUCCESS => {
        "ansible_facts": {
            "ansible_distribution": "CentOS",
            "ansible_distribution_file_parsed": true,
            "ansible_distribution_file_path": "/etc/redhat-release",
            "ansible_distribution_file_variety": "RedHat",
            "ansible_distribution_major_version": "7",
            "ansible_distribution_release": "Core",
            "ansible_distribution_version": "7.9",
            "discovered_interpreter_python": "/usr/bin/python"
        },
        "changed": false
    }

    On nous demande ensuite de supprimer le server apache crée lors du td :
    ansible all -i setup.yml -m yum -a "name=httpd state=absent" --become
    Avec comme réponse :
    antoine.adjamidis.takima.cloud | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "changes": {
        "removed": [
            "httpd"
        ]
    },
    "msg": "",
    "rc": 0,
    "results": [
        "Loaded plugins: fastestmirror\nResolving Dependencies\n--> Running transaction check\n---> Package httpd.x86_64 0:2.4.6-99.el7.centos.1 will be erased\n--> Finished Dependency Resolution\n\nDependencies Resolved\n\n================================================================================\n Package      Arch          Version                       Repository       Size\n================================================================================\nRemoving:\n httpd        x86_64        2.4.6-99.el7.centos.1         @updates        9.4 M\n\nTransaction Summary\n================================================================================\nRemove  1 Package\n\nInstalled size: 9.4 M\nDownloading packages:\nRunning transaction check\nRunning transaction test\nTransaction test succeeded\nRunning transaction\n  Erasing    : httpd-2.4.6-99.el7.centos.1.x86_64                           1/1 \n  Verifying  : httpd-2.4.6-99.el7.centos.1.x86_64                           1/1 \n\nRemoved:\n  httpd.x86_64 0:2.4.6-99.el7.centos.1                                          \n\nComplete!\n"
    ]
    }

Pour valider si cela a bien fonctionner (autrement qu'en regardant la réponse), on peut executer une deuxieme fois la commande, ce qui nous donnera comme réponse :
    antoine.adjamidis.takima.cloud | SUCCESS => {
        "ansible_facts": {
            "discovered_interpreter_python": "/usr/bin/python"
        },
        "changed": false,
        "msg": "",
        "rc": 0,
        "results": [
            "httpd is not installed" <---- aucun server trouvé
        ]
    }
