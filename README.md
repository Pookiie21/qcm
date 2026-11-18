# Aide-mémoire Shell

Pour chaque commande : la formule générique, un exemple, puis ce qu'elle fait. Ctrl+F pour chercher dans la page.

## Navigation et fichiers

| Formule générique | Exemple | Explication |
|---|---|---|
| `pwd` | `pwd` | Affiche le dossier courant |
| `ls [options] [dossier]` | `ls -l /home` | Liste le contenu (-l détaillé, -a avec fichiers cachés, -al les deux) |
| `cd dossier` | `cd /home/alice` | Entre dans un dossier |
| `cd ..` | `cd ..` | Remonte d'un niveau (l'espace est obligatoire) |
| `cd` | `cd` | Retour au dossier personnel |
| `mkdir [-p] dossier` | `mkdir -p tp/data` | Crée un dossier (-p : crée aussi les parents) |
| `touch fichier` | `touch notes.txt` | Crée un fichier vide |
| `cp [-r] source destination` | `cp -r data /tmp/data2` | Copie (-r pour un dossier) |
| `mv source destination` | `mv ancien.txt nouveau.txt` | Déplace ou renomme |
| `rm [-r] cible` | `rm -r vieux_dossier` | Supprime (-r : dossier et contenu) |
| `rmdir dossier` | `rmdir vide` | Supprime un dossier vide |

## Lire et écrire du contenu

| Formule générique | Exemple | Explication |
|---|---|---|
| `cat fichier` | `cat notes.txt` | Affiche tout le fichier |
| `less fichier` | `less access.log` | Affiche page par page (q pour quitter) |
| `head -n N fichier` | `head -n 5 access.log` | Les N premières lignes |
| `tail -n N fichier` | `tail -n 5 access.log` | Les N dernières lignes |
| `echo "texte" > fichier` | `echo "bonjour" > a.txt` | Écrit dans le fichier (écrase) |
| `echo "texte" >> fichier` | `echo "suite" >> a.txt` | Ajoute à la fin du fichier |
| `echo "texte" \| sudo tee [-a] fichier` | `echo "Rapport" \| sudo tee /home/alice/r.txt` | Écrit avec les droits admin (-a pour ajouter). Pas de sudo echo ... > fichier : ça échoue |
| `nano fichier` | `nano exo1.sh` | Éditeur : Ctrl+O puis Entrée pour sauver, Ctrl+X pour quitter |

## Redirections et flux

| Formule générique | Exemple | Explication |
|---|---|---|
| `cmd > fichier` | `ls -l > liste.txt` | Sortie dans un fichier (écrase) |
| `cmd >> fichier` | `date >> log.txt` | Sortie ajoutée à la fin |
| `cmd 2> fichier` | `ls inconnu 2> erreurs.txt` | Erreurs dans un fichier (écrase) |
| `cmd 2>> fichier` | `ls inconnu 2>> erreurs.txt` | Erreurs ajoutées à la fin |
| `cmd >> log 2>&1` | `./backup.sh >> /tmp/backup.log 2>&1` | Sortie et erreurs dans le même fichier (utile dans cron) |
| `cmd < fichier` | `sort < noms.txt` | L'entrée de la commande vient d'un fichier |
| `cmd << MOT` | `cat << END` | Saisie multiligne jusqu'à la ligne END |
| `cmd &> /dev/null` | `id bob &> /dev/null` | Jette la sortie et les erreurs |
| `cmd1 \| cmd2` | `ls \| wc -l` | Pipe : la sortie de cmd1 devient l'entrée de cmd2 |
| `awk '{print $N}'` | `ls -l \| awk '{print $9}'` | Affiche la colonne N de chaque ligne |
| `ls -l \| grep -E "^-.{0,6}(..x){1,3}"` | `ls -l \| grep -E "^-.{0,6}(..x){1,3}"` | Fichiers exécutables (pas des dossiers, au moins un x) |

## Utilisateurs et groupes

