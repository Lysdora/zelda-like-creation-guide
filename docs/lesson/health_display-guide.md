# 💓 Afficher les points de vie au-dessus des ennemis

## 🎯 Objectif
Apprendre à créer et gérer un affichage des points de vie au-dessus des ennemis, comme dans de nombreux jeux RPG.

## 🧩 Concepts clés
- Création d'une scène UI simple
- Communication entre différents nœuds
- Mise à jour d'interface en temps réel
- Hiérarchie parent-enfant

## 🔍 Étape 1 : Créer la scène d'affichage des points de vie

1. Crée une nouvelle scène avec un nœud racine **Node2D** et nomme-la `damage_popup`
2. Ajoute un **Panel** comme enfant (optionnel, pour l'arrière-plan)
3. Ajoute un **Label** comme enfant du Panel et nomme-le `label_vie`
4. Configure le Label :
   - Texte : "3/3" (ou valeur par défaut)
   - Police : choisir une taille et un style lisibles
   - Ajoute un contour pour améliorer la visibilité sur tous les fonds

> 💡 **Conseil** : Pour un style pixel art, utilise une police bitmap qui correspond à l'esthétique de ton jeu !

## 🔍 Étape 2 : Ajouter un script à la scène damage_popup

Crée un nouveau script pour le nœud racine `damage_popup` :

```gdscript
extends Node2D

# Variables pour stocker les valeurs de santé
var max_health: int = 3
var current_health: int = max_health

# Référence au label
@onready var label = $Panel/label_vie

# Fonction appelée au démarrage
func _ready() -> void:
    print("Damage Pop Up Initialize !")

# Fonction pour mettre à jour l'affichage des points de vie
func update_health(current_health, max_health):
    # Met à jour le texte du label avec les valeurs actuelles
    label.text = str(current_health) + "/" + str(max_health)
```

## 🔍 Étape 3 : Ajouter l'affichage à l'ennemi

1. Ouvre ta scène d'ennemi (par exemple, "slime_blue.tscn")
2. Glisse-dépose la scène `damage_popup.tscn` comme enfant de ton ennemi
3. Positionne-la au-dessus de ton ennemi (par exemple, y = -20)

## 🔍 Étape 4 : Mettre à jour les points de vie depuis le script de l'ennemi

Dans le script de ton ennemi (comme `slime_blue.gd`), ajoute les appels à la fonction `update_health` :

```gdscript
# Dans la fonction _ready()
func _ready() -> void:
    startPosition = position
    endPosition = endPoint.global_position
    current_health = max_health
    add_to_group("enemies")
    
    # Initialiser l'affichage des points de vie
    $damage_popup.update_health(current_health, max_health)

# Dans la fonction take_damage()
func take_damage(amount, attacker_position):
    if is_hurt:
        return
        
    current_health -= amount
    print("Slime touché! PV restants: ", current_health, "/", max_health)
    
    # Mettre à jour l'affichage des points de vie
    $damage_popup.update_health(current_health, max_health)
    
    # Reste du code de la fonction...
```

## 🔍 Étape 5 (Optionnel) : Améliorer l'interface

Tu peux améliorer ton affichage en :

1. Remplaçant le texte par une barre de vie graphique avec **TextureProgressBar**
2. Ajoutant des animations quand les PV changent (clignotement, scaling, etc.)
3. Faisant apparaître les PV uniquement quand l'ennemi est touché

## 🎮 Comment ça fonctionne ?

1. L'affichage est une scène séparée avec son propre script
2. Cette scène est un enfant de l'ennemi, donc elle se déplace automatiquement avec lui
3. Quand l'ennemi prend des dégâts, il appelle la fonction `update_health` de l'affichage
4. L'affichage met à jour le texte du label pour refléter les PV actuels

## 💡 Conseils & Astuces

- **Positionnement** : Ajuste la position de l'affichage pour qu'il soit bien visible sans gêner le gameplay
- **Visibilité conditionnelle** : Tu peux faire en sorte que les PV ne s'affichent que lorsque l'ennemi est blessé
- **Animations** : Ajoute des tweens pour faire clignoter ou rebondir l'affichage quand les PV changent
- **Optimisation** : Pour les jeux avec beaucoup d'ennemis, considère désactiver l'affichage pour les ennemis hors écran

## 🧠 Concepts avancés à explorer

- Système de dégâts flottants (chiffres qui montent puis disparaissent)
- Couleurs dynamiques (vert → jaune → rouge selon le niveau de vie)
- Effets de particules lorsque les PV descendent en dessous de certains seuils

---

## 📝 À retenir

- L'UI est un élément crucial du game feel - des affichages de PV bien faits rendent le jeu plus satisfaisant
- La communication entre scripts se fait par appels de fonctions et références aux nœuds
- Godot facilite la création d'interfaces hiérarchiques (parent-enfant)
- La mise à jour des éléments visuels doit se faire aux moments appropriés (initialisation, dégâts, etc.)

N'hésite pas à personnaliser ce système pour qu'il corresponde parfaitement à l'ambiance de ton jeu ! 🎮✨
