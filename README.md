# UGAS - Unity Gameplay Ability System

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)


A flexible and extensible Gameplay Ability System for Unity, inspired by Unreal Engine's GAS. This framework is designed to provide a solid foundation for managing abilities, attributes, and gameplay effects in any Unity project.


---

## ✨ Features


* **Attribute System:** Define custom attributes like Health, Mana, Stamina with features like min/max values and regeneration.
* **Abilities & Effects:** Create complex abilities with costs, cooldowns, cast times, and targeting systems (Self, Target, Area).
* **Gameplay Effects:** Apply both instant and duration-based effects to modify attributes (e.g., buffs, debuffs).
* **Stacking Effects:** Control how effects can stack on a target.
* **Tag System:** A powerful tag system (`GameplayTag`) to manage states, immunities, and triggers.
* [cite_start]**Custom Editors:** User-friendly custom inspectors for easier configuration of abilities, attributes, and effects right inside the Unity Editor. 
* **2D & 3D Support:** Designed to work for both 2D and 3D games.

---

## 🔧 Installation

1.  Go to the [Releases page](https://github.com/sajad0131/Unity-Gameplay-Ability-System/releases).
2.  Download the latest `UGAS by Sajad Amiri.unitypackage` file.
3.  Open your Unity project.
4.  Import the downloaded package by navigating to `Assets > Import Package > Custom Package...`.

---

## 🚀 How to Use


1.  **Create an Attribute:** Create new `AttributeDefinition` ScriptableObjects (e.g., Health, Mana) from the `Create > GAS > Attribute Definition` menu.
2.  **Add AttributeSet:** Add the `AttributeSet` component to your character and assign the initial attributes.
3.  **Create an Ability:** Create a new `AbilityDefinition` from the `Create > GAS > Ability Definition` menu. Configure its cost, cooldown, and effects.
4.  **Activate an Ability:** Get a reference to the `AbilitySystem` component on your character and call the `TryActivateAbility()` method.

```csharp
public AbilitySystem abilitySystem;
public AbilityDefinition fireballAbility;

void CastFireball()
{
    abilitySystem.TryActivateAbility(fireballAbility, target);
}
```


---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.