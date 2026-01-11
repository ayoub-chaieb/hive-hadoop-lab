**TP: Hive**

**Prise en main avec Hive:**

Dans ce TP, vous explorerez Apache Hive, un système d’entrepôt de données distribué et tolérant aux pannes qui permet des analyses à grande échelle. Vous allez créer une table et exécuter des commandes SQL dessus.

**Objectifs d’apprentissage**

À la fin de ce laboratoire, vous serez en mesure de :

* Créer une table dans Hive

* Ajouter des données à la table à l’aide d’un fichier

* Ajouter des données à la table à l’aide de ***insert***

* Interroger les données de la table en utilisant des commandes SQL

**Étape 1 : Obtenez une copie du fichier CSV**

Créez un répertoire nommé data sous /home/project en exécutant la commande suivante.

\>\> mkdir /home/project/data

Changez de répertoire pour /home/project/data.

\>\> cd /home/project/data

Exécutez la commande suivante pour obtenir le emp.csv, un fichier de données contenant des informations sur les employés, dans un fichier séparé par des virgules que vous utiliserez plus tard pour alimenter les données dans la table que vous créez.

\>\>wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-BD0225EN-SkillsNetwork/data/emp.csv

Ouvrez le fichier dans l’éditeur et visualisez le fichier.

![][image1] 

**Étape 2 : Configurer Hive et Bee**

Vous utiliserez le hive du docker hub pour ce laboratoire. Téléchargez l’image hive sur votre système en exécutant la commande suivante.

\>\> docker pull apache/hive:4.0.0-alpha-1

![][image2]

*Cela prendra quelques secondes, en fonction de la vitesse de votre connexion Internet.*

Exécuter le serveur hive sur le port 10002. Nommerez l’instance du serveur myhiveserver. Nous allons monter le dossier local data dans le serveur hive sous le nom hive\_custom\_data. Cela signifie que l’ensemble du dossier data que vous avez créé localement, ainsi que tout ce que vous ajoutez dans le dossier de données, est copié dans le conteneur sous le répertoire hive\_custom\_data.

\>\> docker run \-d \-p 10000:10000 \-p 10002:10002 \--env SERVICE\_NAME=hiveserver2 \-v /home/project/data:/hive\_custom\_data \--name myhiveserver apache/hive:4.0.0-alpha-1

	![][image3]

Vous pouvez ouvrir et jeter un œil au serveur Hive avec l’interface graphique. Cliquez sur le bouton pour ouvrir l’interface graphique HiveServer2.

	\>\> localhost:10002

![][image4] Maintenant, exécutez la commande suivante, qui vous permet d’accéder à beeline. C’est une interface en ligne de commande SQL où vous pouvez créer, modifier, supprimer des tables et accéder aux données dans la table.

docker exec \-it myhiveserver beeline \-u 'jdbc:hive2://localhost:10000/'

![][image5]

**Étape 3 : Créer une table, ajouter et afficher des données**

Pour créer une nouvelle table Employee avec trois colonnes comme dans le fichier csv que vous avez téléchargé \- em\_id, emp\_name et salary, exécutez la commande suivante.

\>\> create table Employee(emp\_id string, emp\_name string, salary  int)  row format delimited fields terminated by ',' ;

![][image6]

*Vous remarquerez peut-être qu’il y a une mention explicite pour les champs délimités par , tout comme dans le fichier csv.*

Exécutez la commande suivante pour vérifier si la table est créée.

show tables;

![][image7]

Cela devrait lister la table Employé que vous venez de créer.

Maintenant, chargez les données dans la table à partir du fichier csv en exécutant la commande suivante.

\>\> LOAD DATA INPATH '/hive\_custom\_data/emp.csv' INTO TABLE Employee;

![][image8]

Exécutez la commande suivante pour lister toutes les lignes de la table afin de vérifier si les données ont été chargées à partir du CSV.

SELECT \* FROM employee;

![][image9]

Vous pouvez voir les détails des commandes et le résultat dans l’interface graphique de HiveServer2.

\>\> localhost:10002

![][image10]

Hive utilise en interne MapReduce pour traiter et analyser les données. Lorsque vous exécutez une requête Hive, elle génère des travaux MapReduce qui s’exécutent sur le cluster Hadoop.

