# TD-3-Monnaie-Numerique

## Install UFW 

On procède de la même manière que les TD précédents

## Installing dependencies

Nous avons supposé que nous aurions besoin de Javascript pour la suite du TD, donc d'entrée de jeu nous avons décidé d'installer node.js et npm via les commande :
* `sudo apt install nodejs`
* `sudo apt install npm`

![version node et npm](https://user-images.githubusercontent.com/62909821/135616740-6606814c-a453-47ee-810e-2b1927c096a7.PNG)


Avant de télécharger geth via PPAs il était recomandé de lancer la commande : `sudo apt-get install software-properties-common`. Ce logiciel fournit une abstraction des référentiels apt utilisés.

## Installing Geth 

Pour l'installation de Geth nous avons suivi le tutoriel suivant : https://developernote.com/2020/11/a-proper-ubuntu-18-04-installation-for-running-ethereum-node/ .
Ansi nous avons lancé le script suivant :

```shell
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt-get update
sudo apt-get install ethereum
```

Finalement nous avons pu vérifier que l'intallation s'était bien déroulée avec la commande `geth -h` qui nous a renvoyé :

![geth version](https://user-images.githubusercontent.com/62909821/135616717-c7e50aff-9ec2-4cb2-8be8-a8c562a345a2.PNG)

## Running Geth on the rinkeby network

Pour lancer Geth sur le réseau nous avons seulement lancé la commande : `geth --rinkeby`.
Cette commande utilise le mode de synchronisation "Snap" qui est le mode de synchronisation par défaut.

![sync mode](https://user-images.githubusercontent.com/62909821/135617574-fe56b0ae-4806-42e5-a477-1b8c5d67d6ce.PNG)


## Turning Geth into a service 

On commence par créer un fichier de configuration système *"geth.service"*, ``sudo nano /etc/systemd/system/geth.service`` dans lequel on insère les informations suivantes :

```
[Unit]
Description=Ethereum go client
[Service]
Type=simple
ExecStart=geth --rinkeby
Restart=on-failure
RestartSec=5
[Install]
WantedBy=default.target
```

Puis autoriser le service : ``sudo systemctl --user enable geth.service`` et le lancer ``systemctl --user start geth.service`` (s'assurer avant que geth ne tourne pas en fond).

## Open the RPC API to interact with your node 

## Connect to the Geth console and extract last block number

Dans le dossier caché `.ethereum` se trouve un fichier `geth.ipc`. Ce fichier peut être contenu dans un autre dossier lui même dans le dossier `.ethereum` si la synchronisation n'a pas été faite sur le mainet. Dans notre cas nous avons fait notre syncrhonisation sur le testnet Rinkeby, donc le fichier geth.ipc se trouve dans le dossier `.ethereum/rinkeby/`.

Pour ouvrir la console javascript nous utilisons la commande : `geth attach .ethereum/rinkeby/geth.ipc`.

Enfin pour afficher le numéro du dernier block il faut lancer la commande `eth.blockNumber` dans la console

Si la chaine est en cours de synchronisation il est également possible d'utiliser la commande `eth.syncing` qui affiche le dernier block mais également le dernier block synchronisé.

![eth syncing](https://user-images.githubusercontent.com/62909821/135761643-4830151f-2da8-413a-9aba-2107d3f3ecfb.PNG)

## Connect to the Geth console and show events data from a specific transaction 

Il est possible de retrouver une transaction spécifiques et les informations relatives grâce au hash de transaction et avec la console geth.
Pour cela il faut une transaction faisant partie des blocks déjà synchronisé sur la VM et on utilise la commande `eth.getTransation('<hash_de_la_transaction>')`

![eth get transaction](https://user-images.githubusercontent.com/62909821/136461879-888739cf-4c89-43a8-96d1-0e840698d51b.PNG)

Il existe plusieurs autres méthode que l'on peut utiliser grâce à l'API web3. Ces méthodes sont :

![fonctions eth](https://user-images.githubusercontent.com/62909821/136462280-8cf524e0-a4d2-4f07-ab16-6860f27f802f.PNG)

## Configure truffle to deploy a smart contract through your node (2 pts)

## Ressources

* https://geth.ethereum.org/docs/install-and-build/installing-geth#install-on-ubuntu-via-ppas
* https://priyalwalpita.medium.com/setting-up-the-go-ethereum-geth-environment-in-ubuntu-linux-67c09706b42
* https://developernote.com/2020/11/a-proper-ubuntu-18-04-installation-for-running-ethereum-node/
* Pour ouvrir la console Javascript : https://ethereum.org/uk/developers/tutorials/run-light-node-geth/

tunnel ssh machine -> vm pour pouvoir se connecter sur le port 8085
