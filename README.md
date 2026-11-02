Ce projet met en place une calculatrice client-serveur avec des sockets TCP. Les clients envoient des opérations arithmétiques au serveur, qui calcule les résultats. La connexion utilise localhost et le port 1234.

Les opérations prises en charge sont l’addition (+), la soustraction (-), la multiplication (*) et la division entière (/). Les échanges utilisent la sérialisation d’objets Java : une opération est représentée par un objet Operation.

Deux variantes du serveur sont fournies :

- Server1 crée un thread de traitement pour chaque client.
- Server2 crée également un thread par client et affiche en plus le nombre total d’opérations traitées.


src
.
├── ClientPackage/
│   ├── Client1.java
│   └── Client2.java
├── ServerPackage/
│   ├── Server1.java
│   ├── Server2.java
│   ├── ClientHandler3_1.java
│   └── ClientHandler3_2.java
└── commun/
    └── Operation.java

Operation est une classe qui implémente Serializable et contient deux entiers (op1, op2) et un opérateur (operateur). Les classes de gestionnaire traitent les connexions des clients, séparément des classes principales des serveurs.

Prérequis

- Un JDK Java installé (javac et java disponibles dans le terminal).
- Le port TCP 1234 libre.
- Les sources rangées dans les dossiers correspondant aux packages.


Exécution

Lancez un seul serveur à la fois, car les deux serveurs utilisent le même port. Démarrez le serveur avant les clients. Le serveur reste actif et accepte plusieurs connexions ; chaque client reçoit un numéro et est traité dans son propre thread.

Utilisation

À l’invite Opération >, saisissez une expression comme :

text
55 * 25

Les espaces sont facultatifs, par exemple 25*25 ou 25 * 25. Pour quitter, saisissez 0 seul. Le client envoie alors un objet Operation spécial et ferme sa connexion.

Les clients vérifient la syntaxe et refusent la division par zéro avant d’envoyer l’opération. Chaque serveur lit les objets reçus, effectue le calcul, puis renvoie le résultat comme objet Integer. Server2 affiche aussi le total cumulé des opérations exécutées.

Détails à connaître

- La division est une division entière Java : 7 / 2 donne 3.
- Le serveur accepte plusieurs clients simultanément en créant un thread par connexion.
- Le 0 seul est réservé à la commande de fin ; une opération dont le premier opérande est zéro ne peut donc pas être envoyée par cette commande.
- Les classes de gestionnaire fournies exécutent directement la division. Elles comptent sur la validation du client pour éviter un diviseur nul.
- Le parseur montré dans les clients peut ne pas accepter correctement les entiers négatifs, car le signe - est également traité comme un opérateur lors du découpage de l’expression.
