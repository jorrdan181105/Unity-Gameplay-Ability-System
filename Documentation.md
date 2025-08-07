# Unity Gameplay Ability System (UGAS) Documentation

## Introduction

Welcome to the Unity Gameplay Ability System (UGAS)\! This document provides a comprehensive guide to using UGAS in your Unity projects. UGAS is a flexible and extensible framework inspired by Unreal Engine's Gameplay Ability System, designed to simplify the management of character abilities, attributes, and gameplay effects.

This system is built to be intuitive for both programmers and designers, with a strong emphasis on custom editors to make configuration as easy as possible.

-----

## Core Concepts

UGAS is built around a few key concepts. Understanding these will help you use the system effectively.

### Attributes

Attributes are the core numerical properties of your characters, such as **Health**, **Mana**, or **Stamina**.

  * **`AttributeDefinition`**: A ScriptableObject that defines a new type of attribute. Here you can set its name, description, default value, min/max values, and regeneration properties.
  * **`AttributeSet`**: A `MonoBehaviour` that you attach to your character (or any GameObject) to give it a set of attributes. You assign your `AttributeDefinition` assets to this component.
  * **`AttributeValue`**: A class that holds the runtime value of an attribute, including its base value and any modifications from effects.

**Example: Creating a Health Attribute**

1.  In the Project window, go to `Create > GAS > Attribute Definition`.
2.  Name the new asset "Health".
3.  Configure its properties, for example:
      * **Attribute Name**: Health
      * **Default Base Value**: 100
      * **Max Value**: 100
      * **Display Color**: Red

### Gameplay Effects

Gameplay Effects are the primary way you modify attributes. They can be instant (like taking damage) or have a duration (like a buff or a poison effect).

  * **`GameplayEffect`**: An abstract base `ScriptableObject` for all effects. It defines the duration, stacking behavior, and any `GameplayTags` that are granted to the target while the effect is active.
  * **`InstantModifierEffect`**: An effect that applies a one-time modification to an attribute.
  * **`DurationModifierEffect`**: An effect that applies a modification for a specific duration.
  * **`GameplayEffectRunner`**: A `MonoBehaviour` that manages the application and removal of active effects on a character.

**Example: Creating a Damage Effect**

1.  Go to `Create > GAS > Effects > Instant Modifier`.
2.  Name it "Damage\_Fire".
3.  Configure its properties:
      * **Attribute**: Drag your "Health" `AttributeDefinition` here.
      * **Type**: Flat
      * **Value**: -10 (a negative value to subtract from health)

### Abilities

Abilities are actions that a character can perform, like casting a spell or using an item.

  * **`AbilityDefinition`**: A `ScriptableObject` that defines an ability. This is where you configure everything about the ability, including:
      * **Cost**: How much of an attribute it costs to use.
      * **Cooldown**: How long before it can be used again.
      * **Cast Time**: How long it takes to activate.
      * **Targeting**: Who or what the ability affects (self, target, area, etc.).
      * **Effects**: The `GameplayEffect`s to apply on activation.
  * **`AbilitySystem`**: The central `MonoBehaviour` that manages a character's abilities. It handles activation, cooldowns, and casting.

**Example: Creating a Fireball Ability**

1.  Go to `Create > GAS > Ability Definition`.
2.  Name it "Fireball".
3.  Configure it:
      * **Cost**: 20
      * **Cost Attribute**: Drag your "Mana" attribute here.
      * **Cooldown**: 5 seconds.
      * **Targeting Type**: Target.
      * **Effects**: Add your "Damage\_Fire" effect.

### Tags

Tags are a powerful way to manage states, immunities, and other gameplay logic.

  * **`GameplayTag`**: A simple `ScriptableObject` that acts as a unique identifier.
  * **`TagSystem`**: A `MonoBehaviour` that keeps track of which tags are currently active on a GameObject. Abilities can have requirements based on tags (e.g., `requiredTags`, `blockedByTags`).

**Example: Creating a "Stunned" Tag**

1.  Go to `Create > GAS > Gameplay Tag`.
2.  Name it "State\_Stunned".
3.  You can now use this tag in your abilities, for instance, to create a "Stun" ability that grants this tag, and other abilities can be configured to be blocked if the character has the "State\_Stunned" tag.

