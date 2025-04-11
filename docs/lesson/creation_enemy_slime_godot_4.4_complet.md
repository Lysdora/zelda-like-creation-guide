# 🧪 Création d’un Ennemi Slime 🟦 (Godot 4.4, Débutants)

Apprenons à créer un **ennemi basique**, le célèbre **Slime Bleu**, qui patrouille tranquillement entre deux points dans ta map !  
Parfait pour poser les bases d’un système de monstres 🧌 dans ton RPG Zelda-like ✨

---

## 🎯 Objectif

Créer un **slime animé** qui :

✅ Se déplace automatiquement d’un point A à un point B  
✅ Change de direction automatiquement  
✅ Joue une animation selon son sens de déplacement  
✅ Peut avoir une destination personnalisée via l’éditeur Godot

---

## 🎨 Étape 1 : Préparer la scène du Slime

1. Crée une **nouvelle scène** `CharacterBody2D` (clic droit > New Scene > Node2D → remplace par `CharacterBody2D`)
2. Renomme-la `SlimeBlue`
3. Ajoute un **enfant `AnimatedSprite2D`**
4. Glisse tes sprites de slime (idle ou marche) dans l’inspecteur :
   - Crée deux animations : `walk_up` et `walk_down` *(tu pourras en ajouter plus après !)*

---

## ✍️ Étape 2 : Attacher un script au Slime

Clic droit sur `SlimeBlue` > Attach Script ➕  
Colle le script suivant :

```gdscript
extends CharacterBody2D

@export var speed = 20                  # Vitesse de déplacement
@export var limit = 0.5                 # Distance à partir de laquelle on fait demi-tour
@export var endPoint: Marker2D          # Destination cible définie depuis la scène

@onready var animations = $AnimatedSprite2D

var startPosition
var endPosition

func _ready() -> void:
	startPosition = position                              # Position de départ du slime
	endPosition = endPoint.global_position                # Position du Marker2D placé dans la scène

func changeDirection():
	var tempEnd = endPosition
	endPosition = startPosition
	startPosition = tempEnd

func updateVelocity():
	var moveDirection = (endPosition - position)          # Direction à suivre
	if moveDirection.length() < limit:
		changeDirection()
		
	velocity = moveDirection.normalized() * speed         # Déplacement vers l’endroit cible

func updateAnimation():
	var animationString = "walk_up"
	if velocity.y > 0:
		animationString = "walk_down"
	animations.play(animationString)

func _physics_process(delta: float) -> void:
	updateVelocity()
	move_and_slide()
	updateAnimation()
```

---

## 📌 Étape 3 : Ajouter le Marker2D dans la scène principale

1. Ouvre la **scène de ton niveau** (ou une scène vide pour tester)
2. Instancie `SlimeBlue` dedans (clic droit → Instance Child Scene)
3. Sélectionne `SlimeBlue` et ajoute-lui un **enfant `Marker2D`**
4. Bouge ce `Marker2D` où tu veux que le slime se rende
5. Dans l’inspecteur de `SlimeBlue`, **assigne le `Marker2D` dans la variable `endPoint`**

---

## 🧠 Détail des variables et logique

| Variable         | Description |
|------------------|-------------|
| `speed`          | La vitesse de déplacement du slime |
| `limit`          | Quand le slime est à moins de `limit` pixels de sa cible, il change de direction |
| `endPoint`       | Le point final que le slime doit atteindre, défini depuis l’éditeur |
| `startPosition`  | Mémorise la position de départ |
| `endPosition`    | Mémorise la destination |
| `velocity`       | Direction actuelle multipliée par la vitesse |

---

## 🧠 Pourquoi utiliser un Marker2D ?

Parce que ça permet aux débutants de **visualiser facilement** où va le slime, **sans coder** !  
Tu peux déplacer le Marker2D directement dans l’éditeur 🎯

C’est aussi une première étape vers des **patrouilles complexes** ou des **zones intelligentes** 👀

---

## 🧪 Résultat attendu

Quand tu lances la scène :

- Le slime commence à sa position d’origine
- Il se déplace **vers le Marker2D**
- Une fois arrivé, il fait **demi-tour** automatiquement
- Et il repart, **en boucle** ♻️

---

## 🧁 Et ensuite ?

Voici quelques idées de suites pour t’amuser avec ton slime :

- Ajouter une **Zone2D** pour détecter Elara 🤺
- Lui ajouter une **Hitbox** et des **points de vie**
- L’animer aussi sur les côtés (`walk_left`, `walk_right`)
- Jouer un **son rigolo** quand il change de direction 🎵

---

## 🌸 Conclusion

Félicitations ! 🎉  
Tu viens de créer ton premier **ennemi vivant**, qui bouge tout seul et réagit à sa position 👏

Un petit pas pour le slime… un grand pas pour ton RPG 🧝‍♀️

---

👩‍💻 Écrit par **Lysdora**  
✨ Projet : [mon-rpg-zelda](https://lysdora.github.io/mon-rpg-zelda/)
