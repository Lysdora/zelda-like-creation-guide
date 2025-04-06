# 🎒 Création d'un Système d'Inventaire dans Godot 4.4.1

Bienvenue aventurière du pixel ! 🌟 Aujourd'hui, nous allons créer un système d'inventaire simple et élégant pour ton jeu RPG Zelda-like. Tu verras, c'est super pratique et les **Resources** de Godot vont rendre ça ultra flexible ! 🚀

---

## 📌 Objectif

Créer un **système d'inventaire** utilisant les **Resources** pour stocker et gérer des items. Cela permettra de **charger facilement des objets**, de les manipuler dans plusieurs endroits de ton jeu, et d'afficher leur contenu dans ton interface utilisateur. 🎮

---

## 📁 Préparation

- **Godot 4.4.1**
- **Dossier dédié : `inventory/`**
- **Textures pour les items (ex : potion, épée)**

---

## 🌟 Étape 1 : Création de l'Inventaire (inventory.gd)

1. **Créer un nouveau script** :

   - `File` → `New Script`
   - **Extends : `Resource`**.
   - **Nom : `inventory.gd`**
   - **Dossier : `res://inventory/`**

2. **Ajouter une classe pour facilement gérer l’inventaire :**

```gdscript
extends Resource

class_name Inventory  # Facilite l'utilisation de l'inventaire par d'autres scripts

@export var items: Array[InventoryItem]  # Liste d'items dans l'inventaire
```

Pourquoi utiliser une `class_name` ? 🤔  
✅ Cela permet de référencer l'inventaire facilement dans d'autres scripts, comme ton joueur ou l'UI d'inventaire.

---

## 🌟 Étape 2 : Création des Items (inventoryItem.gd)

1. **Créer un nouveau script** :

   - `File` → `New Script`
   - **Extends : `Resource`**.
   - **Nom : `inventoryItem.gd`**
   - **Dossier : `res://inventory/`**

2. **Ajouter une classe pour les items :**

```gdscript
extends Resource

class_name InventoryItem  # Facilite l'utilisation des items dans d'autres scripts

@export var name: String  # Nom de l'item
@export var texture: Texture2D  # Texture de l'item
```

---

## 🌟 Étape 3 : Création des Items Spécifiques

1. **Créer un dossier `items`** dans `res://inventory/`.
2. **Créer une ressource `InventoryItem` :**

   - Dans l'inspecteur, clique sur le dossier avec le petit `+` vert.
   - Recherche `InventoryItem`.
   - Remplis les champs comme suit :

     - **Name : `lifepot`**
     - **Texture :** Glisse-dépose la texture de la potion.

   - **Sauvegarde sous : `res://inventory/items/lifepot.tres`**.

   ![🎒 Texture d'inventaire](https://lysdora.github.io/zelda-like-creation-guide/images/inventory_resource.png)

3. **Répète l’opération pour d’autres items (ex: `sword`)**.

---

## 🌟 Étape 4 : Créer une Resource d’Inventaire pour le Joueur

1. **Créer une nouvelle resource `Inventory` :**

   - Nom : `playerInventory.tres`
   - Dossier : `res://inventory/`

2. **Ajouter des items :**

   - Clique sur l’`Array` dans l’inspecteur.
   - Glisse-dépose `lifepot.tres` et `sword.tres` dans l’inventaire.

   ![📦 Inventory Player](https://lysdora.github.io/zelda-like-creation-guide/images/inventoy_player.png)

   - Sauvegarde bien !

---

## 🌟 Étape 5 : Lier l’Inventaire au Joueur

1. **Ajouter cette ligne au script de ton joueur (`Player.gd`) :**

```gdscript
@export var inventory: Inventory  # Associer l'inventaire au joueur
```

2. **Dans la scène du joueur :**

   - Tu verras un champ dédié pour ajouter l’inventaire dans l’inspecteur.
   - Lie `playerInventory.tres`.

   ![📦 Inventory Player 2](https://lysdora.github.io/zelda-like-creation-guide/images/inventoy_player_2.png)

---

## 🌟 Étape 6 : Interface Graphique (Slot GUI)

1. **Dans la scène des slots :**

   - Renomme ton `Sprite2D` contenant les slots : `background`.
   - Ajoute un `Sprite2D` en tant qu'enfant et renomme-le : `item`.

2. **Script pour chaque slot (`slot_gui.gd`) :**

```gdscript
extends Node

@onready var backgroundSprite: Sprite2D = $background
@onready var itemSprite: Sprite2D = $item

func update_item(item: InventoryItem) -> void:
    if !item:
        backgroundSprite.frame = 0
        itemSprite.visible = false
    else:
        backgroundSprite.frame = 1
        itemSprite.texture = item.texture
        itemSprite.visible = true
```

---

## 🌟 Étape 7 : Centrage des Items dans les Slots

1. **Ajoute un `CenterContainer` en tant qu'enfant du `RootNode`.**
2. **Ajoute un `Panel` en tant qu'enfant du `CenterContainer`.**
3. **Déplace ton `item` (le Sprite2D) à l'intérieur du `Panel`.**
4. **Ajuste la taille du `CenterContainer` pour correspondre à ses enfants.**
5. **Dans l'inspecteur, utilise `Layout → Custom Min Size` pour ajuster la taille.**
6. **Réinitialise la position du `Sprite2D` `item` à `(0, 0)` pour qu'il soit bien centré.**

💡 N'oublie pas de **mettre à jour les chemins** si nécessaire, comme par exemple :

```gdscript
@onready var itemSprite: Sprite2D = $CenterContainer/Panel/item
```

![📦 Inventory Player 3](https://lysdora.github.io/zelda-like-creation-guide/images/inventoy_player_3.png)

---

## 🌟 Conclusion

🎉 Bravo ! Tu as maintenant un inventaire qui affiche dynamiquement les items collectés, parfaitement centrés dans les slots. C’est clean et ça marche super bien ! 💪

**Continue de t’éclater dans ton développement !** 🌸🐱‍👤
