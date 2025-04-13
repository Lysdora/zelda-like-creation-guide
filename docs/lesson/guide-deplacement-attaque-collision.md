# 🎮 Gestion des déplacements, attaques et collisions dans Godot 4.4

> Ce guide fait partie de mon journal de progression sur Godot 4.4 ✨  
> Il décrit comment j'ai codé le déplacement de mon personnage, la gestion des animations, les attaques et les collisions avec un `CharacterBody2D`.

---

## 🧩 Objectif

Créer un personnage capable de :

- Se déplacer en 2D (8 directions avec `Input.get_vector`)
- Garder en mémoire sa dernière direction
- Attaquer avec un cooldown
- Jouer les bonnes animations (`walk`, `idle`, `attack`)
- Afficher le nom de ce qu’il percute (debug collision)

---

## 🛠️ Script complet

```gdscript
extends CharacterBody2D

@onready var animated_sprite: AnimatedSprite2D = $AnimatedSprite2D
@export var vitesse: float = 40.0

var direction: Vector2 = Vector2.ZERO
var last_direction: Vector2 = Vector2.DOWN  # direction par défaut

var is_attacking: bool = false
var attack_cooldown := 0.4
var attack_timer := 0.0

func _physics_process(delta: float) -> void:
	if is_attacking:
		attack_timer -= delta
		if attack_timer <= 0:
			is_attacking = false
	else:
		if Input.is_action_just_pressed("attack"):
			start_attack()
		else:
			move_player()
	handleCollision()	
	animation_player()
	move_and_slide()

func start_attack():
	is_attacking = true
	attack_timer = attack_cooldown
	
	if last_direction.x > 0:
		animated_sprite.play("attack_right")
	elif last_direction.x < 0:
		animated_sprite.play("attack_left")
	elif last_direction.y > 0:
		animated_sprite.play("attack_down")
	elif last_direction.y < 0:
		animated_sprite.play("attack_up")

func move_player():
	direction = Input.get_vector("left", "right", "up", "down")

	# 🧠 On garde en mémoire la dernière direction utilisée
	if direction != Vector2.ZERO:
		last_direction = direction

	velocity = direction * vitesse

func animation_player():
	if is_attacking:
		return
		
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

func handleCollision():
	for i in get_slide_collision_count():
		var collision = get_slide_collision(i)
		var collider = collision.get_collider()
		print_debug(collider.name)
```

---

## 💡 Notes utiles

- `@onready` permet de récupérer une référence à notre sprite dès que le node est prêt.
- Le cooldown d’attaque (`attack_cooldown`) empêche de spammer l’attaque.
- `last_direction` est super important pour gérer les animations `idle` et les attaques dans la bonne direction.
- `handleCollision()` affiche dans la console ce qu’on percute. Pratique pour tester si les collisions fonctionnent bien 💥.

---

## 📦 Pré-requis côté projet

- Des animations nommées :
  - `walk_up`, `walk_down`, `walk_left`, `walk_right`
  - `idle_up`, `idle_down`, `idle_left`, `idle_right`
  - `attack_up`, `attack_down`, `attack_left`, `attack_right`
- Un `AnimatedSprite2D` bien configuré.
- Les actions `left`, `right`, `up`, `down` et `attack` dans ton `InputMap`.

---

## 📅 Date de rédaction

*Guide rédigé le 12/04/2025*

---

🎀 *Ce guide fait partie de mon projet de création de RPG Zelda-like. D'autres guides sont à retrouver ici :*  
🔗 [Retour à l'accueil du guide](../README.md)
