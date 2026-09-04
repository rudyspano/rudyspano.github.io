# KendoGame — maquette d'animation

Prototype 2D d'un kendoka de profil pour préparer un jeu de kendo. La maquette
sert à régler les mouvements ; le jeu Unity lit **les mêmes fichiers**, si bien
qu'un chiffre réglé ici est réglé des deux côtés.

## Ouvrir

Il faut la servir en **HTTP** : elle charge ses données au lieu de les contenir,
et un navigateur refuse un `fetch` depuis `file://`.

```bash
python3 -m http.server 8000
# puis http://localhost:8000/kendo/
```

`?frame=82` ouvre sur une frame précise, `?attack=do` sur une autre attaque,
`?side=left` sur l'autre sens de jeu.

## Où vivent les réglages

```
Assets/Resources/Shared/Data/rig.json      proportions du rig, teintes, ancres des vignettes
Assets/Resources/Shared/Data/attacks.json  poses clés, phases et repères des quatre attaques
Assets/Resources/Shared/Art/               les vignettes d'armure et le décor
```

Le chemin a la forme d'un projet Unity parce que c'en est un : ce dossier est la
copie conforme de celui que le jeu embarque, et n'est pas réécrit à la
publication — sans quoi les deux dériveraient.

Ces deux JSON portent des **commentaires**, le pourquoi de chaque chiffre à
l'endroit où on le règle. La maquette les retire avant `JSON.parse`.

## Ce que fait la maquette

Le kendoka est un **rig articulé** en centimètres (sol à y = 0, profil tourné
vers +x), pas un dessin figé :

- os de longueur fixe, jambes résolues en IK à deux os (hanche → genou → cheville) ;
- pied en trois points (talon / plante / orteils), les orteils se replient quand
  le talon monte ;
- coude posé sur le segment épaule-poignet avec un décalage animé, parce que de
  profil un coude s'écarte surtout latéralement : la projection raccourcit les os ;
- chaque pièce d'armure est une vignette dessinée, amenée sur son os par deux
  points d'ancrage exprimés en fractions de sa propre image.

Quatre attaques jouables — men, kote, dō, tsuki — chacune avec ses poses clés,
ses phases, sa cible sur le défenseur et l'ampleur du recul qu'elle provoque.

## Le men avec fumikomi

150 frames à 60 fps (2,5 s), découpées en phases affichées dans le panneau avec
leur nombre de frames et leur durée. Points clés :

| phase | frames | note |
|---|---|---|
| Furikaburi | 44–58 | armé grande amplitude, kissaki derrière |
| Poussée sur les orteils | 58–72 | orteil gauche planté, jambe tendue à 98 % |
| Envol | 72–76 | plus aucun appui, 4,5 m/s |
| Fumikomi (frappe) | 76–82 | le pied droit claque, le shinai touche le men |

Avance du bassin du kamae à la frappe : 116 cm.

## Repères de lecture

- On regarde le kendoka **depuis sa droite** : côté droit au premier plan, côté
  gauche à l'arrière-plan. Le pied droit est devant, la main droite est la main
  haute du shinai, côté tsuba.
- Trois vues : **Jeu** (décor et vignettes), **Normal** et **Calque** — les deux
  dernières servent à analyser le mouvement.