-----

## Getting Started: A Step-by-Step Tutorial

### 1\. Installation

1.  Go to the [Releases page](https://github.com/sajad0131/Unity-Gameplay-Ability-System/releases).
2.  Download the latest `UGAS by Sajad Amiri.unitypackage` file.
3.  Open your Unity project.
4.  Import the downloaded package by navigating to `Assets > Import Package > Custom Package...`.

### 2\. Setting Up a Character

Let's set up a player character.

1.  Create a new GameObject for your player.
2.  Add the following components to it:
      * **`AttributeSet`**
      * **`GameplayEffectRunner`**
      * **`AbilitySystem`**
      * **`TagSystem`**
3.  **Create Attributes**: Create `AttributeDefinition` assets for "Health" and "Mana" as described in the Core Concepts section.
4.  **Assign Attributes**: In the `AttributeSet` component on your player, add the "Health" and "Mana" attributes to the `Initial Attributes` list.

### 3\. Creating a Fireball Ability

1.  **Create the Damage Effect**: Create an `InstantModifierEffect` named "Damage\_Fire" that reduces the "Health" attribute by 10.
2.  **Create the Cost Effect**: Create an `AbilityDefinition` for the fireball. In the "Cost" section, set the cost to 20 and the `costAttribute` to your "Mana" attribute.
3.  **Create the Ability Definition**:
      * Create a new `AbilityDefinition` and name it "Fireball".
      * Set the `Cooldown` to 5.
      * Set the `Targeting Type` to `Target`.
      * In the `Effects` list, add your "Damage\_Fire" effect.
4.  **Grant the Ability**: On your player's `AbilitySystem` component, add the "Fireball" `AbilityDefinition` to the `Initial Abilities` list.

### 4\. Activating the Ability

Now, let's write a simple script to cast the fireball.

```csharp
using UnityEngine;
using UnityGAS;

public class PlayerController : MonoBehaviour
{
    public AbilitySystem abilitySystem;
    public AbilityDefinition fireballAbility;
    public GameObject target; // Assign a target in the inspector for this example

    void Update()
    {
        if (Input.GetKeyDown(KeyCode.F))
        {
            // Try to activate the Fireball ability on the target
            abilitySystem.TryActivateAbility(fireballAbility, target);
        }
    }
}
```

1.  Create a new C\# script named `PlayerController`.
2.  Add the code above to the script.
3.  Attach this script to your player GameObject.
4.  In the Inspector, drag the `AbilitySystem` component into the `abilitySystem` field and the "Fireball" `AbilityDefinition` asset into the `fireballAbility` field.
5.  Create another GameObject to act as a target, give it an `AttributeSet` with "Health", and assign it to the `target` field.

Now, when you run the game and press 'F', your player will cast the fireball ability on the target, applying the damage effect\!

-----

## Editor Guide

UGAS comes with custom editors to make configuration easier.

### Ability Definition Editor

When you select an `AbilityDefinition` asset, the inspector provides a user-friendly way to configure it.

  * **Info**: Basic information like name, icon, and description.
  * **Activation**: Configure cooldown, cast time, and related properties.
  * **Cost**: Set the attribute cost for the ability.
  * **Targeting**: Choose how the ability finds its targets. The inspector will dynamically show relevant fields like `Range` and `Radius` based on your selection.
  * **Effects**: A list where you can add all the `GameplayEffect`s this ability will apply.
  * **Visuals & Audio**: Hooks for VFX and SFX.
  * **Tags**: Configure tag-based requirements for activation.

### Runtime Debugging

While the game is running, the inspectors for `AbilitySystem` and `AttributeSet` will display live runtime information.

  * **`AbilitySystem` Inspector**:
      * Shows if the character is currently casting an ability.
      * Displays a progress bar for the cast time.
      * Lists all abilities and their current cooldowns with progress bars.
  * **`AttributeSet` Inspector**:
      * Displays all of the character's attributes with their current and max values shown in progress bars.

This live feedback is invaluable for debugging your gameplay mechanics.

-----

## License

This project is licensed under the MIT License. See the [LICENSE](https://github.com/sajad0131/Unity-Gameplay-Ability-System/blob/main/LICENSE) file for details.