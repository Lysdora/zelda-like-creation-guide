# 👾 Créer un ennemi qui patrouille entre deux points dans Godot 4.4

> Ce guide fait partie de mon journal de progression Godot RPG 🧪  
> Il décrit comment j'ai créé un slime qui se déplace automatiquement entre deux points, avec des animations mignonnes et une personnalisation facile ✨

---

## 🎯 Objectif

Créer un **ennemi (ex: slime)** capable de :

- Se déplacer automatiquement entre deux points
- Jouer une animation de déplacement cohérente (haut/bas/gauche/droite)
- Être **personnalisable** via l'inspecteur (axe, vitesse, sens)
- Réagir quand il arrive au bout (il change de direction)

---

## 🧠 Script complet

```gdscript
extends CharacterBody2D

@export var speed = 20
@export var limit = 0.5
@export var endPoint: Marker2D
@export var movement_axis: Vector2 = Vector2.RIGHT  # Nouvelle variable pour définir l'axe de mouvement

@onready var animations = $AnimatedSprite2D

var startPosition
var endPosition

func _ready() -> void:
	startPosition = position
	endPosition = endPoint.global_position

func changeDirection():
	var tempEnd = endPosition
	endPosition = startPosition
	startPosition = tempEnd

func updateVelocity():
	var moveDirection = (endPosition - position)
	if moveDirection.length() < limit:
		changeDirection()
		
	velocity = moveDirection.normalized() * speed
	
func updateAnimation():
	var animationString = "walk_"
	if abs(velocity.x) > abs(velocity.y):
		animationString += "right" if velocity.x > 0 else "left"
	else:
		animationString += "down" if velocity.y > 0 else "up"
	
	if animationString in animations.sprite_frames.get_animation_names():
		animations.play(animationString)
	else:
		animations.play("walk_down")  # Animation par défaut si l'animation spécifique n'existe pas
		
func _physics_process(delta: float) -> void:
	updateVelocity()
	move_and_slide()
	updateAnimation()
```

---

## ✨ Explications

| Élément | Rôle |
|--------|------|
| `@export var speed` | Définit la vitesse de déplacement de l’ennemi |
| `@export var limit` | Distance en-dessous de laquelle l’ennemi inverse sa direction |
| `@export var endPoint` | Le point cible que l’ennemi doit atteindre (souvent un `Marker2D`) |
| `@export var movement_axis` | Permet d’orienter différemment l’ennemi (par défaut à droite) |
| `animations.play()` | Joue automatiquement la bonne animation selon la direction |
| `changeDirection()` | Intervertit `startPosition` et `endPosition` pour faire des allers-retours |

---

## 🎨 Animations requises

Ton `AnimatedSprite2D` doit contenir les animations suivantes :
- `walk_up`
- `walk_down`
- `walk_left`
- `walk_right`

Sinon, le slime jouera l’animation `walk_down` par défaut s’il ne trouve pas la bonne.

---

## 🧪 Astuce bonus

Tu peux créer **plusieurs slimes différents** dans ton jeu simplement en :
- Modifiant la `vitesse`
- Changeant le `endPoint` dans l’éditeur
- Changeant l’axe (`movement_axis`) si tu veux qu’un slime se déplace **verticalement** ou en diagonale 🌀

---

## 📅 Date de rédaction

*Guide rédigé le 12/04/2025*

---

🐾 *Ce guide fait partie de mon projet de RPG Godot 4.4.  
D’autres guides sont disponibles ici :*  
🔗 [Retour à l'accueil](../index.md)
