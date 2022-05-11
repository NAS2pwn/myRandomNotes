Titre : Hacker Way: Rethinking Web App Development at Facebook
Lien : https://www.youtube.com/watch?v=nYkdrAPrdcw

---

# L'architecture flux de facebook

En gros facebook a initialement été conçu sur le fameux modèle MVC

![mvc](https://raw.githubusercontent.com/NAS2pwn/myRandomNotes/main/assets/mvc.png)

C'est-à-dire que les différents éléments de la vue appellent différentes parties du modèle et vice-versa.

Dans ces spaghettis d'appels, à mesure où on augmente le nombre d'éléments du site web, on peut facilement se retrouver avec des cycles (au sens graphe), la vue qui appelle le modèle qui rappelle la vue à l'infini...

![mvc-bordel](https://raw.githubusercontent.com/NAS2pwn/myRandomNotes/main/assets/mvc-bordel.png)

L'idée pour éviter ça c'est d'avoir une instance unique dans le navigateur qui traite toutes les actions de la page (le dispatcher), et une source de données unique sur le navigateur qui met à jour une vue unique à chaque changement : c'est l'architecture flux.

![flux](https://raw.githubusercontent.com/NAS2pwn/myRandomNotes/main/assets/flux.png)

![flux-action](https://raw.githubusercontent.com/NAS2pwn/myRandomNotes/main/assets/flux-action.png)

Avec cette solution on se retrouve avec des données consistantes, car mise à jour proprement et de manière prévisible, et avec un code débugguable plus facilement, car on peut remonter à la source des bugs plus facilement.


Prenons un cas concret : celui du chat facebook.

Son développement a été assez chaotique à mesure que facebook prenait en ampleur, alors qu'au début le code était simple, il a fini par être un cas d'usage du MVC spaghetti dont on a parlé.

Au début le code était assez simple : on avait une fonction qui, à chaque fois qu'on recevait un message, ajoutait ce-dit message dans l'onglet de chat, incrémentait le compteur de messages non lus, puis décrémentait le compteur dès lors qu'on accédait au message. C'était simple et fonctionnel.

![chat-begin](https://raw.githubusercontent.com/NAS2pwn/myRandomNotes/main/assets/chat-begin.png)

Puis il a fallu créer une page de chat en plus des petits onglets en bas de l'écran, et là c'est le début des emmerdes.

Il a fallu à la fois gérer le compteur à partir de l'onglet de chat, et à partir de la page de chat : on s'est retrouvé dans un cas de MVC spaghetti.

Y avait cette foutue notification de message non lu qui poppait, puis dépoppait quand on lisait le message, et reppopait alors qu'on avait déjà lu le message : bonne année 2015 mes shegueys.

![chat-double-view](https://raw.githubusercontent.com/NAS2pwn/myRandomNotes/main/assets/chat-double-view.png)

Comme on le voit plus haut, on n'avait qu'un seul handler qui gérait les trois éléments de la vue: le menu de message, le petit cadre de chat, et le compteur de messages 

![external-control](https://raw.githubusercontent.com/NAS2pwn/myRandomNotes/main/assets/external-control.png)

L'idée pour appliquer l'architecture FLUX ça a tout d'abord été d'internaliser ce contrôle, de rapprocher l'état de la vue de la logique qui régit cet état : de créer des fonctions propres à chaque élément de la vue en somme.

Pour les onglets de chat et pour la page de chat, c'est assez facile à gérer : la seule logique qu'on a c'est d'ajouter le message à chaque nouveau message reçu. La logique est claire.

Le problème est au niveau du compteur de message non lus : si on sait l'incrémenter à chaque message reçu, comment bien le décrémente à chaque message lu ?

![internal-control](https://raw.githubusercontent.com/NAS2pwn/myRandomNotes/main/assets/internal-control.png)

L'idée ça a été de remplacer le compteur de messages non lus par une liste de messages non lus, d'ajouter les messages reçus à la listes quand un nouveau message est reçu, et de l'en retirer quand un message est lu, grâce à une nouvelle action (qui passe également par le dispatcher).

![internal-control-threads](https://raw.githubusercontent.com/NAS2pwn/myRandomNotes/main/assets/internal-control-threads.png)

A ce stade de la vidéo, il y a trois leçons à retenir :
- D'abord on doit utiliser des données explicites plutôt que des données dérivées : c'est-à-dire qu'au lieu de compter les messages non lus, on préferera stocker les messages non lus et les compter. C'est ce qui a motivé la naissance des propriétés computed dans react et vue.
- Il faut séparer les données de l'état de la vue
- 


