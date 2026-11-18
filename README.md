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

