# Aide-mémoire Shell

Astuce : Ctrl+F pour chercher une commande dans la page.

## Navigation et fichiers

| Commande | Ce que ça fait |
|---|---|
| `pwd` | Affiche le dossier courant |
| `ls / ls -l / ls -a` | Liste / liste détaillée (droits, propriétaire) / avec fichiers cachés |
| `cd dossier` | Entre dans un dossier |
| `cd ..` | Remonte d'un niveau (avec un espace) |
| `cd ~  ou  cd` | Va dans le dossier personnel |
| `mkdir dossier  /  mkdir -p a/b/c` | Crée un dossier / crée aussi les parents |
| `touch fichier` | Crée un fichier vide |
| `cp source dest  /  cp -r dossier dest` | Copie un fichier / un dossier |
| `mv source dest` | Déplace ou renomme |
| `rm fichier  /  rm -r dossier` | Supprime un fichier / un dossier et son contenu |
| `rmdir dossier` | Supprime un dossier vide |

## Lire et écrire du contenu

| Commande | Ce que ça fait |
|---|---|
| `cat fichier` | Affiche tout le fichier |
| `less fichier` | Affiche page par page (q pour quitter) |
| `head -n 5 fichier  /  tail -n 5 fichier` | 5 premières / 5 dernières lignes |
| `echo "texte" > fichier` | Écrit dans le fichier (écrase) |
| `echo "texte" >> fichier` | Ajoute à la fin du fichier |
| `echo "texte" \| sudo tee fichier` | Écrit dans un fichier avec les droits admin (tee -a pour ajouter) |
| `nano fichier` | Éditeur : Ctrl+O sauver, Entrée, Ctrl+X quitter |

## Redirections et flux

