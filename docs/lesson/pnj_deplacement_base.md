
# 🎓 Leçon Base : Déplacement simple d'un PNJ entre deux points (sans attente)

> Niveau : **Débutant** — Catégorie : "Bases du mouvement"  
> Pour apprendre à faire marcher un fermier, une poule ou un garde entre deux points ✈️

---

## 🌿 Objectif
Faire un PNJ qui se déplace automatiquement entre deux points placés dans la **scène principale**, sans s'arrêter.

---

## 🧰 Matériel requis
- Une scène avec ton PNJ (ex : `Fermier.tscn`)
- Deux `Node2D` nommés `PointA` et `PointB` placés dans la **carte principale**
- Affecte `PointA` et `PointB` au PNJ dans l'inspecteur (grâce aux `@export`)

---

## 🧑‍💻 Script de base (sans animation, sans pause)

```gdscript
extends CharacterBody2D

@export var point_a: Node2D
@export var point_b: Node2D
@export var speed := 40.0

var current_target_node: Node2D

func _ready():
    current_target_node = point_a  # Le PNJ commence vers A

func _physics_process(delta):
    if current_target_node == null:
        return

    var target_pos = current_target_node.global_position
    var direction = (target_pos - global_position).normalized()
    velocity = direction * speed
    move_and_slide()

    if global_position.distance_to(target_pos) < 2.0:
        # ➜ On change de cible !
        if current_target_node == point_a:
            current_target_node = point_b
        else:
            current_target_node = point_a
```

---

## 🛑 Astuces importantes
- ❌ Ne mets pas `PointA` et `PointB` dans la scène du PNJ, mais dans la **map principale**
- ✅ Toujours tester dans la scène principale (pas juste la scène du PNJ)

---

## 👏 Tu sais maintenant :
- ✅ Faire un PNJ qui marche automatiquement
- ✅ Utiliser `@export` pour désigner des points dans l’éditeur
- ✅ Créer une boucle simple de mouvement

➡️ Prêt(e) pour une version plus vivante avec **pause et animation** ? Va voir la leçon intermédiaire 🌟
