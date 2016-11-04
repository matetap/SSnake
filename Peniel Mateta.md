TP DevOps
=========

Note : 12,5 / 20

[1,5] Définir une VM avec un Vagrantfile
--------------------------------------
OK, la VM est bien définie. Ceci étant dit, vous auriez certainement du utiliser un dossier partagé (config.vm.synced_folder) plutôt que d'utiliser le provisionner "file". En effet, ce dernier va copier le dossier que vous désignez (votre application) dans votre VM, mais uniquement lors du provisioning. Donc si vous changez votre application, mais que vous ne reprovisionez pas votre VM, vous ne verrez pas les changements. Pas très pratique.

Déployer avec Ansible
---------------------

    [1] nginx
    ---------
    Ok, vous déployez nginx, mais vous ne vouliez pas le configurer aussi ?

    [1,5] sqlite
    ----------
    Ah, voilà, maintenant vous utilisez vraiment les rôles. Vous créez donc une base, c'est bien. Pas très utile, mais ça montre que vous sauriez le faire en cas de besoin ;) D'autre part, plutôt que de hardcoder la variable dans le fichier default, vous auriez pu mettre cette variable dans votre playbook, non ?

    [1] supervisord
    ---------------
    Ah, vous recommencez : vous installez sans configurer. Quel intérêt ?

    [1] gunicorn
    ------------
    Même remarque : vous installez sans configurer. Quel intérêt ? D'autre part, vous pourriez vous contenter d'installer gunicorn comme une dépendance de l'application, au sein de son virtualenv, via pip.

[3] Créer une appli Flask "hello world"
---------------------------------------
Votre application est très bien. Personnellement, j'aurais ajouté un fichier `requirements.txt` avec la liste des librairies dépendantes, juste pour simplifier le `pip install` de mon application.

[3,5] Créer un rôle qui déploie l'app Flask
-----------------------------------------
Ah, c'est donc là que vous configurez tous vos services ! Il n'est pas nécessaire de séparer la configuration d'un service de son déploiement, c'est même une pratique dangereuse. Vous devriez rassembler vos variables communes dans votre `playbook.yml`, et les utiliser dans vos rôles respectifs.
Vous avez en tout cas le bon réflexe de commencer par installer le virtualenv, puis y déployer vos dépendances.
Attention néanmoins, il faut passer l'activate (le votre a une espace en trop, au passage) pour **chaque** commande, car dans ansible, les commandes sont indépendantes les unes des autres.
Attention aussi sur les ip/ports d'écoute, où il semble que vous vous marchiez sur les pieds : vous demandez à nginx d'écouter sur 127.0.0.1:5000 et de faire proxy vers 127.0.0.1:5000.