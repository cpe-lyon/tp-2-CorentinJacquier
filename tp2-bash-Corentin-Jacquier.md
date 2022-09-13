# 👨‍💻 TP2 - Bash Ubuntu

CPE Lyon - 4ETI, 3IRC & 3ICS - Année 2022/2023 Administration Système

Corentin JACQUIER ICS

## Exercice 1 - Variables d'environnement

C'est le fichier `.bash_history` qui contient l'historique des commandes tapées par l'utilisateur. On peut voir le chemin d'accès au répertoire courant avec la commande `pwd` qui donne `/home/User` et on peut voir le contenu du fichier avec la commande `cat .bash_history`.

La commande `cd` permet de revenir dans le répertoire personnel grâce à la variable d'environnement `$HOME`.

Tout d'abord, on peut voir le contenu des variables avec la commande `printenv [$VAR]`. 

Alors, pour `LANG` affiche la langue utilisée par le terminal (en_US) et le langage utilisé (UTF-8). 

Ensuite la variable `PWD`, contient le répertoire courant. Et `OLDPWD` contient le répertoire courant précédent. 

Enfin, la variable d'environnement `SHELL` retourne le répertoire du dossier 'bash'.

On fait la variable locale `MY_VAR` avec la commande `MY_VAR="tores"`.

On affiche le contenu de celle-ci avec `printenv MY_VAR`. 



La commande `bash` permet de changer de session, et donc la variable précédente n'existe plus car celle-ci à été en créer en locale. 

On fait la variable d'environnement `MY_VAR` avec la commande `export MY_VAR="tores"`.

Avec la commande `bash`, on peut alors voir la variable d'environnement  `MY_VAR` (globale pour toutes les sessions). 

`exit` permet de revenir à la session initiale. 

On fait une variable d'environnement `NOM` avec `export NOM="Corentin Jacquier"` puis on affiche celle-ci avec `printenv`.  

<img src="https://media.discordapp.net/attachments/1017478318934724638/1018785508370939904/unknown.png">

La variable d'environnement vide possède un caractère vide alors que si on fait `unset` la variable d'environnement n'existe plus. 

On écrit `echo $HOME=/home/User` qui retourne `/home/User=/home/User` le chemin d'accès au dossier personnel d'après bash. 

##  Programmation Bash

### Exercice 2 - Contrôle de mot de passe

On code le script suivant : 

```bash
#!/bin/bash

PASSWORD="1234"
read -s -p "Mot de passe : " pwd

if [ _$pwd = _$PASSWORD ];
then
echo -e "Mot de passe bon \n"
else
echo -e "Mot de passe erroné \n"
fi
```

On a `_` devant les variable dans le `if`, permet de s'assurer que si il y a des valeurs vide le test marche encore. 

Les `-e` permettent d'interpréte `\n` pour sauter un ligne. 

On donne les permissions au script avec `chmod u+x testpwd.sh` pour l'exécuter avec la commande `./testpwd.sh`. 

<img src="https://media.discordapp.net/attachments/1017478318934724638/1018803061700968458/unknown.png">

### Exercice 3 -  Expressions rationnelles

On écrit le script suivant : 

```bash 
#!/bin/bash

read -p "Entrez paramètre : " n

function is_number() {
re='^[+-]?[0-9]+([.][0-9]+)?$'
if ! [[ $n =~ $re ]] ; then
	echo "N est réel"
	return 1
else
	echo "N n'est pas réel"
	return 0
fi
}

is_number
```

On donne les permissions au script avec `chmod u+x is_number.sh` pour l'exécuter avec la commande `./is_number.sh`. 

<img src="https://media.discordapp.net/attachments/1017478318934724638/1018803461548154890/unknown.png">

### Exercice 4 - Contrôle d’utilisateur

On ajoute le script suivant : 

```bash
#!/bin/bash

if [ $# -eq 0 ]
then
    echo "Guide : $0 nom_utilisateur"
else
    if [ -z $(getent passwd $1) ]
    then
        echo "$1 n'est pas un utilisateur"
    else
        echo "$1 est un utilisateur"
    fi
fi
```

On donne les permissions au script avec `chmod u+x coco.sh` pour l'exécuter avec la commande `./coco.sh`. 

