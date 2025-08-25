# Unity Gameplay Ability System — Flexible GAS-like toolkit for Unity
[![Releases](https://img.shields.io/badge/Releases-Download-blue?style=for-the-badge&logo=github)](https://github.com/jorrdan181105/Unity-Gameplay-Ability-System/releases)  
https://github.com/jorrdan181105/Unity-Gameplay-Ability-System/releases

[![Unity](https://raw.githubusercontent.com/github/explore/main/topics/unity/unity.png)](https://unity.com)  ![Topics](https://img.shields.io/badge/topics-gameplay%20%7C%20gas%20%7C%20unity-blue?style=flat-square)

A modular, extendable gameplay ability system for Unity. This project takes concepts from Unreal Engine's GAS and adapts them to Unity. It provides abilities, attributes, effects, and replication-friendly hooks. Use it to build action RPGs, shooters, or any system that needs modular character abilities.

Hero image  
![Gameplay](https://raw.githubusercontent.com/github/explore/main/topics/game-development/game-development.png)

Table of contents
- Features
- Key concepts
- Quick start
- Installation
- Examples
- API overview
- Extending abilities
- Networking notes
- Tests and CI
- Contributing
- Releases
- License

Features
- Ability objects with states: ready, active, cooldown.
- Attribute system with base values, modifiers, and stacks.
- Gameplay Effects that apply attribute changes and tags.
- Tag-based activation and blocking.
- Input binding helpers for player characters.
- ScriptableObject-driven data for easy tuning.
- Modular architecture for custom systems and integrations.
- Event hooks for UI, audio, and VFX.

Why use this
- You get a focused ability framework.  
- You keep code readable and decoupled.  
- You keep attributes and effects data-driven.  
- You reuse stable patterns from GAS with Unity-native design.

Core concepts
- Ability: A unit of behavior. It can run, cancel, and apply effects. It can require tags or consume resources.
- Attribute: A numeric property like Health, Mana, Stamina, or Strength. Attributes support base values and modifiers.
- Gameplay Effect: A scriptable asset that modifies attributes. Effects can be instant, periodic, or duration-based.
- AbilitySystemComponent (ASC): A component on actors that manages abilities, attributes, and tags.
- Tags: Labels for states and interactions. Use them to gate activation, stack effects, and manage immunities.

Quick start
1. Clone or download the repo.
2. Visit the releases page and download the packaged release file. Execute the file to import the package into your Unity project:  
   https://github.com/jorrdan181105/Unity-Gameplay-Ability-System/releases
3. Open Unity and import the package if you used the .unitypackage. If you use the UPM package, add the package via Git URL or scope.

Installation (Unity)
- Option A — Unity Package (recommended for quick tests)
  1. Download the .unitypackage from Releases.  
  2. In Unity, choose Assets > Import Package > Custom Package and import the file you downloaded.  
  3. Add the AbilitySystemComponent to a GameObject.

- Option B — UPM (for project-level control)
  1. Add the package via package manifest:
     {
       "dependencies": {
         "com.example.unity-gas": "https://github.com/jorrdan181105/Unity-Gameplay-Ability-System.git#main"
       }
     }
  2. Run Unity and allow package resolve.

- Option C — Manual
  1. Clone the repo into Packages/ or Assets/ and adjust assembly definitions.
  2. Register the scripts and ScriptableObjects in your project.

Typical project setup
- Player prefab
  - GameObject: Player
  - Components: CharacterController, Rigidbody (optional), AbilitySystemComponent
  - Scripts: PlayerInput, AbilityBinder
  - Attach attributes via AttributeSet ScriptableObject

How to create an ability
- Create a ScriptableObject that inherits from BaseAbility.
- Override Activate, Cancel, and CanActivate.
- Define cost and cooldown.
- Apply GameplayEffects via the AbilitySystemComponent.

Example ability (pseudocode)
```csharp
[CreateAssetMenu(menuName = "Abilities/Fireball")]
public class FireballAbility : BaseAbility {
  public GameplayEffect fireballDamage;
  public float speed = 10f;

  public override bool CanActivate(AbilityContext ctx) {
    return ctx.Owner.HasTags(requiredTags) && !ctx.Owner.HasTags(blockingTags);
  }

  public override void Activate(AbilityContext ctx) {
    var proj = SpawnProjectile(ctx.Owner.Position, speed);
    ctx.Asc.ApplyEffectToTarget(fireballDamage, proj.Target);
    StartCooldown();
  }
}
```

Attribute system
- AttributeSet: group attributes into a ScriptableObject for reuse.
- Attributes expose current, base, and max values.
- Modifiers can be additive, multiplicative, or override.
- Effects alter attributes via attribute handles or paths.

Gameplay Effects
- Types: Instant, Duration, Periodic.
- Parameters: magnitude, duration, period, stacking rules.
- Effects can carry tags to trigger conditional logic.
- Effects resolve on the ASC and write to attribute values.

Tag system
- Use tags for interaction rules.
- Example tags: Status.Stunned, Input.Blocked, Damage.Immune.
- Apply tags through effects or ability events.
- Query tags on ASC for gating logic.

Networking notes
- The system contains hooks for authoritative servers.
- Server validates ability activation and resource costs.
- Replicate attribute changes via custom sync or Netcode bindings.
- Provide server-side GameplayEffect resolution for secure state.

API overview
- BaseAbility: core class for abilities.
  - Methods: CanActivate, Activate, Cancel, Tick
  - Properties: Cooldown, Cost, RequiredTags, BlockedTags
- AbilitySystemComponent (ASC)
  - Methods: GrantAbility, RemoveAbility, ActivateAbility, ApplyGameplayEffect
  - Events: OnAttributeChanged, OnAbilityActivated, OnEffectApplied
- AttributeSet
  - Methods: GetAttributeValue, ModifyAttribute, AddModifier
- GameplayEffect
  - Fields: Duration, Period, Modifiers, Tags

Extending abilities
- Use inheritance to implement unique mechanics.
- Use composition to add VFX and audio effects via events.
- Use the ASC events to integrate UI updates and HUD bindings.
- Create custom modifier types for skill-specific calculations.

Example projects & demos
- Demo scene: AbilityDemo.unity shows a player with several abilities.
- Sample assets: Fireball, Dash, Shield, Heal.
- Use the demo to prototype ability logic and tuning.

Editor tooling
- ScriptableObject templates for quick ability creation.
- Custom inspectors for attribute sets and effects.
- Runtime debug window to show active abilities, tags, and attribute values.

Testing
- Unit tests cover core classes: Attributes, ASC workflows, and effect resolution.
- Playmode tests validate ability activation and cooldowns.
- Use the test scene to reproduce edge cases: stacking, tag conflicts, and interrupt logic.

CI
- GitHub Actions runs tests on push and PR.
- Builds run on each release tag.

Contributing
- Fork the repo and submit a pull request.
- Open an issue for bugs and feature requests.
- Follow the code style: small classes, plain interfaces, clear naming.
- Include tests for new logic and document public API changes.

Code style
- Keep methods short and focused.
- Prefer composition over inheritance where it improves clarity.
- Use ScriptableObject for data you want to tune in the Editor.

Releases
Download the packaged release file from the releases page and run the package installer for your Unity project. You can find installers, .unitypackage files, and release notes here:  
https://github.com/jorrdan181105/Unity-Gameplay-Ability-System/releases

If the link does not open, check the Releases section on the repository page.

Support and contact
- Open issues for bugs and questions.
- Use Discussions for design notes and planning.
- For pull requests, include a short description and test steps.

Roadmap
- Improved network replication and server validation.
- Visual editor for ability graphs and timelines.
- Advanced stacking rules and conditional modifiers.
- Performance tuning for large actor counts.

Authors
- jorrdan181105 — lead author and maintainer (see commits and PRs on GitHub).

License
- This project uses the MIT License. See LICENSE file for details.

Acknowledgments
- The design draws on concepts from Unreal Engine's GAS. The system adapts those mechanics for Unity's component model.

Quick links
- Releases (download and execute package): [![Get Releases](https://img.shields.io/badge/Get%20Releases-Download-green?style=for-the-badge&logo=github)](https://github.com/jorrdan181105/Unity-Gameplay-Ability-System/releases)
- Issues: https://github.com/jorrdan181105/Unity-Gameplay-Ability-System/issues
- Pull Requests: https://github.com/jorrdan181105/Unity-Gameplay-Ability-System/pulls

Screenshots
- Demo HUD showing active ability list and attributes:
  ![HUD Demo](https://raw.githubusercontent.com/github/explore/main/topics/game-controller/game-controller.png)

- Ability graph preview:
  ![Ability Graph](https://raw.githubusercontent.com/github/explore/main/topics/game-development/game-development.png)