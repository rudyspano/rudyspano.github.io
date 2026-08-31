# KendoGame — maquette d'animation

Prototype 2D d'un kendoka de profil, au trait, pour préparer un jeu de kendo.
Tout tient dans `index.html` : aucune dépendance, aucun build.

## Ouvrir

Ouvrir `index.html` dans un navigateur, ou via GitHub Pages.

## Ce que fait la maquette

Le kendoka est un **rig articulé** en centimètres (sol à y = 0, profil tourné vers +x),
pas un dessin figé :

- os de longueur fixe, jambes résolues en IK à deux os (hanche → genou → cheville) ;
- pied en trois points (talon / plante / orteils), les orteils se replient quand le talon monte ;
- coude posé sur le segment épaule-poignet avec un décalage animé, parce que de profil
  un coude s'écarte surtout latéralement : la projection raccourcit les os ;
- chaque pièce d'armure est une fonction de dessin isolée (`drawMen`, `drawDo`, `drawTare`,
  `drawKote`, `drawHakamaLeg`, `drawShinai`…), destinée à être remplacée une à une par un asset.

## Le men avec fumikomi

150 frames à 60 fps (2,5 s), découpées en 10 phases affichées dans le panneau avec leur
nombre de frames et leur durée. Points clés :

| phase | frames | note |
|---|---|---|
| Furikaburi | 44–62 | armé grande amplitude, kissaki derrière |
| Poussée sur les orteils | 68–72 | orteil gauche planté, jambe tendue à 98 % |
| Envol | 72–76 | plus aucun appui, 4,4 m/s |
| Fumikomi (frappe) | 76–82 | le pied droit claque, le shinai touche le men |

Avance du bassin du kamae à la frappe : 116 cm.

## Repères de lecture

- On regarde le kendoka **depuis sa droite** : côté droit au premier plan (trait noir),
  côté gauche à l'arrière-plan (trait gris). Le pied droit est devant, la main droite
  est la main haute du shinai, côté tsuba.
- Deux modes de vue : **Calque** (tout translucide) et **Occlusion** (ce qui est devant masque).
- `?frame=82` dans l'URL ouvre directement sur une frame précise.
