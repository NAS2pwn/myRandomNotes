La couche réseaux ça sert à transporter des paquets de la source à la destination, en passant par des intermediaires, avec plusieurs sauts, parfois en leur faisant traverser des continents... à l'inverse de la couche liaison de données qui a pour simple but de bouger des trames entre différents hôtes sur un même support (peut-on considérer que deux machines connectées sur le même VLAN par un switch sont sur un même support physique au même titre qu'un hub ?).

Pour ce faire, la couche réseaux a besoin de connaître la topologie du réseau, et de choisir le meilleur chemin pour acheminer les paquets qu'elle a besoin d'envoyer. Elle doit également faire attention à ne pas surcharger certaines lignes de communication tandis que d'autres lignes sont en idle.

La couche réseaux fournit les services de la couche de transport à la couche de liaison de données. C'est également cette couche qui fait l'intérmediaire entre les réseaux du fournisseur de service et le sous-réseau du client, qui sont de nature très différentes (une dorsale Internet n'utilise pas les mêmes technos ni les mêmes protocoles qu'un réseau d'entreprise). Le travail du fournisseur est simplement de livrer les paquets depuis le réseau de l'utilisateur jusqu'à un hôte connu via une boîte noire d'un point de vue client.

Les services de la couche réseau ont été conçu en respectant trois principes fondamentaux :

1. Elle doit être indépendante des technologies utilisées pour les sous-réseaux
2. La couche transport ne doit pas avoir à gérer l'adresse, le type et la topologie des sous-réseaux (TCP doit aussi bien pouvoir passer sur un réseau qui tourne sur Ethernet qu'un réseau qui utilise IP over ATM)
3. Les adresses réseau rendues disponibles à la couche transport doivent avoir une nomenclature uniforme (les adresses IP en gros)

Du moment qu'ils respectent ces principes, les designers de la couche réseaux ont énormément de liberté quand à la spécification des services développés.

Cette liberté a débouché sur une gueguerre entre deux grandes factions (pour caricaturer) : ceux qui pensent que la couche réseau doit être sans connexion, et ceux qui pensent qu'il vaut mieux qu'elle soit avec connexion.

Le premier camp est la communauté Internet qui considère que le rôle des sous-réseaux n'est que de transporter des bits, rien de plus, et que les sous-réseaux sont intrinsèquement non fiable (unreliable), peu importe leur conception... C'est pourquoi les machines aux extrémités doivent-elles même faire leur contrôle de flux et d'erreur.

C'est en partie pour ça qu'on dit que dans Internet l'intelligence se situe aux extrêmités. Cela a été rendu possible par l'augmentation drastique des performances des machines, et l'organisation "open-source" qui a permis la constitution de ce que Soriano appellerait une "tribu" pour la conception d'Internet. On est très loin des réseaux bancals à faible fiabilité du support ET faible puissance de calcul des hôtes qui avaient nécessité le déploiement du protocole frame relay.

Les designers d'Internet sont vite arrivés à la conclusion que dans ces conditions il faut privilégie une couche réseau sans connexion, avec seulement les primitives SEND PACKETS pour fournir les paquets à la couche transport (qui gère la reprise sur erreur s'il y en a), et RECEIVE PACKETS pour construire les paquets à partir des trames fournies par la couche liaison de données.

Dans cette conception des choses, il n'y a évidemment pas d'ordonnancement des paquets (contrairement à X.25 et ATM).

L'autre camp défend que les sous-réseaux devraient fournir un service un minimum fiable, avec connexion (les fameux opérateurs historiques lol).

Pour ce faire, ils proposent quatre principes fondateurs :

1. Avant d'envoyer de la donnée, la couche réseau du côté de l'émetteur doit mettre en place un circuit avec le destinataire. Ce circuit est "numéroté", et utilisé pour envoyer les données jusqu'à la fin explicite de la communication. Cette stratégie, issue des réseaux téléphoniques, avait pour but d'éviter la gigue lors des appels téléphoniques grâce à la possibilité d'assurer un circuit constant puisque passant par le même chemin tout au long de la communication.
2. Quand un connexion est initialisée, le deux processus aux extrémités peuvent entrer dans une négociation quant à la qualité de service et au coût de la communication à suivre. Ca peut être un débit constant comme dans le contexte des appels téléphoniques, ou beaucoup d'autres configurations (voir les contrats ATM).
3. La communication est bidirectionnelle, et les paquets sont séquencés.
4. Le contrôle de flux est fourni niveau réseau pour éviter qu'un émetteur trop rapide ne submerge un destinataire un peu plus lent, et ne cause des overflow (donc des pertes de paquet). Dans le paradigme Internet TCP/IP, ce contrôle de flux est fourni par la couche transport, par les fameux algo tahoe, Reno, new Reno (en UDP ràf)...

La grande différence entre ces deux paradigmes est l'endroit où se situe la complexité du réseau. Dans le paradigme avec connexion, elle est mise dans la couche réseau, c'est à dire dans les équipements réseau, tandis que dans le paradigme sans connexion elle est mise dans les hôtes

blablabla finir

La pile TCP/IP a rendu obsolète les technologies avec connexions, ATM principalement. Pour interconnecter les machines il faut forcément 