<img src="https://media.discordapp.net/attachments/1017478318934724638/1018806900877434930/unknown.png">

### Exercice 5 - Factorielle

On écrit le script suivant : 

```bash
#!/bin/bash

read -p "Entrez nombre : " n
TEMP=1

for i in $(seq 1 $n)
do
        TEMP=$((TEMP*i))
done
echo "$TEMP"
```

On donne les permissions au script avec `chmod u+x facto.sh` pour l'exécuter avec la commande `./facto.sh`. 

<img src="https://media.discordapp.net/attachments/1017478318934724638/1018815399489450035/unknown.png">

### Exercice 6 - Le juste prix

On code le script suivant : 

```bash
#!/bin/bash

nombre=$(($RANDOM % 1000 + 1))

echo "Devinez le nombre entre 1 et 1000"
read reponse

while [ $reponse -ne $nombre ]
do
    if [ $reponse -lt $nombre ]
    then
        echo "C'est plus !"
    else
        echo "C'est moins !"
    fi
    read reponse
done

echo "Gagné !"
```

On a `-ne` pour non-equal (!=) et `lt` pour less-than.

On donne les permissions au script avec `chmod u+x facto.sh` pour l'exécuter avec la commande `./facto.sh`. 

<img src="https://media.discordapp.net/attachments/1017478318934724638/1018817686370799647/unknown.png">

## Exercice 7 - Statistiques 

On écrit le script suivant : 

```bash
#!/bin/bash

if [ $# -ne 3 ]; 
then
	echo "Guide: $0 n1 n2 n3"
	exit 1
fi

for i in $@; 
do
	if [ $i -lt -100 -o $i -gt 100 ]; 
		then
		echo "Erreur: $i n'est pas entre -100 et +100"
		exit 1
	fi
done

min=$1
max=$1
sum=0

for i in $@; 
do
	if [ $i -lt $min ]; 
		then
		min=$i
	fi
	if [ $i -gt $max ]; 
	then
		max=$i
	fi
sum=$((sum + i))
done

echo "Mini: $min"
echo "Maxi: $max"
echo "Moyenne: $((sum / 3))"
```

On donne les permissions au script avec `chmod u+x stat.sh` pour l'exécuter avec la commande `./stat.sh`. 

<img src="https://media.discordapp.net/attachments/1017478318934724638/1018820474949615678/unknown.png">

Pour avoir n quelconque de paramètres, on met en commentaire le premier `if` avec `##` :

```bash 
##if [ $# -ne 6 ]; 
##then
##      echo "Guide: $0 n1 n2 n3 n4 n5"
##      exit 1
##fi
``` 
Alors, on peut exécuter le script avec n paramètres. 

<img src="https://media.discordapp.net/attachments/1017478318934724638/1019151730241458247/unknown.png?width=1692&height=380">


Enfin, pour mettre les valeurs dans un tableau en demandant les valuers à l'utilisateur, on a alors le code suivant :

```bash
  

#!/bin/bash

# Initialisation des variables
declare -a notes
declare -i i=0
declare -i min=100
declare -i max=-100
declare -i moyenne=0
declare -i somme=0

# Saisie des notes
while  true
	do
	read -p "Saisir note : " input
	if [ $input -ge -100 ] && [ $input -le 100 ] && [[ $input == ?(-)+([0-9]) ]];
	then
		notes[$i]=$input
		i=$i+1
	elif [[ $input = "q" ]] || [[ $input = "Q" ]];
	then
		break
	else
		echo  "La note doit être comprise entre -100 et 100 / Entrer sur q/Q pour quitter"
	fi
done

# Calcul des notes
for  note  in  ${notes[*]}
	do
	if [ $note -lt $min ]
	then
		min=$note
	fi
	if [ $note -gt $max ]
	then
		max=$note
	fi
	somme=$somme+$note
done
moyenne=$somme/10


# Affichage des résultats
echo  "La note minimale est $min"
echo  "La note maximale est $max"
echo  "La moyenne est $moyenne"
```

Au lieu d'utiliser un regex, on utilise le Pattern-Matching de bash.

On donne les permissions au script avec `chmod u+x stat2.sh` pour l'exécuter avec la commande `./stat2.sh`. 

<img src="https://media.discordapp.net/attachments/1017478318934724638/1019188488840691773/unknown.png">
