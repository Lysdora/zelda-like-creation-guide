# 🍏 Compter ses objets - Version Padawan (console uniquement)

## 🎯 Objectif du jour

Quand on ramasse des objets (ex : des pommes), il faut bien **les compter quelque part** !  
Voici comment créer une **variable compteur** et l’utiliser pour suivre nos 🍏

---

## 🧩 Étape 1 : Ajouter une variable dans le Player

Dans le script `Player.gd` (ou celui de ton personnage), ajoute tout en haut :

```gdscript
var nombre_de_pommes = 0
```

---

## 🧩 Étape 2 : Incrémenter quand on ramasse une pomme

Dans le script de la pomme (dans `_on_body_entered()`), ajoute :

```gdscript
if body.name == "Player":
    body.nombre_de_pommes += 1
    print("🍏 Nombre de pommes : ", body.nombre_de_pommes)
```

---

## 🧠 Et voilà !

Maintenant, à chaque fois que tu ramasses une pomme :
- Le compteur **augmente**
- Le nombre s’affiche dans la console
- 🎉 Tu peux suivre ta **collection d’objets** en live !

---

## 🔮 Prochaine étape

On pourra ensuite :
- 🖥️ Afficher le nombre dans le HUD
- 📦 Gérer plusieurs objets différents (potions, clés…)
- 🧃 Créer une **barre de raccourcis** pour manger la pomme 💪

---

👩‍💻 Par : **Lysdora**  
✨ Guide écrit pour la Team Padawan - *“Un pas de plus vers ton inventaire magique”*