| Formule générique | Exemple | Explication |
|---|---|---|
| `sudo adduser nom` | `sudo adduser alice` | Crée un utilisateur (interactif, avec dossier personnel) |
| `sudo useradd nom` | `sudo useradd bob` | Crée un utilisateur (de base, sans dossier perso ni mot de passe) |
| `sudo passwd nom` | `sudo passwd bob` | Définit ou change le mot de passe |
| `sudo deluser --remove-home nom` | `sudo deluser --remove-home bob` | Supprime l'utilisateur et son dossier personnel |
| `su - nom` | `su - alice` | Se connecter en tant que nom (exit pour revenir) |
| `whoami` | `whoami` | Mon nom d'utilisateur |
| `id nom` | `id bob` | Identifiant et groupes (no such user s'il n'existe pas) |
| `groups nom` | `groups bob` | Liste les groupes de l'utilisateur |
| `sudo usermod -aG groupe nom` | `sudo usermod -aG alice bob` | Ajoute nom au groupe (le -a est obligatoire) |
| `sudo usermod -aG sudo nom` | `sudo usermod -aG sudo bob` | Le rend administrateur (sudoer) |
| `sudo groupadd groupe` | `sudo groupadd equipe` | Crée un groupe |
| `/etc/skel` | `sudo mkdir /etc/skel/documents` | Dossier modèle copié dans le home des nouveaux utilisateurs |

## Propriétaire et droits

| Formule générique | Exemple | Explication |
|---|---|---|
| `ls -l fichier` | `ls -l rapport.txt` | Affiche par exemple : -rw-r----- 1 alice alice 17 nov 18 11:00 rapport.txt |
| `sudo chown [-R] user[:groupe] cible` | `sudo chown -R alice:alice /home/alice/documents` | Change propriétaire et groupe (-R : dossier et contenu) |
| `sudo chgrp groupe cible` | `sudo chgrp equipe rapport.txt` | Change seulement le groupe |
| `chmod NNN cible` | `chmod 640 rapport.txt` | Droits en chiffres : proprio, groupe, autres (voir tableau) |
| `chmod [ugoa][+-=][rwx] cible` | `chmod g+w rapport.txt` | Droits en lettres : u proprio, g groupe, o autres, a tous ; + ajoute, - retire, = fixe |
| `chmod g+w,o-r cible` | `chmod g+w,o-r rapport.txt` | Plusieurs changements d'un coup |
| `chmod u=rwx,g=rx,o= cible` | `chmod u=rwx,g=rx,o= script.sh` | Fixe exactement les droits (équivaut à 750) |
| `chmod -R NNN dossier` | `chmod -R 755 site` | Sur tout un dossier |
| `chmod +x script` | `chmod +x exo1.sh` | Rend un script exécutable |

## chmod : tableau des chiffres

| Formule générique | Exemple | Explication |
|---|---|---|
| `r = 4, w = 2, x = 1` | `rw- = 4+2 = 6` | Le chiffre est la somme des droits |
| `0` | `chmod 000 f` | --- : aucun droit |
| `1` | `chmod 111 f` | --x : exécution seulement |
| `2` | `chmod 222 f` | -w- : écriture seulement |
| `3` | `chmod 333 f` | -wx : écriture + exécution |
| `4` | `chmod 444 f` | r-- : lecture seulement |
| `5` | `chmod 555 f` | r-x : lecture + exécution |
| `6` | `chmod 666 f` | rw- : lecture + écriture |
| `7` | `chmod 777 f` | rwx : tous les droits |
| `777` | `chmod 777 f` | rwxrwxrwx : tous les droits pour tout le monde (à éviter) |
| `755` | `chmod 755 script.sh` | rwxr-xr-x : proprio tout, les autres lisent et exécutent |
| `750` | `chmod 750 dossier` | rwxr-x--- : proprio tout, groupe lit/exécute, autres rien |
| `744` | `chmod 744 index.html` | rwxr--r-- : proprio tout, les autres lisent |
| `700` | `chmod 700 prive` | rwx------ : proprio seul |
| `666` | `chmod 666 f` | rw-rw-rw- : lire/écrire pour tout le monde |
| `664` | `chmod 664 f` | rw-rw-r-- : proprio et groupe écrivent, autres lisent |
| `660` | `chmod 660 f` | rw-rw---- : proprio et groupe écrivent, autres rien |
| `644` | `chmod 644 f` | rw-r--r-- : fichier normal (proprio écrit, tous lisent) |
| `640` | `chmod 640 rapport.txt` | rw-r----- : proprio lit/écrit, groupe lit, autres rien |
| `600` | `chmod 600 f` | rw------- : proprio lit/écrit, personne d'autre |
| `444` | `chmod 444 f` | r--r--r-- : lecture seule pour tous |
| `400` | `chmod 400 ~/keys/backup_key` | r-------- : lecture seule pour le proprio (clé privée SSH) |
| `-rwxr-xr--` | `ls -l` | Lecture : 1er caractère - fichier, d dossier, l lien ; puis proprio rwx, groupe r-x, autres r-- |
| `Sur un dossier` | `chmod 755 dossier` | r = lister le contenu, w = créer/supprimer dedans, x = y entrer (cd) |
| `Qui peut chmod ?` | `sudo chmod 640 f` | Le propriétaire ou l'administrateur (sudo) |

