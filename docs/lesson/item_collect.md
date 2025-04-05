# 🍏 Créer une pomme à ramasser (version débutant)

Dans ce guide, on va créer un objet simple à collecter dans Godot 4.4 : une **pomme**.  
Le joueur pourra la ramasser en marchant dessus. Elle disparaîtra ensuite automatiquement.

---

## 🎯 Objectif

Créer une pomme qui :
- Est visible dans le monde
- A une zone de détection
- Disparaît quand le joueur la touche

---

## 🧱 Étapes de création

### 1️⃣ Crée une nouvelle scène `Pomme.tscn`

Node principal : `Area2D` → nomme-le `Pomme`

Ajoute dedans :
- `Sprite2D` → pour afficher l’image de la pomme
- `CollisionShape2D` → pour détecter les collisions (forme simple comme cercle ou carré)

---

### 2️⃣ Active la détection dans l’inspecteur

- Sélectionne `Pomme (Area2D)`
- Coche ✅ **Monitoring**
- Coche ✅ **Monitorable**

---

### 3️⃣ Connecte le signal `body_entered`

Depuis l’onglet **Node > Signals** :  
Double-clique sur `body_entered`, connecte-le à un script sur le node `Pomme`.

---

### 4️⃣ Ajoute ce code :

```gdscript
func _on_body_entered(body):
	if body.name == "Player":
		queue_free()
```

Cela supprime la pomme si c’est le joueur qui la touche.

---

### ✅ Résultat

Quand le joueur entre en collision avec la pomme :
- Le signal est déclenché
- Elle se détruit avec `queue_free()`

---

## 🧪 Petits exercices

- Change le sprite pour un autre fruit 🍌🍇
- Ajoute plusieurs pommes dans la scène
- Modifie le `CollisionShape2D` pour qu’il colle bien au sprite

---

👩‍💻 Écrit par : **Lysdora**  
