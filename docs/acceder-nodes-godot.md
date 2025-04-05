# 🧭 Comprendre et accéder aux Nodes dans Godot 4.4

Dans ce guide, on va explorer comment **accéder à des nodes dans Godot**, détecter un joueur, et comprendre la différence entre les noms de scènes, de scripts et de nodes dans l’arbre.

On commence simple, et on enrichira ce guide au fur et à mesure ✨

---

## 🔎 Vérifier le nom du node dans une collision

Quand on utilise un signal comme `body_entered`, on peut détecter quel node est entré dans la zone.  
Exemple :

```gdscript
func _on_body_entered(body):
	if body.name == "Player":
		queue_free()
```

---

## ❓ Que signifie `"Player"` ici ?

Ce **n’est pas** :
- Le nom de la **scène `.tscn`**
- Le nom du **script attaché au node**

✅ C’est le **nom du node dans l’arbre de scène**.

---

### Exemple :

```text
MainScene
├── Player
│   └── Sprite2D
```

Dans cet exemple, le node `CharacterBody2D` du joueur s’appelle **Player** (avec un P majuscule).  
Donc dans le script, on doit écrire :

```gdscript
if body.name == "Player":
```

Mais si dans ton projet ton node s’appelle `"player"` (avec un p minuscule), il faudra écrire :

```gdscript
if body.name == "player":
```

🎯 Astuce : **fais un clic sur ton node dans l’arbre de scène et regarde son nom exact** dans l’inspecteur en haut.

---

## 🧪 À venir

Dans les prochaines parties, on verra :
- 🎯 Comment accéder à des nodes enfants ou parents (`get_node`, `$`, `get_parent()`, etc.)
- 🗂️ Comment organiser les scènes et noms proprement
- 👑 Comment utiliser les **groupes** pour détecter des types de nodes (comme tous les "joueurs")
- 🧠 Comment accéder à des nodes entre scènes (via `get_tree().root`, `AutoLoad`, etc.)

---

👩‍💻 Guide en cours d’écriture par Lysdora  

