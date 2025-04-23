---
title: "Open Video Downloader - Télécharger une vidéo YouTube"
---
{% include toc.html %}

# Introduction

Une demande assez courante que je reçois, c’est “Tu sais comment on télécharge une vidéo sur YouTube ?”

Et pour répondre simplement à cette question, oui, je sais comment faire !

Je tacherai d’être compréhensible, mais je pense que ça peut être tout à fait faisable.

# Open Video Downloader

## Présentation générale

C’est un logiciel open source qui fonctionne sous Windows, macOS, Linux. Il vous permettra de télécharger simplement des vidéos en provenance de YouTube, mais également Vimeo, Twitter, etc.

Il est possible de télécharger uniquement l’audio d’une vidéo ou même une playlist entière. 

Il y a bien sûr moyen de choisir la qualité de la vidéo, mais j’imagine que c’est une évidence pour ce type de logiciel. 

Le seul hic, c’est que le logiciel est uniquement en anglais

## Télécharger le logiciel
<p class="encart"> 
📥Pour installer le logiciel, il suffit de se rendre sur ce lien 

<a href="https://github.com/StefanLobbenmeier/youtube-dl-gui" class="bouton"> https://github.com/StefanLobbenmeier/youtube-dl-gui </a>
</p>

## Mais du coup comment je fait moi ?

Parfois, une image vaut mieux que mille mots (même si cet article ne fait sûrement pas mille mots). 

![Petit GIF qui montre la démarche](https://github.com/jely2002/youtube-dl-gui/raw/master/ytdlgui_demo.gif)
_Petit GIF qui montre la démarche_

Bon après, quelques étapes écrites ça fait pas de mal. 

- Tu récupères l’URL de ta vidéo. Ça ressemble un peu à ça : [https://www.youtube.com/watch?v=dQw4w9WgXcQ](https://www.youtube.com/watch?v=dQw4w9WgXcQ).
- Soit le lien apparaîtra automatiquement dans la barre supérieure ou alors il suffit de coller le lien.
- Clique sur le petit +, pour rajouter l’audio/vidéo à la liste d’attente.
- En bas, le petit dossier permet de sélectionner l’endroit où tu veux stocker la vidéo ou l’audio que tu veux télécharger.
- Tu peux choisir avec le menu déroulant si tu veux l’audio et la vidéo ou que l’audio.
- Juste à côté, tu peux choisir la qualité vidéo
- La petite poubelle permet de supprimer la file d’attente au-dessus.
- Et enfin, tu peux cliquer sur le bouton vert “Download” pour télécharger ta liste d’attente.

NB :Tu peux mettre plusieurs vidéos/audios dans la liste d’attente avant de les télécharger, tu peux juste cliquer sur le petit + en remplaçant le lien dans la barre blanche.

NB 2 : Tu peux bien sûr aussi télécharger des playlists entières. (et même supprimer certains éléments en cliquant sur la petit croix).

⚠️ Si ça ne fonctionne pas du premier coup, il faut parfois fermer le logiciel et le relancer 😉

## Mais si c’est une vidéo privée (ou alors qu’il faut un compte pour y accéder) ?

Alors je préviens ce n'est pas miracle à pouvoir télécharger des vidéos Netflix non plus, mais ça permet de télécharger des vidéos de [Arte.tv](http://Arte.tv) ou [Auvio](https://www.rtbf.be/auvio/) par exemple. 

### **Étape 1 : Installer une extension pour exporter les cookies**

Selon le navigateur que vous utilisez, choisissez l'une des extensions suivantes :

- **Pour Google Chrome** : [Get cookies.txt LOCALLY](https://chrome.google.com/detail/get-cookiestxt-locally/cclelndahbckbenkjhflpdbgdldlbecc)
- **Pour Mozilla Firefox** : [cookies.txt](https://addons.mozilla.org/fr/firefox/addon/cookies-txt/)

Ces extensions vous permettent d'exporter les cookies de votre session de navigation au format `cookies.txt`.

### **Étape 2 : Exporter les cookies**

1. **Accédez au site web** contenant la vidéo que vous souhaitez télécharger (par exemple, YouTube) et connectez-vous à votre compte si nécessaire.
2. **Ouvrez l'extension** que vous avez installée :
    - Pour **Get cookies.txt LOCALLY** sur Chrome, cliquez sur l'icône de l'extension dans la barre d'outils, puis sélectionnez l'option pour exporter les cookies.
    - Pour **cookies.txt** sur Firefox, cliquez sur l'icône de l'extension et choisissez d'exporter les cookies.
3. **Enregistrez le fichier** `cookies.txt` à un emplacement facilement accessible sur votre ordinateur.

### **Étape 3 : Utiliser les cookies avec Open Video Downloader**

1. **Ouvrez Open Video Downloader**.
2. **Ajoutez l'URL** de la vidéo que vous souhaitez télécharger.
3. **Avant de lancer le téléchargement**, cliquez sur l'icône des paramètres ou des options avancées.
4. **Recherchez l'option** permettant d'ajouter des cookies. Cela peut être intitulé "Load cookies from file" ou similaire.
5. **Sélectionnez le fichier** `cookies.txt` que vous avez exporté précédemment.
6. **Lancez le téléchargement** de la vidéo.

En suivant ces étapes, Open Video Downloader utilisera les cookies de votre session de navigation pour accéder aux vidéos nécessitant une authentification ou des autorisations spécifiques. Cela est particulièrement utile pour télécharger des vidéos privées ou restreintes.

### **Remarque importante**

Certaines extensions de cookies ont été signalées pour des problèmes de confidentialité. Par exemple, l'extension "Get cookies.txt" a été mise à jour pour envoyer des informations de navigation à son développeur (voy. [Reddit](https://www.reddit.com/r/youtubedl/comments/10ar7o7/if_youve_been_using_the_get_cookiestxt_chrome/?tl=fr&utm_source=chatgpt.com)). Il est donc recommandé d'utiliser des alternatives comme "Get cookies.txt LOCALLY" ou "cookies.txt" pour Firefox, qui n'ont pas ces problèmes connus.

Assurez-vous toujours de respecter les conditions d'utilisation des sites web et de ne télécharger des vidéos que pour un usage personnel ou avec l'autorisation appropriée.

---

Si vous avez toujours des questions n’hésitez pas à m’envoyer un mail à [guidevoyageurinformatique@sorcier.mozmail.com](mailto:guidevoyageurinformatique@sorcier.mozmail.com)