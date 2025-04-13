# ❤️ Créer une UI de cœurs pour afficher la vie du joueur (Godot 4.4)

> Ce guide fait partie de mon journal de progression Godot RPG 🧪  
> Ici, je crée une interface pour afficher les points de vie du joueur avec des **cœurs pixelisés** trop mignons ❤️✨

---

## 🎯 Objectif

Créer une **UI stylée** qui montre la vie du joueur à l’écran avec des cœurs animés.

---

## 🛠️ Étapes détaillées

### 🧱 Étape 1 — Créer le conteneur principal

1. **Créer une nouvelle scène**
2. Ajouter un `HBoxContainer`
3. Renommer ce node `HeartsContainer`
4. Sauvegarder la scène sous le nom **HeartsContainer.tscn**

> 🧩 Ce conteneur affichera tous les cœurs en ligne (un par PV du joueur).

---

### 🧸 Étape 2 — Créer la scène pour un cœur

1. Créer une **nouvelle scène vide**
2. Ajouter un `Panel` comme node principal
3. Renommer cette scène en **HeartGUI**
4. Ajouter un `Sprite2D` en tant qu’enfant du `Panel`
5. Dans l’inspecteur du Sprite :

| Propriété | Valeur |
|-----------|--------|
| **Texture** | (mettre ton sprite de cœur ici 💖) |
| **Offset > Centered** | ❌ décoché |
| **Animation > Hframes** | `5` |
| **Animation > Vframes** | `4` (si c’est 4 lignes dans l’image) |

6. Ajuster la taille du `Panel` pour correspondre à celle du sprite :
   - Dans l’onglet **2D**, redimensionne manuellement le `Panel`
   - Puis déplace le Sprite pour qu’il s’aligne parfaitement dans le `Panel`

---

### 🧩 Étape 3 — Astuce de mise en page

Dans le `Panel` :
- Clique sur le menu **Layout**
- Choisis `Full Rect`
- Clique ensuite sur la petite flèche à droite du `Rect` → **Copy Rect**
- Va dans **"Custom Minimum Size"** et **colle** cette taille

🎯 Résultat : ton `HeartGUI` aura **toujours la bonne taille**, bien calée dans la ligne du `HBoxContainer`.

---

## 💡 Astuce

Une fois la scène `HeartGUI` terminée, tu peux en **ajouter plusieurs** comme enfants du `HeartsContainer` pour afficher la vie du joueur, par exemple via un script global comme `GameState`.

---

## 🔗 Ressources

🎨 Sprite de cœur utilisé :

![Sprite de cœur](./images/heart.png)


---

## 📅 Date de rédaction

*Guide rédigé le 12/04/2025*

---

🐾 *Ce guide fait partie de mon projet de RPG Godot 4.4.*  
🔗 [Retour à l'accueil](index.md)