## date : les formats

| Formule générique | Exemple | Explication |
|---|---|---|
| `date +FORMAT` | `date +%d/%m/%Y` | Affiche 18/11/2026 |
| `%Y  %y` | `date +%Y` | Année sur 4 chiffres (2026) / sur 2 chiffres (26) |
| `%m  %d` | `date +%m-%d` | Mois (01-12) / jour (01-31) |
| `%H  %M  %S` | `date +%H:%M:%S` | Heure (00-23) / minute / seconde |
| `date +%Y-%m-%d_%H-%M-%S` | `date +%Y-%m-%d_%H-%M-%S` | 2026-11-18_11-25-00 (pour nommer une sauvegarde) |

## Chercher et filtrer du texte

| Formule générique | Exemple | Explication |
|---|---|---|
| `wc -l fichier` | `wc -l access.log` | Nombre de lignes |
| `grep "motif" fichier` | `grep "78.219.34.20" access.log` | Lignes qui contiennent le motif |
| `grep -c "motif" fichier` | `grep -c "wp-login" access.log` | Compte les lignes qui correspondent |
| `grep -v "motif" fichier` | `grep -v "GET" access.log` | Lignes SANS le motif |
| `grep -i "motif" fichier` | `grep -i "erreur" log.txt` | Ignore majuscules/minuscules |
| `grep -n "motif" fichier` | `grep -n "root" /etc/passwd` | Affiche les numéros de ligne |
| `grep -E "regex" fichier` | `grep -E "^[0-9]{3}" access.log` | Motif en expression rationnelle |
| `grep -r "motif" dossier` | `grep -r "root" /etc` | Cherche dans tous les fichiers du dossier |
| `cut -d'SEP' -fN fichier` | `cut -d' ' -f1 access.log` | Colonne N (séparateur SEP) : la 1re colonne d'un log Apache = l'IP |
| `sort [-u] [-nr]` | `cut -d' ' -f1 access.log \| sort -u` | Trie (-u sans doublons, -nr numérique décroissant) |
| `uniq -c` | `sort \| uniq -c` | Compte les doublons consécutifs (donc trier avant) |
| `head -N` | `... \| head -5` | Garde les N premières lignes |
| `Nombre d'IP différentes` | `cut -d' ' -f1 access.log \| sort -u \| wc -l` | cut extrait les IP, sort -u supprime les doublons, wc -l compte |
| `Top 5 des IP` | `cut -d' ' -f1 access.log \| sort \| uniq -c \| sort -nr \| head -5` | Résultat du type : 312 78.219.34.20 |
| `IP avec plus de 100 requêtes` | `cut -d' ' -f1 access.log \| sort \| uniq -c \| sort -nr \| awk '$1 > 100'` | awk garde les lignes dont la 1re colonne dépasse 100 |
| `IP qui accèdent à un script` | `grep "wp-login.php" access.log \| cut -d' ' -f1 \| sort -u` | IP suspectes |
| `IP ayant téléchargé un fichier` | `grep "GET" access.log \| grep "CM-3-Scripts-Shell.pdf" \| cut -d' ' -f1 \| sort -u` | Deux grep enchaînés |

## Chercher des fichiers

