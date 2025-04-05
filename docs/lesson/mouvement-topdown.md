# 🚶‍♀️ Mouvement Top-Down de Base (Godot 4.4)

Un script simple pour déplacer ton personnage en vue de dessus, comme dans les jeux à la Zelda ! 🧝‍♂️✨

---

## 🎮 Le script de base (à mettre sur un `CharacterBody2D`)

```gdscript
extends CharacterBody2D

@onready var animated_sprite: AnimatedSprite2D = $AnimatedSprite2D
@export var vitesse: float = 40.0

var direction: Vector2 = Vector2.ZERO
var last_direction: Vector2 = Vector2.DOWN  # direction par défaut

func _physics_process(_delta: float) -> void:
	move_player()
	animation_player()
	move_and_slide()

func move_player():
	direction = Input.get_vector("left", "right", "up", "down")

	# 🧠 On garde en mémoire la dernière direction utilisée
	if direction != Vector2.ZERO:
		last_direction = direction

	velocity = direction * vitesse

func animation_player():
	if direction != Vector2.ZERO:
		if direction.x > 0:
			animated_sprite.play("walk_right")
		elif direction.x < 0:
			animated_sprite.play("walk_left")
		elif direction.y > 0:
			animated_sprite.play("walk_down")
		elif direction.y < 0:
			animated_sprite.play("walk_up")
	else:
		if last_direction.x > 0:
			animated_sprite.play("idle_right")
		elif last_direction.x < 0:
			animated_sprite.play("idle_left")
		elif last_direction.y > 0:
			animated_sprite.play("idle_down")
		elif last_direction.y < 0:
			animated_sprite.play("idle_up")
```

---

## 🧾 Résumé

- `direction` ➜ direction actuelle selon les touches appuyées 🎯  
- `last_direction` ➜ garde la dernière direction pour les animations idle 🧭  
- `velocity = direction * vitesse` ➜ applique la vitesse  
- `move_and_slide()` ➜ déplace le personnage

---

## 🚀 Aller plus loin : 3 façons d’améliorer ce système

### 1️⃣ Méthode simple avec `if / elif` ✅

Celle qu’on utilise ici. Lisible, facile à comprendre.  
Parfait pour débuter 💪

---

### 2️⃣ Méthode avec `match` 🎯

Plus compacte, mais attention : elle ne marche que si la direction est **exactement** `(1, 0)`, `(0, -1)`, etc.

```gdscript
func animation_player():
	if velocity:
		match direction:
			Vector2.RIGHT:
				animated_sprite.play("walk_right")
			Vector2.LEFT:
				animated_sprite.play("walk_left")
			Vector2.DOWN:
				animated_sprite.play("walk_down")
			Vector2.UP:
				animated_sprite.play("walk_up")
	else:
		match last_direction:
			Vector2.RIGHT:
				animated_sprite.play("idle_right")
			Vector2.LEFT:
				animated_sprite.play("idle_left")
			Vector2.DOWN:
				animated_sprite.play("idle_down")
			Vector2.UP:
				animated_sprite.play("idle_up")
```

🧠 Astuce : si tu veux que `match` fonctionne bien, pense à arrondir les directions avec `round()`.

---

### 3️⃣ Utiliser une `State Machine` 🧩 (Machine à États)

Quand ton jeu devient plus complexe (parler, attaquer, rouler...), c’est mieux d’organiser le code en "états" :

```gdscript
enum State { IDLE, WALK, ATTACK }

var current_state = State.IDLE

func _physics_process(_delta):
	match current_state:
		State.IDLE:
			handle_idle()
		State.WALK:
			handle_walk()
		State.ATTACK:
			handle_attack()
```

🧘‍♀️ Chaque état a son propre comportement. C’est plus clean, mais un peu plus avancé.

---

## 🛠️ Astuce Debug : animations qui restent bloquées en "walk" ?

Si ton personnage continue à jouer l’animation de marche même quand tu ne bouges plus...

➡️ **Vérifie cette ligne :**

```gdscript
if direction != Vector2.ZERO:
	last_direction = direction
```

⚠️ Ne fais pas `if last_direction != Vector2.ZERO`, c’est une erreur classique.

---

## 🐾 Conclusion

Commence **simple** avec les `if` pour bien tout comprendre.  
Quand tu seras à l’aise, tu pourras passer à une version plus flexible avec `match` ou une `State Machine`.

---

👩‍💻 Écrit par **Lysdora**  
✨ Projet : [mon-rpg-zelda](https://lysdora.github.io/mon-rpg-zelda/)
