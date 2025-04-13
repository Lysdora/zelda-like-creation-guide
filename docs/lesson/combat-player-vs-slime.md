
# 🗡️ Guide : Système de Combat Player vs Slime dans Godot

## Table des matières

- [Configuration du Joueur](#configuration-du-joueur)
- [Configuration du Slime](#configuration-du-slime)
- [Mise en place des collisions](#mise-en-place-des-collisions)
- [Bonus et Améliorations](#bonus-et-améliorations)

---

## 🎮 Configuration du Joueur

### Structure des Nœuds

Voici comment structurer ton joueur dans l’arborescence des nœuds :

```
Player (CharacterBody2D)
├── AnimatedSprite2D
├── CollisionShape2D
└── AttackArea (Area2D)
    └── CollisionShape2D
```

> 🧠 `AttackArea` est une zone invisible qui détecte les ennemis lorsqu'on attaque. Elle ne sert qu’à détecter les collisions !

---

### Code du Joueur (player.gd)

```gdscript
extends CharacterBody2D

#-----VARIABLES D'ATTAQUE 🗡️-----
@export var attack_damage: int = 1
var is_attacking: bool = false
var attack_cooldown := 0.4
var attack_timer := 0.0

#-----INITIALISATION 🔄-----
func _ready() -> void:
    $AttackArea/CollisionShape2D.set_deferred("disabled", true)
```

> 🎯 À l'initialisation, on désactive la zone d'attaque. Elle ne s'activera que pendant une attaque.

---

```gdscript
#-----SYSTÈME D'ATTAQUE 💥-----
func start_attack():
    if is_attacking:
        return

    is_attacking = true
    attack_timer = attack_cooldown

    match last_direction:
        Vector2.RIGHT:
            $AttackArea.position = Vector2(20, 0)
            animated_sprite.play("attack_right")
        Vector2.LEFT:
            $AttackArea.position = Vector2(-20, 0)
            animated_sprite.play("attack_left")
        Vector2.DOWN:
            $AttackArea.position = Vector2(0, 20)
            animated_sprite.play("attack_down")
        Vector2.UP:
            $AttackArea.position = Vector2(0, -20)
            animated_sprite.play("attack_up")

    $AttackArea/CollisionShape2D.set_deferred("disabled", false)
```

> 🧱 On positionne l'attaque devant le joueur selon la direction (haut, bas, gauche, droite). Le sprite joue aussi l’animation correspondante !

---

```gdscript
#-----GESTION DES COLLISIONS AVEC LES ENNEMIS 🎯-----
func _on_attack_area_body_entered(body: Node2D) -> void:
    if body.is_in_group("enemies") and body.has_method("take_damage"):
        body.take_damage(attack_damage, global_position)
```

> 🩸 Quand un ennemi entre dans la zone d’attaque, on appelle sa méthode `take_damage`. Il faut donc bien que l'ennemi soit dans le groupe `"enemies"` et possède une fonction `take_damage`.

---

## 🧟‍♂️ Configuration du Slime

### Structure des Nœuds

```
SlimeBlue (CharacterBody2D)
├── AnimatedSprite2D
├── CollisionShape2D
└── HitBox (Area2D)
    └── CollisionShape2D
```

> 💡 Le `HitBox` est utilisé pour détecter si le joueur touche le slime. C'est cette zone qui sera ciblée par l'attaque.

---

### Code du Slime (slime_blue.gd)

```gdscript
extends CharacterBody2D

#-----VARIABLES DE BASE 🎮-----
@export var max_health = 3
var current_health

#-----SYSTÈME DE DÉGÂTS ET KNOCKBACK 💥-----
var is_hurt = false
var knockback_force = 100
var knockback_duration = 0.15
var knockback_timer = 0.0
```

> ❤️ `current_health` permet de suivre les points de vie restants. Le knockback sert à repousser l’ennemi après un coup.

---

```gdscript
#-----INITIALISATION DU MOB 🔄-----
func _ready() -> void:
    current_health = max_health
    add_to_group("enemies")
```

> 📌 Important : sans le `add_to_group("enemies")`, le joueur ne pourra pas détecter le slime comme cible valide.

---

```gdscript
#-----SYSTÈME DE COMBAT ET DÉGÂTS 🗡️-----
func take_damage(amount, attacker_position):
    if is_hurt:
        return

    current_health -= amount
    print("Slime touché! PV restants: ", current_health, "/", max_health)

    is_hurt = true
    modulate = Color(1,0.5,0.5)
    knockback_timer = knockback_duration

    var knockback_direction = (global_position - attacker_position).normalized()
    velocity = knockback_direction * knockback_force

    if current_health <= 0:
        die()
```

> 💥 Le slime devient rouge quand il prend un coup, recule avec le knockback, et meurt si sa vie tombe à zéro.

---

```gdscript
#-----MORT DU SLIME 💀-----
func die():
    $CollisionShape2D.set_deferred("disabled", true)
    $HitBox/CollisionShape2D.set_deferred("disabled", true)

    animations.play("death")
    var tween = create_tween()
    tween.tween_property(self, "modulate", Color(1, 1, 1, 0), 0.5)

    await tween.finished
    queue_free()
```

> ☠️ On désactive les collisions, on joue une animation de mort, puis on fait disparaître le slime avec un fondu.

---

## 🧩 Mise en place des collisions

# Configuration des Collisions 🎯

Dans **Project Settings > Layer Names > 2D Physics**, configure les couches comme ceci :

- Layer 1 : Player  
- Layer 2 : Enemies  
- Layer 3 : PlayerAttack  
- Layer 4 : EnemyAttack

---

### Collision Layer / Mask

| Élément              | Layer          | Mask                  |
|----------------------|----------------|------------------------|
| Player               | 1              | 2, 4                   |
| Player HurtBox       | 0              | 4                      |
| Player AttackArea    | 3              | 2                      |
| Slime                | 2              | 1, 3                   |
| Slime HitBox         | 4              | 1                      |

> 📐 Le layer détermine où l'objet **est**, et le mask détermine **ce qu'il touche** !

### Explications des Collisions

1. **Player** :
   - Layer 1 : Il est sur la couche Player
   - Mask 2, 4 : Il peut toucher les ennemis et leurs attaques

2. **Player HurtBox** :
   - Layer 0 : Pas de layer (juste une zone de détection)
   - Mask 4 : Détecte uniquement les attaques ennemies

3. **Player AttackArea** :
   - Layer 3 : Zone d'attaque du joueur
   - Mask 2 : Peut toucher les ennemis

4. **Slime** :
   - Layer 2 : Il est sur la couche Enemies
   - Mask 1, 3 : Peut toucher le joueur et ses attaques

5. **Slime HitBox** :
   - Layer 4 : Zone d'attaque ennemie
   - Mask 1 : Ne peut toucher que le joueur

---

## 🎁 Bonus et Améliorations

### Effets Visuels 🎨

- 💫 Particules lors des coups
- 🔴 Flash rouge quand le slime est touché
- ✨ Animation de disparition

### Sons 🔊

- 👊 Son d’attaque
- 😵 Son de dégât
- 💀 Son de mort

### Équilibrage ⚖️

- `attack_damage`
- `max_health`
- `knockback_force`
- `knockback_duration`

### Debug 🔧

- 🔍 Active "Visible Collision Shapes" dans le menu Debug
- 📝 Ajoute des `print()` pour tester
- 🧪 Vérifie les groupes/couches

---

🧾 _Guide créé pour le projet RPG Godot - 2024_


---

## 📜 Code Complet du Joueur

```gdscript
extends CharacterBody2D

#-----RÉFÉRENCES ET VARIABLES DE BASE 🎮-----
@onready var animated_sprite: AnimatedSprite2D = $AnimatedSprite2D
@export var vitesse: float = 40.0
@export var hurt_cooldown_duration: float = 0.8 # durée entre deux dégâts
@export var attack_damage: int = 1 # Dégâts infligés par attaque

#-----GESTION DES ÉTATS DU JOUEUR ⏱️-----
var hurt_cooldown_timer: float = 0.0
var direction: Vector2 = Vector2.ZERO
var last_direction: Vector2 = Vector2.DOWN # direction par défaut
var is_attacking: bool = false
var attack_cooldown := 0.4
var attack_timer := 0.0

#-----SIGNAUX DU JOUEUR 📡-----
signal player_died # Signal pour indiquer que le joueur est mort
signal player_hurt # Signal pour indiquer que le joueur a été touché

#-----INITIALISATION DU JOUEUR 🔄-----
func _ready() -> void:
	# S'assurer que la zone d'attaque est désactivée au départ
	$AttackArea/CollisionShape2D.set_deferred("disabled", true)
	print("Position joueur: ", global_position)
	print("Position zone d'attaque: ", $AttackArea.global_position)

#-----BOUCLE PRINCIPALE DE PHYSIQUE ⚙️-----
func _physics_process(delta: float) -> void:
	if is_attacking:
		attack_timer -= delta
		if attack_timer <= 0:
			is_attacking = false
			# Désactiver la zone d'attaque quand l'attaque est terminée
			$AttackArea/CollisionShape2D.set_deferred("disabled", true)
	else:
		if Input.is_action_just_pressed("attack"):
			start_attack()
		else:
			move_player()
	
	# ⏱️ Mise à jour du timer de blessure
	if hurt_cooldown_timer > 0:
		hurt_cooldown_timer -= delta
	
	handleCollision()
	animation_player()
	move_and_slide()

#-----SYSTÈME D'ATTAQUE 🗡️-----
func start_attack():
	is_attacking = true
	attack_timer = attack_cooldown
	
	# Positionner la zone d'attaque selon la direction
	if last_direction.x > 0:
		animated_sprite.play("attack_right")
		$AttackArea.position = Vector2(20, 0)  # Ajuste cette valeur selon la taille de ton joueur
		# Option: tu peux aussi faire pivoter la forme de collision si besoin
		# $AttackArea/CollisionShape2D.rotation = 0
	elif last_direction.x < 0:
		animated_sprite.play("attack_left")
		$AttackArea.position = Vector2(-20, 0)
		# $AttackArea/CollisionShape2D.rotation = PI
	elif last_direction.y > 0:
		animated_sprite.play("attack_down")
		$AttackArea.position = Vector2(0, 20)
		# $AttackArea/CollisionShape2D.rotation = PI/2
	elif last_direction.y < 0:
		animated_sprite.play("attack_up")
		$AttackArea.position = Vector2(0, -20)
		# $AttackArea/CollisionShape2D.rotation = -PI/2
	
	# Activer la zone d'attaque
	$AttackArea/CollisionShape2D.set_deferred("disabled", false)
	
	# Jouer un son d'attaque si disponible
	if has_node("AttackSound"):
		$AttackSound.play()

#-----GESTION DU MOUVEMENT 🚶-----
func move_player():
	direction = Input.get_vector("left", "right", "up", "down")
	# 🧠 On garde en mémoire la dernière direction utilisée
	if direction != Vector2.ZERO:
		last_direction = direction
	velocity = direction * vitesse

#-----GESTION DES ANIMATIONS 🎬-----
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

#-----GESTION DES COLLISIONS 💥-----
func handleCollision():
	for i in get_slide_collision_count():
		var collision = get_slide_collision(i)
		var collider = collision.get_collider()
		print_debug(collider.name)

#-----SYSTÈME DE SANTÉ ET DÉGÂTS 💔-----
func _on_hurt_box_area_entered(area):
	if hurt_cooldown_timer <= 0 and HealthManager.current_health > 0:
		HealthManager.lose_health()
		$HurtSound.play()
		emit_signal("player_hurt")
		hurt_cooldown_timer = hurt_cooldown_duration
		if HealthManager.current_health == 0:
			handle_player_death()
	else:
		print("Touché, mais cooldown actif ou déjà mort.")

#-----MORT DU JOUEUR 💀-----
func handle_player_death():
	print("Player is dead!")
	animated_sprite.play("dead")
	$Defeat.play()
	emit_signal("player_died")
	velocity = Vector2.ZERO
	set_physics_process(false)
	await get_tree().create_timer(1.5).timeout # délai avant l'écran
	get_tree().change_scene_to_file("res://ui/game_over.tscn")

#-----IMPACT DES ATTAQUES SUR LES ENNEMIS 🎯-----
func _on_attack_area_body_entered(body: Node2D) -> void:
	print("Position du joueur: ", global_position)
	print("Position de la zone d'attaque: ", $AttackArea.global_position)
	print("Position du slime: ", body.global_position)
	print("Collision détectée avec: ", body.name)  # Pour debug
	if body.is_in_group("enemies") and body.has_method("take_damage"):
		print("Ennemi touché! Inflige ", attack_damage, " dégâts")  # Pour debug
		# Infliger des dégâts au mob
		body.take_damage(attack_damage, global_position)
		print(attack_damage)
		# Option: effet visuel ou son lors d'un coup réussi
		if has_node("HitEnemySound"):
			$HitEnemySound.play()


```

## 📜 Code Complet du Slime

```gdscript
extends CharacterBody2D

#-----VARIABLES DE BASE 🎮-----
@export var speed = 20
@export var limit = 0.5
@export var endPoint: Marker2D
@export var movement_axis: Vector2 = Vector2.RIGHT  # Nouvelle variable pour définir l'axe de mouvement
@onready var animations = $AnimatedSprite2D

#-----SYSTÈME DE SANTÉ ❤️-----
@export var max_health = 3
var current_health

#-----DÉPLACEMENT ET POSITIONS 🚶-----
var startPosition
var endPosition

#-----SYSTÈME DE DÉGÂTS ET KNOCKBACK 💥-----
var is_hurt = false
var knockback_force = 100
var knockback_duration = 0.15
var knockback_timer = 0.0

#-----INITIALISATION DU MOB 🔄-----
func _ready() -> void:
	startPosition = position
	endPosition = endPoint.global_position
	current_health = max_health
	add_to_group("enemies")  # Important pour que le slime soit détecté par les attaques

#-----GESTION DU MOUVEMENT 🔀-----
func changeDirection():
	# Inverse les points de départ et d'arrivée pour faire un aller-retour
	var tempEnd = endPosition
	endPosition = startPosition
	startPosition = tempEnd

func updateVelocity():
	# Si en knockback, ne pas mettre à jour la vélocité
	if is_hurt:
		return
		
	var moveDirection = (endPosition - position)
	if moveDirection.length() < limit:
		changeDirection()
		
	velocity = moveDirection.normalized() * speed
	
#-----GESTION DES ANIMATIONS 🎬-----
func updateAnimation():
	# Ne pas changer l'animation si le slime est blessé
	if is_hurt:
		return
		
	# Déterminer l'animation selon la direction du mouvement
	var animationString = "walk_"
	if abs(velocity.x) > abs(velocity.y):
		animationString += "right" if velocity.x > 0 else "left"
	else:
		animationString += "down" if velocity.y > 0 else "up"
	
	# Vérifier si l'animation existe, sinon utiliser une animation par défaut
	if animationString in animations.sprite_frames.get_animation_names():
		animations.play(animationString)
	else:
		animations.play("walk_down")  # Animation par défaut si l'animation spécifique n'existe pas
		
#-----BOUCLE PRINCIPALE DE PHYSIQUE ⚙️-----
func _physics_process(delta: float) -> void:
	# Gérer le knockback
	if is_hurt:
		knockback_timer -= delta
		if knockback_timer <= 0:
			is_hurt = false
			modulate = Color(1,1,1)  # Remettre la couleur normale
			
	updateVelocity()
	move_and_slide()
	updateAnimation()

#-----SYSTÈME DE COMBAT ET DÉGÂTS 🗡️-----
# Fonction pour gérer les dégâts
func take_damage(amount, attacker_position):
	if is_hurt:
		return # Éviter de prendre des dégâts multiples rapidement
		
	current_health -= amount
	print("Slime touché! PV restants: ", current_health, "/", max_health)  # Debug
	# Effet visuel de dégât
	is_hurt = true
	modulate = Color(1,0.5,0.5) # Rougir le slime
	knockback_timer = knockback_duration
	
	# Calculer direction du knockback (s'éloigner de l'attaquant)
	var knockback_direction = (global_position - attacker_position).normalized()
	velocity = knockback_direction * knockback_force
	
	# Jouer un son de dégât si disponible
	if has_node("HurtSound"):
		$HurtSound.play()
		
	# Vérifier si le slime est mort
	if current_health <= 0:
		die()	
	
	# Afficher des dégâts flottants (optionnel)	
	spawn_damage_number(amount)	
	
#-----MORT DU SLIME 💀-----
func die():
	print("Tentative de mort du slime")
	# Désactiver les collisions immédiatement
	$CollisionShape2D.set_deferred("disabled", true)
	$HitBox/CollisionShape2D.set_deferred("disabled", true)
	
	# Jouer l'animation de mort
	animations.play("death")
	
	# Effet de fade out
	var tween = create_tween()
	tween.tween_property(self, "modulate", Color(1, 1, 1, 0), 0.5)  # Fade out sur 0.5 secondes
	
	# Attendre la fin du fade
	await tween.finished
	
	print("Fade out terminé")
	queue_free()  # Supprimer le slime

#-----EFFETS VISUELS ✨-----
func spawn_damage_number(amount):
	# Tu pourrais créer une scène séparée pour ça et l'instancier ici
	# Par exemple:
	# var damage_popup = preload("res://damage_popup.tscn").instantiate()
	# damage_popup.amount = amount
	# damage_popup.position = position + Vector2(0, -20)
	# get_parent().add_child(damage_popup)
	pass


func _on_hit_box_area_entered(area: Area2D) -> void:
	if area.get_parent().name == "Player":
		area.get_parent()._on_hurt_box_area_entered($HitBox)

	pass

```