| Formule générique | Exemple | Explication |
|---|---|---|
| `ls ?? (un ? par caractère)` | `ls ????` | Noms de exactement 4 caractères |
| `ls *.ext` | `ls *.txt` | Tous les fichiers .txt (* = n'importe quoi) |
| `ls ????.ext` | `ls ????.txt` | 4 caractères puis .txt |
| `ls \| grep -E "regex"` | `ls \| grep -E "^[a-zA-Z]{4}$"` | Noms de exactement 4 lettres |
| `find chemin -name "motif"` | `find . -name "*.txt"` | Cherche aussi dans les sous-dossiers |

## Archives

| Formule générique | Exemple | Explication |
|---|---|---|
| `tar -czf archive.tar.gz dossier` | `tar -czf /tmp/backup.tar.gz ~/data` | Crée une archive compressée (c créer, z gzip, f fichier) |
| `tar -tzf archive.tar.gz` | `tar -tzf /tmp/backup.tar.gz` | Liste son contenu |
| `tar -xzf archive.tar.gz` | `tar -xzf /tmp/backup.tar.gz` | La décompresse |
| `tar -czf /tmp/backup_$(hostname -I \| awk '{print $1}')_$(date +%Y-%m-%d_%H-%M-%S).tar.gz dossier` | `tar -czf /tmp/backup_$(hostname -I \| awk '{print $1}')_$(date +%Y-%m-%d_%H-%M-%S).tar.gz ~/data` | Archive nommée backup_IP_date.tar.gz comme dans le TD |

## Réseau, SSH, transfert

| Formule générique | Exemple | Explication |
|---|---|---|
| `hostname -I` | `hostname -I` | Mon adresse IP |
| `wget URL` | `wget https://www.lamarmotte.info/wp-content/uploads/2022/10/access.log` | Télécharge un fichier |
| `scp [-r] source user@ip:dossier` | `scp /tmp/backup.tar.gz esistock@57.128.123.180:backups` | Envoie un fichier vers la machine distante (-r pour un dossier) |
| `ssh user@ip` | `ssh esistock@57.128.123.180` | Se connecte à la machine distante |
| `ssh-keygen -t rsa -b 2048 -f chemin` | `ssh-keygen -t rsa -b 2048 -f ~/keys/backup_key` | Crée la clé privée et backup_key.pub (passphrase vide) |
| `ssh-copy-id -i cle.pub user@ip` | `ssh-copy-id -i ~/keys/backup_key.pub esistock@57.128.123.180` | Envoie la clé publique à la machine distante |
| `~/.ssh/config` | `Host 57.128.123.180 puis IdentityFile ~/keys/backup_key` | Indique quelle clé utiliser pour cette machine |
| `chmod 400 clé` | `chmod 400 ~/keys/backup_key ~/keys/backup_key.pub` | Clés en lecture seule pour le propriétaire |
| `chmod 644 ~/.ssh/config` | `chmod 644 ~/.ssh/config` | Config : proprio rw-, les autres lisent seulement |
| `sudo apt update` | `sudo apt update` | Met à jour la liste des paquets |
| `sudo apt install paquet` | `sudo apt install openssh-client` | Installe un paquet |

## Cron (tâches planifiées)

| Formule générique | Exemple | Explication |
|---|---|---|
| `crontab -e` | `crontab -e` | Édite les tâches (on y ajoute une ligne) |
| `crontab -l` | `crontab -l` | Liste les tâches |
| `crontab -r` | `crontab -r` | Supprime TOUTES les tâches, sans confirmation |
| `min heure jour mois jour_semaine commande` | `30 8 * * 1 /home/pookiie/backup.sh` | Les 5 champs (jour_semaine : 0 = dimanche) : ici lundi à 8h30 |
| `* * * * * commande` | `* * * * * /home/pookiie/backup.sh` | Toutes les minutes |
| `*/N * * * * commande` | `*/5 * * * * /home/pookiie/backup.sh` | Toutes les N minutes |
| `0 H * * * commande` | `0 2 * * * /home/pookiie/backup.sh` | Tous les jours à H heures (ici 2h) |
| `0 H * * J commande` | `0 23 * * 3 /home/pookiie/backup.sh` | Un jour de la semaine (ici mercredi, 23h) |
| `0 H J * * commande` | `0 23 1 * * /home/pookiie/backup.sh` | Un jour du mois (ici le 1er, 23h) |
| `commande >> log 2>&1` | `0 2 * * * /home/pookiie/backup.sh >> /tmp/backup.log 2>&1` | Garde une trace de l'exécution |
| `chemin absolu` | `/home/pookiie/backup.sh` | Toujours le chemin absolu du script dans cron |

## Divers

| Formule générique | Exemple | Explication |
|---|---|---|
| `man commande` | `man find` | Le manuel (q pour quitter) |
| `commande --help` | `ls --help` | Aide courte |
| `sudo commande` | `sudo adduser alice` | Exécute avec les droits administrateur |
| `./script.sh [paramètres]` | `./exo3.sh 4 7` | Lance un script (après chmod +x) |
| `ln -s cible lien` | `sudo ln -s /home/pookiie/scripts/info /usr/bin/info` | Lien symbolique : le script se lance comme une commande |
| `export VAR=valeur` | `export NOM=Max` | Variable d'environnement (jusqu'à la fin de la session) |
| `printenv [VAR]` | `printenv SHELL` | Liste les variables / valeur d'une variable |

