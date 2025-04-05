
# 🎒 Créer l'UI de l'Inventaire dans Godot 4.4

Dans cette leçon, nous allons créer une interface d'inventaire pour notre RPG Zelda-like. 🎮✨

Nous allons procéder par étapes pour bien comprendre chaque concept.

---

## 📌 Objectif
Créer un **inventaire visuel** où seront affichés les objets ramassés par Elara, avec une belle interface pixelisée.

## 📁 Préparation
- **Godot 4.4.1**
- **Dossier dédié : `inventory/`**

---

## 🌟 Étape 1 : Création de l'Inventaire (InventoryGui)

On va maintenant créer notre interface d'inventaire ! 👜🎒

### 📋 Création de la GUI principale

1. **Crée une nouvelle scène** de type **`Control`**.
2. **Renomme-la `InventoryGui`**.
3. **Ajoute un enfant : `NinePatchRect`** (pour éviter que le sprite se déforme quand il est étendu).

### 🎨 Configuration de l'apparence

1. **Ajoute ta texture d'inventaire** à ton `NinePatchRect` (par exemple, une fenêtre d'inventaire jolie et pixelisée).
2. **Clique sur `Edit Region`** et ajuste les **marges** de la texture pour qu'elle s'étire correctement sans se déformer.
3. **Agrandis le `NinePatchRect`** pour voir l'effet : La texture s'étend proprement ! 🎉

### 📥 Télécharger les textures

Vous pouvez télécharger les textures nécessaires ici :

- ![🎒 Texture d'inventaire](../images/InventoryRect.png)
- ![📦 Texture de slot vide](../images/InventorySlot.png)


---

## 🌟 Étape 2 : Création des Slots d'Inventaire

Maintenant, on va ajouter des **slots** pour nos objets ! 🎯

### 📦 Création des Slots

1. **Crée une nouvelle scène** avec un noeud racine de type **`Panel`**.
2. **Renomme cette scène `InventorySlot`**.
3. **Ajoute un `Sprite` en tant qu'enfant de `Panel`** pour représenter le fond de ton slot.
4. **Ajuste ton sprite** en utilisant une texture de slot vide (par exemple, un petit carré avec un contour).

🎯 Ta hiérarchie devrait ressembler à ceci :  
```
InventorySlot (Panel)
└── Sprite (représentant le slot)
```

---

## ✅ Conclusion
✨ Bravo ! Tu as créé l'interface de ton inventaire et ajouté des slots vides ! 🎒

📌 **Prochaine étape ?** Ajouter des objets collectés à ces slots, les gérer, les utiliser... Tout un programme ! 🚀

---

📥 Voici le lien du nouveau guide : [inventory_ui.md](./articles/inventory_ui.md)
