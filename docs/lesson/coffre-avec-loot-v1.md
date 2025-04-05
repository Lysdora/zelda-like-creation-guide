# 🗝️ Créer un coffre avec un objet visible à l’ouverture (version débutant)

Dans ce guide, on va apprendre à créer un **coffre simple** qui s’ouvre quand le joueur s’approche.  
Une **animation** se joue automatiquement, et un **objet (ex : une pomme 🍏)** apparaît !

Ce sera notre version **de base**, idéale pour commencer à poser les fondations des coffres dans un RPG 🧰

---

## 🎯 Objectif

- Créer une scène `Coffre.tscn`
- Utiliser un `AnimatedSprite2D` pour l’animation d’ouverture
- Détecter le joueur avec une `Area2D`
- Afficher un objet (visuel) une fois le coffre ouvert

---

## 🧱 Étapes de création

### 1️⃣ Crée une nouvelle scène `Coffre.tscn`

Node principal : `Area2D` → nomme-le `Coffre`

Ajoute dedans :
- `AnimatedSprite2D` → nomme-le `AnimatedSprite2D`
- `CollisionShape2D` → forme simple comme un carré
- `Node2D` vide (optionnel) → qu’on appellera `SpawnPoint` (pour faire apparaître l’objet)
- (plus tard : on pourra ajouter un `AudioStreamPlayer2D`, mais on reste simple ici !)

---

### 2️⃣ Prépare les animations du coffre

Dans `AnimatedSprite2D` :
- Ajoute une animation `"close"` → le coffre fermé
- Ajoute une animation `"open"` → l'ouverture (tu peux utiliser une seule frame si tu n’as pas d’animation)

💡 Commence avec `"close"` comme animation par défaut

---

### 3️⃣ Crée l’objet à faire apparaître

Tu peux utiliser une **pomme** qu’on a déjà faite dans `Pomme.tscn`, ou une simple `Sprite2D` avec une image.

---

### 4️⃣ Script sur le coffre

Crée un nouveau script `Coffre.gd` et écris :

```gdscript
extends Area2D

@onready var animated_sprite = $AnimatedSprite2D
@onready var spawn_point = $SpawnPoint
var coffre_ouvert = false
var scene_objet = preload("res://Pomme.tscn") # remplace avec ton vrai chemin

func _on_body_entered(body):
	if body.name == "Player" and not coffre_ouvert:
		coffre_ouvert = true
		animated_sprite.play("open")
		var objet = scene_objet.instantiate()
		spawn_point.add_child(objet)
```

---

### 5️⃣ Connecte le signal `body_entered`

Comme pour la pomme :
- Va dans l’onglet **"Node" > Signals**
- Double-clique sur `body_entered` et connecte-le à `_on_body_entered()` dans ton script

---

## ✅ Mise à jour v1.2 - Amélioration du coffre

On améliore notre coffre pour qu’il ne s’ouvre **qu’une seule fois**, et n’engendre **qu’un seul objet**.  
Car oui, un coffre magique qui génère une infinité de pommes… ce n’est pas très réaliste 😅

---

### 💡 Pourquoi c’était un problème avant ?

Sans vérification, chaque entrée du joueur **déclenchait à nouveau** :
- l’animation d’ouverture
- l’apparition d’un nouvel objet 🍏
- donc... **plein de pommes infinies !**

---

### ✅ Solution logique

On utilise une **variable de contrôle** : `coffre_ouvert`

Et on vérifie sa valeur **avant d’ouvrir** :

```gdscript
if body.name == "Player" and coffre_ouvert == false:
	coffre_ouvert = true
	animated_sprite_2d.play("open")
	var objet = scene_objet.instantiate()
	spawn_point.call_deferred("add_child", objet)
	await get_tree().create_timer(0.5).timeout
	animated_sprite_2d.play("close")
```

---

### ❓ Pourquoi `call_deferred()` ?

> ⚠️ Si on utilise `add_child()` directement pendant une détection de collision (`body_entered`), Godot peut générer une erreur :
> 
> `"Can't change this state while flushing queries"`

C’est pour ça qu’on utilise :

```gdscript
spawn_point.call_deferred("add_child", objet)
```

✅ Cela **repousse l’action** à un moment sûr, juste après la fin du signal.  
On avait déjà vu ça dans le **guide de la pomme 🍏**, quand on faisait `queue_free()` sur la collision.

---



## ✅ Résultat attendu

- Le joueur s’approche
- Le coffre joue l’animation `"open"`
- Un objet (ex : une pomme) apparaît juste au-dessus

---

## 🧪 Exercices

- Change l’objet (clé, potion, pièce…)
- Place le `SpawnPoint` un peu plus haut pour que l’objet flotte
- Ajoute un `AudioStreamPlayer2D` pour jouer un son à l’ouverture 🔊

---

👩‍💻 Écrit par : **Lysdora**  