## Script : variables et paramètres

| Formule générique | Exemple | Explication |
|---|---|---|
| `#!/bin/bash` | `#!/bin/bash` | 1re ligne d'un script |
| `$1 $2 …` | `./s.sh a b  donne  $1=a, $2=b` | Paramètres du script |
| `$#` | `./s.sh a b  donne  $#=2` | Nombre de paramètres |
| `"$@"` | `for p in "$@"` | Tous les paramètres |
| `$0` | `./s.sh` | Nom du script |
| `$?` | `ls ; echo $?` | Code retour de la dernière commande (0 = succès) |
| `variable=valeur` | `age=18  puis  echo $age` | Pas d'espace autour du = |
| `$(commande)` | `annee=$(date +%Y)` | Résultat d'une commande |
| `$((calcul))` | `echo $((4+7))` | Calcul entier |
| `i=$((i+1))` | `i=$((i+1))` | Incrémente i |
| `declare -i / -a / -r` | `declare -i n=12` | Entier / tableau / constante (-r) |
| `tab=(a b c)` | `fruits=(pomme poire kiwi)` | Déclare un tableau |
| `${tab[i]}` | `${fruits[0]}` | Élément d'indice i (commence à 0) |
| `${#tab[@]}` | `${#fruits[@]}` | Nombre d'éléments |
| `read -p "texte " var` | `read -p "Nom ? " nom` | Demande une saisie, stockée dans var |
| `$RANDOM` | `$((RANDOM % 100 + 1))` | Entier aléatoire (0 à 32767) : ici de 1 à 100 |
| `$USER  $EUID  $PWD  $OLDPWD` | `echo $USER` | Utilisateur / identifiant / dossier courant / dossier précédent |
| `$SECONDS` | `echo $SECONDS` | Secondes depuis le début du script |
| `$IFS` | `echo $IFS` | Séparateur utilisé pour découper une chaîne |
| `${!nom}` | `v=jeu ; n=v ; echo ${!n}` | Indirection : valeur de la variable dont le nom est dans n |
| `echo "calcul" \| bc` | `echo "1712 * 89 + 18" \| bc` | Calcul avec décimaux (bash seul : entiers) |

## Script : tests et alternatives

| Formule générique | Exemple | Explication |
|---|---|---|
| `if [ condition ]; then … fi` | `if [ $# -ne 2 ]; then echo "erreur"; exit 1; fi` | Alternative (espaces obligatoires autour des [ ]) |
| `if … elif … else … fi` | `if [ $1 -lt 10 ]; then … elif [ $1 -lt 12 ]; then … else … fi` | Plusieurs cas |
| `[ $a -eq $b ]` | `[ $1 -eq 0 ]` | Nombres : égal. Aussi -ne, -lt (<), -le, -gt (>), -ge |
| `[ "$a" = "$b" ]` | `[ "$1" = "oui" ]` | Texte : égal (!= différent). Pas -eq pour du texte |
| `[ -z "$a" ]  /  [ -n "$a" ]` | `[ -z "$1" ]` | Chaîne vide / non vide |
| `[ -d "$a" ]` | `[ -d "$1" ]` | Est un dossier. Aussi -f fichier, -e existe, -L lien, -r -w -x droits |
| `[ f1 -nt f2 ]` | `[ a.txt -nt b.txt ]` | f1 plus récent que f2 (-ot : plus ancien) |
| `[ ! cond ]  -a  -o` | `[ ! -d "$1" ]` | NON / ET / OU dans [ ] |
| `[[ a && b \|\| c ]]` | `[[ $a -gt 0 && $a -lt 10 ]]` | Version [[ ]] avec && et \|\| |
| `case $var in … esac` | `case $choix in 1) date ;; 2) whoami ;; *) echo "invalide" ;; esac` | Choix multiple (;; termine un cas, *) = par défaut) |

## Script : boucles et fonctions

