
# ✨ Configurer Visual Studio Code pour Godot 4.4.1 (GDScript)

Salut aventurière du pixel ! 🌟 Voici un petit guide simple, clair et doux pour configurer confortablement **Visual Studio Code** (VSCode) comme éditeur principal pour ton projet Godot (version 4.4.1), en utilisant GDScript. Suis ces étapes pas à pas et tout ira comme sur des roulettes ! 🚀

---

## 📌 Étape 1 : Configurer Godot

Dans Godot :

- Va dans `Editor` → `Editor Settings`.
- Recherche : `External Editor`.
- Sélectionne **Visual Studio Code**.
- Coche ✅ **Use External Editor**.
- Coche ✅ **Sync Script Changes** pour que Godot détecte tes modifications automatiquement.

Ajoute ensuite le chemin vers ton exécutable VSCode :

- **Sous Windows** :
  ```
  C:\Users\[ton-nom]\AppData\Local\Programs\Microsoft VS Code\Code.exe
  ```
- **Sous MacOS** :
  ```
  /Applications/Visual Studio Code.app
  ```
- **Sous Linux** :
  ```
  /usr/bin/code
  ```

Garde l'option `Exec Flags` telle quelle :
```
{file}
```

---

## 🔧 Étape 2 : Installer l’extension Godot Tools dans VSCode

Dans VSCode :
- Clique sur l’icône `Extensions` (Ctrl+Shift+X).
- Recherche et installe l'extension officielle :
  **Godot Tools** par `Godot Engine`.

Cette extension ajoute :
- ✅ Autocomplétion avancée
- ✅ Debugging intégré
- ✅ Documentation rapide

---

## 🧙‍♀️ Étape 3 : Activer le Language Server dans Godot

Pour une autocomplétion intelligente, active le Language Server :

- Dans Godot : `Editor` → `Editor Settings` → recherche `Language Server`.
- Active ✅ **Enable Smart Resolve**.
- Active ✅ **Show Native Symbols in Editor** pour plus de confort.
- Garde le port par défaut (`6005`).

Assure-toi que VSCode utilise le même port (`6005`) dans ses paramètres (cherche dans VSCode : `Godot Tools: Lsp Server Port`).

---

## 🚀 Étape 4 : Tester ta configuration !

Crée un script simple pour vérifier :

```gdscript
extends Node

func _ready():
    print("🌟 Configuration réussie, bravo ! 🌟")
```

Si tu vois la phrase dans la console, tout est bon ! 🥳

---

🎉 Félicitations, tu es maintenant prête à coder efficacement et agréablement avec VSCode et Godot 4.4.1 ! 

Bonne aventure dans le monde du pixel ! 🌸🐱‍👤
