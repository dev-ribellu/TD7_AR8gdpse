# AR.js - Flèche jaune en réalité augmentée

Application web de réalité augmentée (AR.js + A-Frame) qui détecte une image
(une flèche jaune peinte sur un sol en dalles grises) grâce à l'image tracking
NFT, puis fait apparaître un modèle 3D au format GLB par-dessus.

## Aperçu

1. L'utilisateur ouvre la page sur son smartphone.
2. Il autorise l'accès à la caméra.
3. Il pointe la caméra vers la flèche jaune.
4. Le modèle 3D apparaît sur l'image.

## Structure du projet

```
.
├── index.html
├── README.md
└── assets/
    ├── arrow.fset
    ├── arrow.fset3
    ├── arrow.iset
    └── model.glb
```

| Fichier | Rôle |
|---|---|
| `index.html` | Scène A-Frame + AR.js, écran de chargement, messages |
| `assets/arrow.*` | Descripteurs d'image NFT générés à partir de la photo de la flèche |
| `assets/model.glb` | Modèle 3D affiché sur l'image |

## Prérequis

- Un smartphone avec caméra (Chrome sur Android ou Safari sur iOS)
- Un accès **HTTPS** : le navigateur refuse la caméra en HTTP simple
  (sauf sur `localhost`)
- Node.js (uniquement pour le test en local)

## Générer les descripteurs d'image

1. Ouvrir https://carnaux.github.io/NFT-Marker-Creator/
2. Charger la photo de la flèche (`arrow.jpg`)
3. Télécharger les 3 fichiers générés : `.fset`, `.fset3`, `.iset`
4. Les renommer `arrow.fset`, `arrow.fset3`, `arrow.iset`
5. Les placer dans le dossier `assets/`

Le code utilise `url="assets/arrow"` : le chemin s'écrit **sans extension**.

## Lancer le projet

### Option 1 : GitHub Pages (recommandé)

1. Pousser le dépôt sur GitHub
2. Aller dans **Settings → Pages**
3. Choisir la branche `main` et le dossier `/ (root)`, puis enregistrer
4. Ouvrir l'URL `https://<utilisateur>.github.io/<depot>/` sur le smartphone

### Option 2 : Glitch

1. Créer un projet **hello-website** sur https://glitch.com
2. Remplacer le contenu de `index.html` par celui du projet
3. Uploader les 4 fichiers dans le dossier `assets/`
4. Ouvrir l'URL générée par Glitch sur le smartphone

### Option 3 : En local avec ngrok

```bash
# Terminal 1 : serveur local
npx serve -l 3000

# Terminal 2 : tunnel HTTPS
ngrok http 3000
```

Ouvrir sur le smartphone l'URL `https://xxxx.ngrok-free.app` fournie par ngrok.
Pour éviter de la taper, générer un QR code avec https://ngrok-qr.alanwsmith.com/.

## Utilisation sur smartphone

1. Ouvrir l'URL HTTPS
2. Accepter l'autorisation de la caméra
3. Attendre la fin du message « Chargement, veuillez patienter... »
   (cela peut prendre quelques secondes selon l'appareil)
4. Pointer la caméra vers la flèche jaune (en vrai, imprimée ou affichée
   sur un autre écran)

## Ajuster le modèle 3D

Dans `index.html`, modifier l'entité du modèle :

```html
<a-entity gltf-model="assets/model.glb"
          scale="2 2 2"
          position="50 0 -50"
          rotation="-90 0 0"></a-entity>
```

| Attribut | Effet | Conseil |
|---|---|---|
| `scale` | Taille du modèle | Augmenter si trop petit, diminuer si trop grand |
| `position` | Décalage x y z par rapport au centre de l'image | Les unités du NFT sont grandes (ordre de 100) |
| `rotation` | Orientation en degrés | `-90 0 0` couche le modèle à plat sur l'image |

Pour que le modèle soit plus grand ou plus petit, essayer par exemple
`scale="10 10 10"` ou `scale="0.5 0.5 0.5"`.

## Conseils pour améliorer la détection

- **Lumière** : travailler dans un endroit bien éclairé, sans reflets
  sur la surface
- **Distance** : se placer entre 30 cm et 1 m de la flèche
- **Angle** : éviter les angles trop rasants, rester presque face à l'image
- **Stabilité** : tenir le téléphone fermement et bouger lentement
- **Image source** : une image nette, contrastée et de bonne résolution
  (au moins 1000 px de large) donne un meilleur suivi. Les surfaces très
  répétitives comme les dalles uniformes sont plus difficiles à suivre :
  cadrer la photo de façon à inclure la flèche et les joints voisins
- **Tracking** : si le modèle tremble, augmenter `smoothCount` ou
  `smoothTolerance` sur `<a-nft>`

## Dépannage

| Problème | Solution |
|---|---|
| Écran de chargement qui ne disparaît pas | Vérifier que les fichiers `arrow.*` sont bien dans `assets/` et que le chemin est correct |
| La caméra ne s'ouvre pas | Vérifier que l'URL est en `https://` et que l'autorisation caméra est accordée |
| Le modèle n'apparaît pas | Ouvrir la console du navigateur, vérifier le chemin `assets/model.glb` et ajuster `scale` |
| Le modèle est mal orienté | Modifier `rotation` |
| Le modèle tremble | Améliorer la lumière, augmenter les paramètres `smooth*` |

## Technologies

- [A-Frame](https://aframe.io/)
- [AR.js](https://ar-js-org.github.io/AR.js-Docs/) (image tracking NFT)
- [NFT-Marker-Creator](https://carnaux.github.io/NFT-Marker-Creator/)

## Contexte

Projet réalisé dans le cadre du TD n°7 de la ressource R5.Crea.06
(Création numérique), BUT MMI 3.