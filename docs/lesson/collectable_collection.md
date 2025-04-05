
# 🍄 Créer un système de Collectables dans Godot 4.4

Dans cette leçon, nous allons apprendre à créer un système de collectables (comme des potions, des pièces ou d'autres objets à ramasser) dans notre RPG Zelda-like. 🎮✨

Nous allons procéder par étapes pour bien comprendre chaque concept.

---

## 📌 Objectif
Créer un **Collectable** qui disparaît lorsqu'il est ramassé par le joueur, grâce à un script simple.

## 📁 Préparation
- **Godot 4.4.1**
- **Dossier dédié : `collectables/`**

---

## 🌟 Étape 1 : Création de la scène Collectable

1. **Crée une nouvelle scène** en tant que **`Area2D`**.
2. **Renomme-la `Collectable`**.
3. **Ajoute un `Sprite2D`** en tant que child de `Collectable`.
4. **Ajoute un `CollisionShape2D`** en tant que child de `Collectable`.
5. **Sélectionne `CollisionShape2D` et clique sur `Make Unique`** pour la rendre indépendante.
6. **(Important !) Cela permet de modifier chaque `CollisionShape2D` séparément pour chaque objet que tu créeras à partir de ce template.**

🎯 Ta hiérarchie devrait ressembler à ceci :  
```
Collectable (Area2D)
├── Sprite2D
└── CollisionShape2D
```

---

## ✍️ Étape 2 : Ajouter le script de collectable

1. **Ajoute un nouveau script** au noeud `Collectable` avec le code suivant :

```gdscript
extends Area2D

func collect():
    queue_free()
```

📌 Ici, la méthode `collect()` va supprimer (`queue_free()`) l'objet dès qu'il est ramassé par le joueur.

---

## 📂 Étape 3 : Créer un Template d'Item

1. **Sauvegarde ta scène `Collectable`** dans ton dossier dédié : `collectables/`.
2. **Clique sur `Scene > New Inherit Scene`** pour créer un nouvel objet basé sur `Collectable`.
3. **Ajoute un Sprite pour représenter ton item (ex: potion)**.
4. **Clique sur `CollisionShape2D` et clique sur `Make Unique`** pour pouvoir modifier sa forme indépendamment des autres objets.
5. **Ajuste la `CollisionShape2D`** pour correspondre au sprite.
6. **Sauvegarde cette scène spécifique (par exemple, `Potion.tscn`)** dans ton dossier `collectables/`.

✅ **En rendant chaque `CollisionShape2D` unique (`Make Unique`), tu pourras facilement ajuster les collisions pour chaque type d'objet.**

---

## 🗺️ Étape 4 : Détection de la collecte par le joueur

1. **Ajoute un `Area2D` nommé `HurtBox`** au noeud `Player`.
2. **Ajoute un `CollisionShape2D`** en tant que child de `HurtBox`.
3. **Crée un signal pour détecter les collisions :**

```gdscript
func _on_hurt_box_area_shape_entered(area_rid: RID, area: Area2D, area_shape_index: int, local_shape_index: int) -> void:
    if area.has_method("collect"):
        area.collect()
```

📌 Cela signifie que **lorsque ton joueur détecte une `Area2D` qui possède une fonction `collect()`, elle sera supprimée** ! 🎉

---

## 🌟 Étape 5 : Création d'une Scène Collection avec TileMap

1. **Crée un nouveau TileMap Layer** sous ton noeud principal (par exemple, `Campagne`).
2. **Renomme ce TileMap en `Collectables`**.
3. Dans l'inspecteur, clique sur `Tile Set` et appuie sur le bouton `+` à côté de la poubelle en bas à gauche.
4. Sélectionne **`Scene Collection`** et renomme-la en `Collectable`.
5. Fais glisser tes scènes de collectables (par exemple `life_pot.tscn` et `sword.tscn`) dans la zone prévue pour les ajouter à ta collection.
6. Maintenant, tu peux facilement placer ces objets collectables **directement sur ta carte** en cliquant et en les positionnant ! 🎉

---

## 🗡️ Étape 6 : Création de l'Épée (Sword) avec Animation

🎉 Nous allons maintenant ajouter une **épée** qui s'anime quand Elara la ramasse !

### 🔨 Création de l'Épée

1. **Crée une nouvelle scène héritée de `Collectable`** (clic droit sur `Collectable > New Inherit Scene`).
2. **Renomme-la `Sword`**.
3. **Ajoute un sprite d'épée** (si tu utilises une spritesheet : `New Atlas Texture > Edit Region > Active Pixel Snap > Sélectionne l'épée`).
4. **Ajoute un `AnimationPlayer` au noeud `Sword`**.

### 💾 Création de l'Animation

1. Renomme l'animation en `"spin"`.
2. Sur la timeline, mets une durée de `0.4` secondes.
3. Sélectionne l'épée et clique sur **`Rotation`** dans l'inspecteur (`Transform > Rotation`).
4. À `0` seconde, ajoute une clé (icône de clé).
5. À `0.2` seconde, fais légèrement pivoter l'épée et ajoute une nouvelle clé.
6. À `0.4` seconde, remets l'épée dans sa position initiale et ajoute une clé.

### 📜 Script de l'Épée

1. **Crée un nouveau script** en l'attachant au `Sword`.
2. **Déplace le script `collectable.gd` en tête d'extension** avec :

```gdscript
extends "res://scenes/collectables/collectable.gd"

@onready var animation: AnimationPlayer = $AnimationPlayer

func collect():
    animation.play("spin")
    await animation.animation_finished
    super.collect()
```

💡 **Explication :**  
- Le `await animation.animation_finished` signifie que le script attend que l'animation soit terminée avant de continuer.  
- `super.collect()` permet d'appeler la fonction `collect()` de la scène `Collectable` qui fait disparaître l'objet (`queue_free()`).

---

## ✅ Conclusion
✨ Bravo ! Tu viens d'ajouter un **nouvel objet dynamique** avec une animation au ramassage ! 🎉

On peut maintenant ajouter des **épées**, des **potions**, et bien plus à notre jeu. 🌈



---


