# 💀 Système de mort complet dans Godot 4.4

Bienvenue dans ce guide pour mettre en place un système complet de mort dans un jeu RPG avec Godot 4.4 !  
Tu vas apprendre à gérer la perte de vie, jouer un son quand le joueur est touché, afficher une scène de Game Over et réinitialiser proprement la partie 🎮

---

## 🧱 Étape 1 : Créer un `HealthManager` (singleton)

Ce script gère les points de vie et émet des signaux quand la santé change.

```gdscript
extends Node

signal health_updated(current, max)

var max_health := 3
var current_health := 3

func lose_health(amount: int = 1):
	current_health = clamp(current_health - amount, 0, max_health)
	emit_signal("health_updated", current_health, max_health)

func reset_health():
	current_health = max_health
	emit_signal("health_updated", current_health, max_health)
```

Ajoute-le dans **Project > Project Settings > AutoLoad** avec le nom `HealthManager`.

---

## 🧠 Étape 2 : Gérer la perte de vie dans `Player.gd`

Dans le joueur, détecte les collisions avec la `HitBox` et ajoute un cooldown pour éviter de perdre toute la vie d'un coup :

```gdscript
@export var hurt_cooldown_duration: float = 0.8
var hurt_cooldown_timer: float = 0.0

func _physics_process(delta):
	if hurt_cooldown_timer > 0:
		hurt_cooldown_timer -= delta

func _on_hurt_box_area_entered(area):
	if area.name == "HitBox":
		if hurt_cooldown_timer <= 0 and HealthManager.current_health > 0:
			HealthManager.lose_health()
			$HurtSound.play()
			hurt_cooldown_timer = hurt_cooldown_duration
			if HealthManager.current_health == 0:
				handle_player_death()
```

---

## 🔊 Étape 3 : Ajouter un son quand le joueur est touché

- Ajoute un `AudioStreamPlayer` au joueur
- Renomme-le `HurtSound`
- Glisse ton fichier `.wav` dans le champ **Stream** (désactive Autoplay)
- Dans le code ci-dessus, on utilise : `$HurtSound.play()`

---

## 💀 Étape 4 : Créer la scène `GameOver.tscn`

Structure recommandée :

```
GameOver (Control)
├── ColorRect (fond noir, Full Rect)
├── CanvasGroup
│   ├── Label ("GAME OVER")
│   ├── Button ("Rejouer")
│   └── Label (Crédits)
├── AnimationPlayer (pour le fade)
└── AudioStreamPlayer (musique triste)
```

Animation `"fade_in"` sur le `CanvasGroup:modulate.a` :

- 0s : `a = 0.0`
- 1.0s : `a = 0.0` (pause dramatique)
- 3.0s : `a = 1.0`

Dans le script :

```gdscript


func _ready():
	$AnimationPlayer.play("fade_in")
	await get_tree().create_timer(1.0).timeout
	$GameOverMusic.play()

func _on_restart_button_pressed():
	HealthManager.reset_health()
	gget_tree().change_scene_to_file("res://world/village.tscn") # ajoute le nom de ta scene
```

---

## 🔁 Étape 5 : Jouer l’animation de mort dans `Player.gd`

Quand la vie tombe à 0 :

```gdscript
func handle_player_death():
	print("Player is dead!")
	animated_sprite.play("dead")
	velocity = Vector2.ZERO
	set_physics_process(false)
	await get_tree().create_timer(1.5).timeout
	get_tree().change_scene_to_file("res://ui/game_over.tscn") # ajoute le nom de ta scene
```

---

## 🎉 Résultat final

✔ Le joueur peut être blessé avec un cooldown  
✔ Un son se joue quand il est touché  
✔ Une animation de mort se lance  
✔ Un écran Game Over apparaît avec fade et musique  
✔ Le bouton "Rejouer" recharge la scène et réinitialise la vie

---

🐾 _Ce guide fait partie de mon projet de RPG Godot 4.4._  
🔗 [Retour à l'accueil](../index.md)  
📆 Dernière mise à jour : 12/04/2025
