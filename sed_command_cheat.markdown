# sed

Substituer des mots:

    echo Bonjour Adrien | sed -e "s/Adrien/à toi/"
    
Substituer tous les mots d'un fichier (ici, tous les salut en bonjour) -i pour le fichier et le /g pour global (toutes les occurrences):

    sed -e "s/salut/bonjour/g" -i fichier.txt

Récupérer le chemin en cours et remplacer les espaces par un "antislash + espace":

    chem=$(pwd | sed -e "s/ /\\\ /g")

Ajouter le mot "Requires: " au début de chaque ligne du fichier codec, et garder le résultat dans le fichier codec2:

    sed -e 's/^/Requires: /g' codec > codec2

Ou en remplaçant le fichier actuel:

    sed -i -e 's/^/Requires: /g' codec

Remplacer dans tous les fichiers du répertoire courant azerty par qwerty:

    find . -name "*" -exec sed -i 's/azerty/qwerty/g' {} \;

Concaténer toutes les lignes d'un fichier :

    sed -e 'N;s/\n/ /' fichier

Ecrire au début d'un fichier:
 
    sed -i '1s/^/en haut\n/' fichier

Supprimer la première ligne d'un fichier:

    sed -i -e "1d" fichier

Ajoute toto à la ligne 27 de text.data:

    sed -i '27i\'"toto" text.data