| Commande | Ce que ça fait |
|---|---|
| `cmd > fichier  /  cmd 1> fichier` | Sortie standard dans un fichier (écrase s'il existe) |
| `cmd >> fichier` | Ajoute à la fin du fichier (le crée s'il n'existe pas) |
| `cmd 2> fichier` | Redirige les erreurs (écrase) |
| `cmd 2>> fichier` | Ajoute les erreurs à la fin |
| `cmd 1>> log 2>&1` | Sortie et erreurs dans le même fichier (utile dans cron) |
| `cmd < fichier` | L'entrée de la commande vient d'un fichier |
| `cmd << END` | Saisie multiligne jusqu'au mot END |
| `sudo passwd user < motdepasse.txt` | Le mot de passe est lu dans le fichier |
| `cmd &> /dev/null` | Jette la sortie et les erreurs |
| `cmd1 \| cmd2` | Pipe : la sortie de cmd1 devient l'entrée de cmd2 |
| `awk '{print $9}'` | Affiche la 9e colonne de chaque ligne |
| `ls -l \| grep -E "^-.{0,6}(..x){1,3}"` | Fichiers exécutables (pas des dossiers, au moins un x) |
| `ls -al \| grep -E "^-.{0,8}x" \| awk '{print $9}'` | Noms des fichiers exécutables par quelqu'un |

## Utilisateurs et groupes

| Commande | Ce que ça fait |
|---|---|
| `sudo adduser alice` | Crée un utilisateur (interactif, avec dossier personnel) |
| `sudo useradd bob` | Crée un utilisateur (version de base, sans dossier perso) |
| `sudo passwd bob` | Définit ou change le mot de passe |
| `sudo deluser --remove-home bob` | Supprime l'utilisateur et son dossier personnel |
| `su - alice` | Se connecter en alice (exit pour revenir) |
| `whoami` | Mon nom d'utilisateur |
| `id bob  /  groups bob` | Infos et groupes d'un utilisateur |
| `sudo usermod -aG groupe user` | Ajoute user au groupe (le -a est obligatoire) |
| `sudo usermod -aG sudo user` | Le rend administrateur (sudoer) |
| `sudo groupadd nom` | Crée un groupe |
| `/etc/skel` | Dossier modèle copié dans le home des nouveaux utilisateurs |

## Propriétaire et droits

| Commande | Ce que ça fait |
|---|---|
| `ls -l fichier` | Lire les droits : -rwxr-xr-- (type, propriétaire, groupe, autres) |
| `sudo chown alice fichier` | Change le propriétaire |
| `sudo chown alice:groupe fichier` | Change propriétaire et groupe |
| `sudo chown -R alice:alice dossier` | Idem sur tout le dossier et son contenu |
| `sudo chgrp groupe fichier` | Change seulement le groupe |
| `chmod 640 fichier` | Droits en chiffres : r=4, w=2, x=1 (640 = rw- r-- ---) |
| `chmod u+x fichier` | Lettres : u=propriétaire, g=groupe, o=autres, a=tous ; + ajoute, - retire, = fixe |
| `chmod g+w,o-r fichier` | Plusieurs changements d'un coup |
| `chmod -R 755 dossier` | Sur tout un dossier |
| `chmod +x script.sh` | Rend un script exécutable |

## chmod : tableau des chiffres

| Commande | Ce que ça fait |
|---|---|
| `r = 4,  w = 2,  x = 1` | Le chiffre est la somme des droits : 6 = 4+2 = rw- |
| `0` | --- : aucun droit |
| `1` | --x : exécution seulement |
| `2` | -w- : écriture seulement |
| `3` | -wx : écriture + exécution (1+2) |
| `4` | r-- : lecture seulement |
| `5` | r-x : lecture + exécution (4+1) |
| `6` | rw- : lecture + écriture (4+2) |
| `7` | rwx : tous les droits (4+2+1) |
| `777` | rwxrwxrwx : tous les droits pour tout le monde (à éviter) |
| `755` | rwxr-xr-x : proprio tout, groupe et autres lisent/exécutent (scripts, dossiers) |
| `750` | rwxr-x--- : proprio tout, groupe lit/exécute, autres rien |
| `744` | rwxr--r-- : proprio tout, groupe et autres lisent |
| `700` | rwx------ : proprio seul |
| `666` | rw-rw-rw- : lire/écrire pour tout le monde |
| `664` | rw-rw-r-- : proprio et groupe lisent/écrivent, autres lisent |
| `660` | rw-rw---- : proprio et groupe lisent/écrivent, autres rien |
| `644` | rw-r--r-- : fichier normal (proprio écrit, tous lisent) |
| `640` | rw-r----- : proprio lit/écrit, groupe lit, autres rien |
| `600` | rw------- : proprio lit/écrit, personne d'autre |
| `444` | r--r--r-- : lecture seule pour tous |
| `400` | r-------- : lecture seule pour le proprio (clé privée SSH) |
| `-rwxr-xr--` | Lecture de ls -l : 1er caractère - fichier, d dossier, l lien ; puis proprio rwx, groupe r-x, autres r-- |
| `chmod u=rwx,g=rx,o= fichier` | Équivalent en lettres de 750 |
| `Sur un dossier` | r = lister le contenu, w = créer/supprimer des fichiers dedans, x = y entrer (cd) |
| `Qui peut chmod ?` | Le propriétaire ou l'administrateur (sudo) |

## date : les formats

| Commande | Ce que ça fait |
|---|---|
| `%Y  %y` | Année sur 4 chiffres (2026) / sur 2 chiffres (26) |
| `%m  %d` | Mois (01-12) / jour (01-31) |
| `%H  %M  %S` | Heure (00-23) / minute / seconde |
| `date +%d/%m/%Y` | 18/11/2026 |
| `date +%Y-%m-%d_%H-%M-%S` | 2026-11-18_11-25-00 (nom de sauvegarde) |

## Chercher et filtrer du texte

| Commande | Ce que ça fait |
|---|---|
| `wc -l fichier` | Nombre de lignes |
| `grep "motif" fichier` | Lignes qui contiennent le motif |
| `grep -c "motif" fichier` | Compte les lignes |
| `grep -v "motif" fichier` | Lignes SANS le motif |
| `grep -i "motif" fichier` | Ignore la casse |
| `grep -n "motif" fichier` | Affiche les numéros de ligne |
| `grep -E "regex" fichier` | Motif en expression rationnelle |
| `grep -r "motif" dossier` | Cherche dans tous les fichiers d'un dossier |
| `cut -d' ' -f1 fichier` | 1re colonne (séparateur espace) : les IP d'un log |
| `sort  /  sort -u  /  sort -nr` | Trie / trie sans doublons / trie numérique décroissant |
| `uniq -c` | Compte les doublons (l'entrée doit être triée) |
| `head -5` | Garde les 5 premières lignes |
| `cmd1 \| cmd2` | Pipe : la sortie de cmd1 devient l'entrée de cmd2 |
| `cut -d' ' -f1 access.log \| sort -u \| wc -l` | Nombre d'IP différentes |
| `cut -d' ' -f1 access.log \| sort \| uniq -c \| sort -nr \| head -5` | Top 5 des IP les plus actives |

## Chercher des fichiers

| Commande | Ce que ça fait |
|---|---|
| `ls ????` | Noms de exactement 4 caractères (? = un caractère) |
| `ls *.txt` | Tous les .txt (* = n'importe quoi) |
| `ls \| grep -E "^[a-zA-Z]{4}$"` | Noms de exactement 4 lettres (regex) |
| `find . -name "*.txt"` | Cherche dans le dossier et les sous-dossiers |
| `find . -name "????"` | Noms de 4 caractères, récursif |

## Archives

| Commande | Ce que ça fait |
|---|---|
| `tar -czf archive.tar.gz dossier` | Crée une archive compressée (c créer, z gzip, f fichier) |
| `tar -tzf archive.tar.gz` | Liste son contenu |
| `tar -xzf archive.tar.gz` | La décompresse |
| `date +%Y-%m-%d_%H-%M-%S` | Date formatée, pour nommer une sauvegarde |

## Réseau, SSH, transfert

| Commande | Ce que ça fait |
|---|---|
| `hostname -I` | Mon adresse IP |
| `wget URL` | Télécharge un fichier |
| `scp fichier user@ip:dossier` | Envoie un fichier à la machine distante (-r pour un dossier) |
| `ssh user@ip` | Se connecte à la machine distante |
| `ssh-keygen -t rsa -b 2048` | Crée une paire de clés (passphrase vide pour un script) |
| `ssh-copy-id -i cle.pub user@ip` | Envoie la clé publique à la machine distante |
| `~/.ssh/config` | Host ip  puis  IdentityFile chemin/de/la/cle_privee |
| `chmod 400 cle  /  chmod 644 ~/.ssh/config` | Droits : clé en lecture seule pour le proprio ; config rw-r--r-- |

## Cron (tâches planifiées)

| Commande | Ce que ça fait |
|---|---|
| `crontab -e` | Édite les tâches |
| `crontab -l` | Liste les tâches |
| `crontab -r` | Supprime TOUTES les tâches, sans confirmation |
| `minute heure jour mois jour_semaine commande` | Les 5 champs d'une ligne (jour_semaine : 0 = dimanche) |
| `* * * * *  /chemin/script.sh` | Toutes les minutes |
| `0 2 * * *  /chemin/script.sh` | Tous les jours à 2h00 |
| `*/5 * * * *  /chemin/script.sh` | Toutes les 5 minutes |
| `0 23 * * 3  /chemin/script.sh` | Tous les mercredis à 23h |
| `Chemin absolu` | Toujours donner le chemin absolu du script dans cron |

## Divers

| Commande | Ce que ça fait |
|---|---|
| `man commande  /  commande --help` | Le manuel / l'aide courte (q pour quitter) |
| `sudo commande` | Exécute avec les droits administrateur |
| `./script.sh` | Lance un script (après chmod +x) |
| `ln -s /chemin/script /usr/bin/nom` | Lien symbolique : le script se lance comme une commande |
| `printenv  /  echo $PATH` | Variables d'environnement |

## Rappel : syntaxe de script

| Commande | Ce que ça fait |
|---|---|
| `#!/bin/bash` | 1re ligne d'un script |
| `$1 $2 … $#  "$@"` | 1er, 2e paramètre… / nombre de paramètres / tous les paramètres |
| `$?` | Code retour de la dernière commande (0 = succès) |
| `x=5  puis  $x` | Variable : pas d'espace autour du = |
| `$(commande)` | Récupère le résultat d'une commande |
| `$((a+b))` | Calcul entier |
| `if [ cond ]; then … elif … else … fi` | Alternative (espaces obligatoires autour des [ ]) |
| `-eq -ne -lt -le -gt -ge` | Comparaisons de nombres |
| `-d  -f  -e  -r  -w  -x` | Dossier / fichier / existe / lecture / écriture / exécution |
| `for p in "$@"; do … done` | Boucle sur les paramètres |
| `while [ cond ]; do … done` | Boucle tant que |
| `case $x in 1) … ;; *) … ;; esac` | Choix multiple |
| `read -p "texte " var` | Demande une saisie |
| `exit 1` | Arrête le script en erreur (exit 0 = normal) |
| `t=(a b c)  ${t[0]}  ${#t[@]}` | Tableau : 1er élément / nombre d'éléments |
| `$RANDOM` | Entier aléatoire de 0 à 32767 |
| `function f { … }  puis  f 1 2` | Fonction (paramètres $1 $2 à l'intérieur) |

## Compléments : variables, tests, boucles

| Commande | Ce que ça fait |
|---|---|
| `export VAR=valeur` | Variable d'environnement (jusqu'à la fin de la session) |
| `printenv  /  printenv SHELL` | Liste des variables / valeur d'une variable |
| `$USER  $EUID  $PWD  $OLDPWD` | Utilisateur / identifiant de l'utilisateur / dossier courant / dossier précédent |
| `$SECONDS  $RANDOM  $?` | Secondes depuis le début du script / entier aléatoire / code retour |
| `$0` | Nom du script |
| `$IFS` | Séparateur utilisé pour découper une chaîne en tableau |
| `declare -i n=12` | Variable entière |
| `declare -a t` | Déclare un tableau |
| `declare -r PI=3.14` | Constante |
| `${!nom}` | Indirection : valeur de la variable dont le nom est dans nom |
| `echo "1712 * 89 + 18" \| bc` | Calcul (bash seul ne gère que les entiers) |
| `test a = b  /  [ a = b ]` | Test de texte (les deux écritures sont équivalentes) |
| `= != -z -n` | Chaînes : égal / différent / vide / non vide |
| `! cond  -a  -o` | NON / ET / OU dans [ ] |
| `[[ ! (a) && (b) \|\| (c) ]]` | Version [[ ]] : ! && \|\| |
| `-L  -nt  -ot` | Lien symbolique / plus récent que / plus ancien que |
| `for (( i=0; i<10; i++ ))` | Boucle for style C |
| `for i in {1..10}  /  {1..10..2}` | Intervalle / intervalle avec pas de 2 |
| `for j in lundi mardi mercredi` | Parcours d'une liste |
| `while ls $dossier; do … done` | Boucle tant que la commande réussit |
| `return N  /  echo  /  var=…` | 3 façons de renvoyer une valeur depuis une fonction (return : nombres seulement, lu avec $?) |
| `exit -1` | Arrête le script en erreur |
| `grep --color=always` | Met en évidence les correspondances |
| `sudo apt update  /  sudo apt install openssh-client` | Met à jour la liste des paquets / installe un paquet |
| `ssh-keygen -t rsa -b 2048 -f ~/keys/backup_key` | Crée la clé dans le fichier indiqué |
| `ls -a -l  =  ls -al  =  ls --all -l` | Les options peuvent se combiner |
| `man find  /  find --help` | Manuel / aide courte d'une commande |

## Rappel : regex

| Commande | Ce que ça fait |
|---|---|
| `^  $` | Début / fin du texte |
| `[0-9]  [a-z]  [^0-9]` | Un chiffre / une lettre minuscule / un caractère qui n'est pas un chiffre |
| `{3}  {1,3}  {2,}` | Exactement 3 / de 1 à 3 / 2 ou plus |
| `+  *  ?` | Au moins un / zéro ou plus / zéro ou un |
| `\|` | Ou : jpg\|png |
| `\.` | Un vrai point (hors crochets) ; . seul = n'importe quel caractère |
| `[[ "$1" =~ ^[0-9]{5}$ ]]` | Test de regex en bash |
| `^([0-9]{2} ){4}[0-9]{2}$` | Téléphone : 5 groupes de 2 chiffres |
| `^(0[1-9]\|[12][0-9]\|3[01])/(0[1-9]\|1[0-2])/[0-9]{4}$` | Date jj/mm/aaaa |
| `^[a-zA-Z0-9.]+@[a-zA-Z0-9]+\.[a-z]{2,}$` | E-mail simple |

## Exemples : utilisateurs et droits

| Commande | Ce que ça fait |
|---|---|
| `sudo adduser alice` | Crée alice : demande le mot de passe (pwdalice) et crée /home/alice |
| `sudo useradd bob  puis  sudo passwd bob` | Crée bob sans dossier perso, puis lui donne un mot de passe |
| `su - alice` | Se connecter en alice (exit pour revenir) |
| `sudo mkdir /home/alice/documents` | Crée le dossier chez alice (sudo obligatoire) |
| `echo "Rapport d'alice" \| sudo tee /home/alice/documents/rapport.txt` | Crée le fichier avec du texte (il appartient à root) |
| `sudo chown -R alice:alice /home/alice/documents` | Rend le dossier et son contenu à alice |
| `ls -l /home/alice/documents` | Affiche par exemple : -rw-r--r-- 1 alice alice 17 nov 18 11:00 rapport.txt |
| `sudo chmod 640 /home/alice/documents/rapport.txt` | Proprio rw-, groupe r--, autres rien |
| `sudo chmod g+w /home/alice/documents/rapport.txt` | Donne l'écriture au groupe (donne rw-rw----) |
| `sudo usermod -aG alice bob` | Ajoute bob au groupe alice |
| `groups bob` | Vérifie : bob alice |
| `su - bob  puis  nano /home/alice/documents/rapport.txt` | Nouvelle session : bob peut maintenant modifier le fichier |
| `sudo usermod -aG sudo bob` | bob peut utiliser sudo |
| `sudo mkdir /etc/skel/documents  puis  sudo adduser user3` | user3 reçoit un dossier documents dans son home |
| `sudo deluser --remove-home bob` | Supprime bob et /home/bob |
| `id bob` | Vérifie la suppression : « no such user » |

## Exemples : logs (access.log)

| Commande | Ce que ça fait |
|---|---|
| `wget https://www.lamarmotte.info/wp-content/uploads/2022/10/access.log` | Télécharge le fichier de logs |
| `wc -l access.log` | Nombre de lignes (de requêtes) |
| `grep "78.219.34.20" access.log` | Requêtes de cette IP |
| `grep -c "78.219.34.20" access.log` | Combien de requêtes pour cette IP |
| `cut -d' ' -f1 access.log` | Toutes les IP, avec doublons |
| `cut -d' ' -f1 access.log \| sort -u` | IP sans doublons |
| `cut -d' ' -f1 access.log \| sort -u \| wc -l` | Nombre d'IP différentes |
| `grep "GET" access.log \| grep "CM-3-Scripts-Shell.pdf" \| cut -d' ' -f1 \| sort -u` | IP qui ont téléchargé ce PDF avec GET |
| `grep "wp-login.php" access.log \| cut -d' ' -f1 \| sort -u` | IP suspectes : accès à wp-login.php |
| `cut -d' ' -f1 access.log \| sort \| uniq -c \| sort -nr \| head -5` | Top 5 : par exemple « 312 78.219.34.20 » |
| `cut -d' ' -f1 access.log \| sort \| uniq -c \| sort -nr \| awk '$1 > 100'` | Les IP avec plus de 100 requêtes |

## Exemples : sauvegarde, SSH, cron

| Commande | Ce que ça fait |
|---|---|
| `mkdir ~/data  &&  touch ~/data/maxime` | Dossier data avec un fichier à ton nom |
| `hostname -I \| awk '{print $1}'` | Première adresse IP de la machine |
| `tar -czf /tmp/backup_$(hostname -I \| awk '{print $1}')_$(date +%Y-%m-%d_%H-%M-%S).tar.gz ~/data` | Archive nommée backup_IP_date.tar.gz dans /tmp |
| `tar -tzf /tmp/backup_xxx.tar.gz` | Vérifie le contenu de l'archive |
| `scp /tmp/backup_xxx.tar.gz esistock@57.128.123.180:backups` | Envoie l'archive vers le dossier backups de la machine distante |
| `ssh esistock@57.128.123.180` | Se connecte pour vérifier (ls /home/esistock/backups) |
| `mkdir ~/keys` | Dossier pour les clés |
| `ssh-keygen -t rsa -b 2048 -f ~/keys/backup_key` | Crée backup_key et backup_key.pub (passphrase vide) |
| `ssh-copy-id -i ~/keys/backup_key.pub esistock@57.128.123.180` | Envoie la clé publique à la machine distante |
| `nano ~/.ssh/config` | Contenu : Host 57.128.123.180 puis IdentityFile ~/keys/backup_key |
| `chmod 400 ~/keys/backup_key ~/keys/backup_key.pub` | Clés en lecture seule pour le proprio |
| `chmod 644 ~/.ssh/config` | Config : proprio rw-, les autres lisent seulement |
| `crontab -e` | Puis ajouter la ligne ci-dessous |
| `* * * * * /home/pookiie/scripts/backup.sh /home/pookiie/data` | Toutes les minutes |
| `0 2 * * * /home/pookiie/scripts/backup.sh /home/pookiie/data >> /tmp/backup.log 2>&1` | Tous les jours à 2h, avec un fichier de log |
| `crontab -l` | Vérifie les tâches enregistrées |

## Exemples : fichiers, filtres, scripts

| Commande | Ce que ça fait |
|---|---|
| `touch abcd abc abcde test.txt zzzz` | Crée des fichiers de test |
| `ls ????` | Affiche abcd et zzzz (4 caractères) |
| `ls \| grep -E "^[a-z]{4}$"` | Idem avec une regex : abcd, zzzz |
| `ls ????.txt` | Fichiers de 4 caractères finissant par .txt |
| `find . -name "*.txt"` | Tous les .txt, sous-dossiers compris |
| `echo "ligne 1" > notes.txt  puis  echo "ligne 2" >> notes.txt` | Crée le fichier, puis ajoute une ligne à la fin |
| `cat notes.txt  /  head -n 1 notes.txt  /  wc -l notes.txt` | Tout afficher / 1re ligne / nombre de lignes (trois commandes séparées) |
| `nano exo1.sh  puis  chmod +x exo1.sh  puis  ./exo1.sh` | Créer, rendre exécutable, lancer |
| `./exo3.sh 4 7` | Lance le script avec 2 paramètres ($1 = 4, $2 = 7) |
| `sudo ln -s /home/pookiie/scripts/info /usr/bin/info` | Lance info depuis n'importe quel dossier |
| `./exo5.sh /home exo1.sh inconnu` | Plusieurs paramètres à la suite : dossier, fichier, n'existe pas |

## Pièges à retenir

| Commande | Ce que ça fait |
|---|---|
| `cd..` | Faux : il faut cd .. (avec un espace) |
| `[$1 -eq 0]` | Faux : espaces obligatoires, [ $1 -eq 0 ] |
| `x = 5` | Faux : x=5 sans espaces |
| `home/alice` | Faux : le chemin absolu commence par /home/alice |
| `usermod -G` | Sans -a, l'utilisateur perd ses autres groupes |
| `Nouveau groupe` | Pris en compte seulement après une nouvelle session (su - user) |
| `sudo echo "x" > f` | Échoue : la redirection est faite par ton shell, utiliser \| sudo tee f |
| `Fichier créé avec sudo` | Appartient à root : corriger avec sudo chown |