**Conclusion**

Dans ce laboratoire, vous avez créé une table dans hive, ajouté des données à la table à partir de csv et listé les données contenues dans la table.

**TP2: Hadoop Map-Reduce**

**Objectives**

* Run a single-node Hadoop instance

* Perform a word count using Hadoop **Map Reduce**.

**Configurer Hadoop à nœud unique**

Les étapes décrites dans ce laboratoire utilisent la version 3.3.6 de Hadoop. **Hadoop** est le plus utile lorsqu’il est déployé en mode entièrement distribué sur un grand cluster de serveurs en réseau partageant un grand volume de données. Cependant, pour une compréhension de base, nous allons configurer Hadoop sur un seul nœud.

Dans ce laboratoire, nous allons exécuter l’exemple WordCount avec un texte d’entrée et voir comment le contenu du fichier d’entrée est traité par WordCount.

Téléchargez hadoop-3.2.3.tar.gz dans votre environnement theia en exécutant la commande suivante.

\>\> curl [https://dlcdn.apache.org/hadoop/common/hadoop-3.3.6/hadoop-3.3.6.tar.gz \--output hadoop-3.3.6.tar.gz](https://dlcdn.apache.org/hadoop/common/hadoop-3.3.6/hadoop-3.3.6.tar.gz%20--output%20hadoop-3.3.6.tar.gz)

![][image11]

Extraire le fichier tar dans le répertoire actuel.

\>\> tar \-xvf hadoop-3.3.6.tar.gz

![][image12]

Accédez au répertoire hadoop-3.3.6.

\>\>cd hadoop-3.3.6

Vérifiez la commande hadoop pour voir si elle est configurée. Cela affichera la documentation d’utilisation du script hadoop.

\>\> bin/Hadoop

![][image13]

Exécutez la commande suivante pour télécharger data.txt dans votre répertoire actuel.

\>\> curl https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-BD0225EN-SkillsNetwork/labs/data/data.txt \--output data.txt![][image14]

Exécutez l’application Map reduce pour le comptage de mots sur data.txt et stockez la sortie dans **/user/root/output**

\>\> bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.6.jar wordcount data.txt output

![][image15] *Cela peut prendre un certain temps.*

8. Une fois que le compteur de mots a fonctionné avec succès, vous pouvez exécuter la commande suivante pour voir le fichier de sortie qu’il a généré.

   \>\> ls output

![][image16]

Vous devriez voir **part-r-00000** avec **\_SUCCESS** indiquant que le comptage des mots a été effectué.

*Alors qu’il est encore en cours de traitement, vous ne verrez peut-être que ‘\_temporary’ répertorié dans le répertoire de sortie. Attendez quelques minutes et exécutez à nouveau la commande jusqu’à ce que vous voyiez une sortie comme celle indiquée ci-dessus.*

Exécutez la commande suivante pour voir la sortie du comptage des mots.

\>\> cat  output/part-r-00000

![Sortie de comptage de mots locale][image17]

**Example 2:**

Faites un compte de mots sur un fichier contenant le texte suivant.

Italy Venice

Italy Pizza

Pizza Pasta Gelato

Cliquez ici pour un indice sur la façon de créer un fichier pour le comptage de motsCréez data.txt avec le contenu requis. Vous pouvez utiliser l'éditeur de fichiers.Cliquez ici pour la solution sur la façon de faire le comptage de mots sur le fichierExécutez la commande suivante

\>\> bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.6.jar wordcount data.txt output

Cliquez ici pour un exemple de sortie

La sortie sera comme ci-dessous.

![][image18]

L’image ci-dessous montre comment le comptage de mots MapReduce se déroule.

![][image19]

[image1]: images/1.png
[image2]: images/2.png
[image3]: images/3.png
[image4]: images/4.png
[image5]: images/5.png
[image6]: images/6.png
[image7]: images/7.png
[image8]: images/8.png
[image9]: images/9.png
[image10]: images/10.png
[image11]: images/11.png
[image12]: images/12.png
[image13]: images/13.png
[image14]: images/14.png
[image15]: images/15.png
[image16]: images/16.png
[image17]: images/17.png
[image18]: images/18.png
[image19]: images/19.png