| Formule générique | Exemple | Explication |
|---|---|---|
| `for i in {debut..fin}` | `for i in {1..10}; do echo $i; done` | Parcourt un intervalle |
| `for i in {debut..fin..pas}` | `for i in {2..10..2}; do echo $i; done` | Intervalle avec pas |
| `for (( i=0; i<N; i++ ))` | `for (( i=0; i<10; i++ )); do echo $i; done` | Boucle style C |
| `for p in "$@"` | `for p in "$@"; do echo "$p"; done` | Boucle sur les paramètres |
| `for x in a b c` | `for jour in lundi mardi mercredi; do echo $jour; done` | Boucle sur une liste |
| `while [ condition ]; do … done` | `while [ "$mot" != "fin" ]; do read -p "Mot ? " mot; done` | Tant que la condition est vraie |
| `while commande` | `while ls $dossier; do …; done` | Tant que la commande réussit |
| `nom() { … }` | `somme() { echo $(( $1 + $2 )); }` | Déclare une fonction (paramètres $1, $2) |
| `nom args` | `somme 18 24` | Appelle la fonction |
| `$(fonction args)` | `r=$(somme 18 24)` | Récupère ce que la fonction affiche |
| `return N  puis  $?` | `est_pair() { return 0; } ; est_pair 4 ; echo $?` | Valeur de retour numérique |
| `exit N` | `exit 1` | Arrête le script (0 = normal, autre = erreur) |

## Regex

| Formule générique | Exemple | Explication |
|---|---|---|
| `^motif$` | `^[0-9]{5}$` | ^ début, $ fin : tout le texte doit correspondre (ici code postal) |
| `[abc]  [a-z]  [0-9]` | `[a-zA-Z]` | Un caractère parmi l'ensemble |
| `[^0-9]` | `[^0-9]` | Un caractère qui n'est pas un chiffre |
| `.` | `a.c` | N'importe quel caractère |
| `\.` | `[0-9]+\.[0-9]+` | Un vrai point (hors crochets ; dans [ ] le point est déjà un vrai point) |
| `{n}  {n,m}  {n,}` | `[0-9]{1,3}` | Exactement n / de n à m / n ou plus |
| `+  *  ?` | `[0-9]+` | Au moins un / zéro ou plus / zéro ou un |
| `motif1\|motif2` | `jpg\|png` | Ou |
| `\w  \d` | `\d{4}` | Lettre / chiffre |
| `[[ "$var" =~ regex ]]` | `[[ "$1" =~ ^[0-9]{5}$ ]]` | Test de regex en bash |
| `Téléphone` | `^([0-9]{2} ){4}[0-9]{2}$` | 5 groupes de 2 chiffres séparés par un espace |
| `Date jj/mm/aaaa` | `^(0[1-9]\|[12][0-9]\|3[01])/(0[1-9]\|1[0-2])/[0-9]{4}$` | Jour 01-31, mois 01-12, année 4 chiffres |
| `E-mail` | `^[a-zA-Z0-9.]+@[a-zA-Z0-9]+\.[a-z]{2,}$` | Partie locale, @, domaine, vrai point, extension |
| `Adresse IP` | `^([0-9]{1,3}\.){3}[0-9]{1,3}$` | 4 groupes de 1 à 3 chiffres séparés par des points |

## Pièges à retenir

| Formule générique | Exemple | Explication |
|---|---|---|
| `Faux : cd..` | `Juste : cd ..` | Il faut un espace entre cd et .. |
| `Faux : [$1 -eq 0]` | `Juste : [ $1 -eq 0 ]` | Espaces obligatoires autour des crochets |
| `Faux : x = 5` | `Juste : x=5` | Pas d'espace autour du = dans une affectation |
| `Faux : [ "$1" -eq enzo ]` | `Juste : [ "$1" = "enzo" ]` | -eq pour les nombres, = pour le texte |
| `Faux : home/alice` | `Juste : /home/alice` | Un chemin absolu commence par / |
| `Faux : usermod -G sudo bob` | `Juste : usermod -aG sudo bob` | Sans -a, l'utilisateur perd ses autres groupes |
| `Faux : sudo echo "x" > f` | `Juste : echo "x" \| sudo tee f` | La redirection est faite par ton shell, pas par sudo |
| `Nouveau groupe` | `su - bob` | Pris en compte seulement après une nouvelle session |
| `Fichier créé avec sudo` | `sudo chown alice:alice f` | Appartient à root : le corriger avec chown |

