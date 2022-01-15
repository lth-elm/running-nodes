# TD 4 Monnaie Numérique

1. [SSH setup (2 pts)](#ssh)
2. [UFW config (2 pts)](#ufw)
3. [Installing dependencies (2 pts)](#dependency)
    1. [GO](#go)
    2. [Java et Libsodium](#javalibsodium)
    3. [NodeJS et Truffle](#nodejstruffle)
4. [Installing Quorum (2 pts)](#quorum)
5. [Connect Geth to our group's private network (2 pts)](#connectgethprivate)
6. [Deploy a contract (2 pts)](#deployment)
    1. [Installations](#installations)
    2. [Projet Truffle](#truffleproject)
7. [Installing Tessera (2 pts)](#tessera)
    1. [Java JDK](#javajdk)
    2. [Libsodium (optionel)](#libsodium)
    3. [Installation Tessera](#tesserainstall)
8. [Configure Tessera (3 pts)](#tesseraconfig)
9. [Create a private smart contract with XXX](#privatesmartcontract)
        

## SSH setup (2 pts) <a name="ssh"></a>

Pour cette étape il suffit de reproduire à l'identique ce qui a déjà été fait aux TD précédents et notamment au 1.

## UFW config (2 pts) <a name="ufw"></a>

Pour le moment nous avons autorisé que le port 22 pour ssh, si besoin nous ferons d'autres adaptations.

## Installing dependencies (2 pts) <a name="dependency"></a>

### GO <a name="go"></a>

Dans un premier temps il faut installer le langage GO. 

L'installation la plus sipmle que nous ayons trouvé est la suivante :

```shell
sudo add-apt-repository ppa:longsleep/golang-backports
sudo apt update
sudo apt install golang-go
```
Neanmoins il semblerait que ce n'est pas une installation officielle, mais elle a fonctionné pour nous.

Pour plus de détails voir : https://github.com/golang/go/wiki/Ubuntu

### Java et Libsodium <a name="javalibsodium"></a>

Pour l'installation des dépendances ***Java*** et ***Libsodium*** voir la partie sur l'installation de [Tessera](#tessera).

### NodeJS et Truffle <a name="nodejstruffle"></a>

Pour ***NodeJS*** et ***Truffle*** voir le [déploiement d'un contrat](#deployment).

## Installing Quorum (2 pts) <a name="quorum"></a>

Pour insatller Quorum on suit les instructions présentes dans le fichier `BUILDING.md` présent dans le dossier `quorum` de `quorum-21.7.1.tar.gz`

![build quorum](https://user-images.githubusercontent.com/62909821/136551001-7e422ddb-4c4f-4803-b314-381f7ab94f23.PNG)

Pour vérifier que tout est bien installer on lance la commande : 

```shell
export PATH=$PATH:/home/administrateur1/quorum/build/bin
geth version 
```

Si tout s'est déroulé correctement on obtient quelque chose du style :

![geth version path](https://user-images.githubusercontent.com/62909821/136555434-71d166fc-0d3f-4295-ace3-12961495a673.PNG)

On place la commande ci dessus débutant avec "export" dans le dossier `~/.bashrc` et `~/.bash_profile` pour pouvoir lancer la commande `geth` a tout moment.

## Connect Geth to our group's private network (2 pts) <a name="connectgethprivate"></a>

Pour initialiser geth avec notre fichier genesis on crée un dossier *ibftInit/* dans lequel on va créer et remplir **genesis.json**. On revient ensuite à la racine et on entre la commande suivante ``geth init --datadir data init ibftInit/genesis.json`` qui va alors créer un nouveau dossier *data/*. On observe qu'on obtient bien le résultat attendu avec la bonne database 'lightchaindata' et le bon hash.

![init genesis](https://user-images.githubusercontent.com/62894505/136674756-55a2b797-a196-4900-9748-ab75450bf81f.png)

on viendra mettre dans ce nouveau dossier *data/* le **static-nodes.json**.

Après s'être replacé un dossier avant la commande suivante à lancer est ``PRIVATE_CONFIG=ignore geth --datadir "./data"``. Il est recommandé de retirer *'nohup'* si cette commande est présente, en effet, elle empêche l'affichage des logs et on ne peut pas savoir tout de suite si nous nous sommes correctement connecté. Si tout se passe bien voilà ce qui devrait s'afficher :

![connection logs 1](https://user-images.githubusercontent.com/62894505/136674724-a0e6ce01-173c-484f-9e88-968cab28bf68.png)

![connection logs 2](https://user-images.githubusercontent.com/62894505/136674764-c74fd5ae-f584-4257-86c5-42b663cba10c.png)

Pour continuer il va falloir ouvrir une nouvelle console.

## Deploy a contract (2 pts) <a name="deployment"></a>

### Installations <a name="installations"></a>

Pour déployer le contrat nous aurons besoin de ***Truffle***, or pour cela nous aurons besoin de ***NodeJS*** que nous allons commencer par installer de la sorte.

```shell
curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo npm install -g express
```

Une fois que ceci est fait nous pouvons installer Truffle avec ``sudo npm install -g truffle``.

Eventuellement on peut aussi installer *testrpc* avec ``sudo npm install -g ethereumjs-testrpc`` pour nos simulation.

![testrpc](https://user-images.githubusercontent.com/62894505/137341026-6d6b48dd-47ac-4a40-ac98-3c3e2a4f1390.PNG)

### Projet Truffle <a name="truffleproject"></a>

Nous allons maintenant créer notre projet truffle et l'initialiser en effectuant l'enchainement des commandes suivantes :

```shell
mkdir mytestproject
cd mytestprojet/
truffle init
```

![init_truffle_project](https://user-images.githubusercontent.com/62894505/137341507-96d8bd7c-c1a8-45cd-ae7d-9b37771817c0.PNG)

On observe que les fichiers ont bien été générés et notamment le *truffle-config.js* dans lequel il va falloir renseigner les infos de notre réseau privé sur lequel nous souhaitons deployer notre contrat.

Une fois que cette étape sera faite il faudra compiler avec ``truffle compile`` puis déployer/migrer avec ``truffle migrate``.

On se rend ensuite dans le dossier contracts `cd contracts`

Ensuite on crée un contrat très simple `hello.sol`

```shell
touch hello.sol

#On ouvre le fichier

nano hello.sol
```

puis on y ajoute le code suivant :

```solidity
pragma solidity ^0.5.0;

contract Hello {
  string public message;

  function setMessage(string memory initialMessage) public {
    message = initialMessage;
  }
}
```

## Installing Tessera (2 pts) <a name="tessera"></a>

Avant de pouvoir installer Tessera, il faut installer les dépendences : Java JDK et libsodium (optionel)

### Java JDK <a name="javajdk"></a>

On s'assure que Java n'est pas présent en faisant `java -version`
Si Java est bel est bien absent on installe Java dans un premier temps :

```shell
sudo apt update
sudo apt install default-jre

#Pour vérifier que Java est bien installé
java -version
```

Une fois que Java est installé sur notre machine distante, on installe Java JDK :

```shell
sudo apt install default-jdk

#Pour vérifier que Java JDK est bien installé
javac -version
```

![java](https://user-images.githubusercontent.com/62894505/137338245-9b962933-a6a3-4502-8442-5b22faabb267.png)

### Libsodium (optionel) <a name="libsodium"></a>

```shell
wget https://download.libsodium.org/libsodium/releases/LATEST.tar.gz
sudo tar xvzf LATEST.targ.gz
cd libsodium-latest
sudo ./configure
sudo make && make check
sudo make install
```

### Installation Tessera <a name="tesserainstall"></a>

```shell
wget https://codeload.github.com/ConsenSys/tessera/tar.gz/refs/tags/tessera-21.10.0
tar xvf tessera-21.10.0
```

## Configure Tessera (3 pts) <a name="tesseraconfig"></a>

## Create a private smart contract with XXX <a name="privatesmartcontract"></a>

