# Singleplayer Zombies Game Implementation Guide

Based on analysis of Unity's FPS Sample project, this guide outlines what you need to build a CoD Black Ops 2 style zombies game as a singleplayer experience.

---

## Table of Contents

1. [Project Setup & Unity Version](#1-project-setup--unity-version)
2. [What You Can Simplify (Singleplayer Advantage)](#2-what-you-can-simplify-singleplayer-advantage)
3. [Animation Sources & Options](#3-animation-sources--options)
4. [Step-by-Step Implementation Plan](#4-step-by-step-implementation-plan)
5. [Detailed System Breakdowns](#5-detailed-system-breakdowns)
6. [Work Effort Estimates](#6-work-effort-estimates)
7. [Asset Checklist](#7-asset-checklist)

---

## 1. Project Setup & Unity Version

### Recommended Unity Version
- **Unity 2022.3 LTS** or **Unity 6 (2023.3+)**
- Both have mature Animation Rigging, stable HDRP/URP, and good tooling

### Render Pipeline Choice

| Option | Pros | Cons | Recommendation |
|--------|------|------|----------------|
| **URP** | Better performance, more tutorials, mobile-friendly | Less graphical fidelity | ✅ Best for solo dev |
| **HDRP** | AAA visuals, realistic lighting | Heavy, complex, overkill | Only if you want maximum quality |
| **Built-in** | Simple, familiar | Legacy, limited features | Not recommended |

### Essential Packages to Install

```
com.unity.animation.rigging     # For IK (foot placement, aiming)
com.unity.ai.navigation         # For zombie pathfinding
com.unity.cinemachine           # For camera system
com.unity.inputsystem           # Modern input handling
com.unity.textmeshpro           # UI text
com.unity.probuilder            # Level prototyping (optional)
```

---

## 2. What You Can Simplify (Singleplayer Advantage)

The FPS Sample was built for **server-authoritative multiplayer**, which adds massive complexity. Going singleplayer removes:

### Removed Complexity

| FPS Sample Feature | Singleplayer Equivalent | Savings |
|-------------------|------------------------|---------|
| Network replication | Not needed | ~40% code reduction |
| Client-side prediction | Not needed | Huge complexity reduction |
| Rollback/reconciliation | Not needed | Huge complexity reduction |
| Server/client prefab variants | Single prefab per entity | Asset management simplified |
| Custom Playable Graph for sync | Standard Animator Controller | Much easier animation |
| Tick-based simulation | Standard Update() loop | Simpler logic |

### What You Still Need

| System | Why Still Needed |
|--------|------------------|
| Player controller | Core gameplay |
| Animation system | Visual feedback |
| Foot IK | Professional look on uneven terrain |
| Weapon system | Core gameplay |
| AI system | Zombie behavior |
| Round system | Game progression |
| Economy system | Points/purchases |
| UI system | HUD, menus |

---

## 3. Animation Sources & Options

### Option A: Mixamo (FREE - Best Starting Point)

**Website:** https://www.mixamo.com/

**What's Available:**
- ✅ Full humanoid character rigs
- ✅ Locomotion (walk, run, sprint in all directions)
- ✅ Jumping, falling, landing
- ✅ Zombie animations (walks, attacks, idles)
- ✅ Combat animations (shooting, reloading, melee)
- ✅ Death animations
- ✅ Hit reactions

**Process:**
1. Upload your character model (or use their characters)
2. Auto-rigging in seconds
3. Browse animation library
4. Download as FBX with "In Place" option for locomotion
5. Import to Unity, set to Humanoid rig

**Limitations:**
- First-person arms need custom work
- Some animations are generic (may need tweaking)
- No ADS (aim down sights) poses

**Cost:** FREE (Adobe account required)

---

### Option B: Unity Asset Store

**Recommended Packs:**

| Pack | Price | What You Get |
|------|-------|--------------|
| **Zombie Characters Pack** | $15-40 | Zombie models + animations |
| **FPS Animation Pack** | $20-60 | First-person arm animations |
| **Locomotion Animator** | $30-50 | Full movement system with blendtrees |
| **Animation Rigging Examples** | Free | IK setup references |

**Search terms:**
- "FPS arms animations"
- "Zombie animation pack"
- "Third person shooter animations"
- "Humanoid locomotion"

---

### Option C: Create Your Own

**Tools Needed:**
- **Blender** (free) - Modeling and rigging
- **Cascadeur** (free tier) - AI-assisted animation
- **Rokoko** or similar - Motion capture (if you have budget)

**Time Investment:**
- Learning curve: 2-6 months
- Per animation: 2-8 hours for a polished result

**Recommended only if:**
- You enjoy animation as a craft
- You need very specific animations
- Long-term project investment

---

### Option D: Hybrid Approach (RECOMMENDED)

1. **Base locomotion:** Mixamo (free)
2. **Zombie animations:** Mixamo + Asset Store pack
3. **First-person arms:** Asset Store FPS pack (~$40)
4. **Custom tweaks:** Learn basic Blender for adjustments

**Estimated Cost:** $40-100  
**Time Saved:** 100+ hours

---

## 4. Step-by-Step Implementation Plan

### Phase 1: Foundation (2-3 weeks)

#### Week 1: Project Setup
- [ ] Create new Unity project (URP recommended)
- [ ] Install required packages (see Section 1)
- [ ] Set up folder structure:
  ```
  Assets/
  ├── Animation/
  │   ├── Player/
  │   ├── Zombies/
  │   └── Weapons/
  ├── Audio/
  ├── Materials/
  ├── Models/
  │   ├── Characters/
  │   ├── Weapons/
  │   └── Environment/
  ├── Prefabs/
  ├── Scenes/
  ├── Scripts/
  │   ├── Player/
  │   ├── Enemies/
  │   ├── Weapons/
  │   ├── Systems/
  │   └── UI/
  └── UI/
  ```
- [ ] Create test scene with basic lighting
- [ ] Set up Input System with actions:
  - Move (WASD)
  - Look (Mouse)
  - Jump (Space)
  - Sprint (Shift)
  - Fire (LMB)
  - ADS (RMB)
  - Reload (R)
  - Interact (E/F)
  - Melee (V)

#### Week 2-3: Player Controller
- [ ] Implement CharacterController-based movement
- [ ] Add mouse look (first-person camera)
- [ ] Implement sprinting
- [ ] Add jumping with ground detection
- [ ] Set up basic collision layers

**Simplified Player Controller (vs FPS Sample):**

```csharp
// FPS Sample uses: CharacterPredictedData + UserCommand + network sync
// You need: Simple MonoBehaviour

public class PlayerController : MonoBehaviour
{
    [Header("Movement")]
    public float walkSpeed = 5f;
    public float sprintSpeed = 8f;
    public float jumpForce = 5f;
    public float gravity = -20f;
    
    [Header("Look")]
    public float mouseSensitivity = 2f;
    public float maxLookAngle = 85f;
    
    private CharacterController controller;
    private Vector3 velocity;
    private float xRotation;
    private bool isGrounded;
    
    // Standard Update() - no tick system needed
}
```

---

### Phase 2: Animation System (2-3 weeks)

#### Week 4: Player Animation Setup
- [ ] Download Mixamo locomotion pack:
  - Idle
  - Walk Forward/Back/Left/Right
  - Run Forward/Back/Left/Right
  - Sprint
  - Jump Start/Air/Land
- [ ] Create Animator Controller with layers:
  - Base Layer: Locomotion
  - Upper Body Layer: Actions (shoot, reload)
  - Additive Layer: Breathing/Sway
- [ ] Set up 2D Blend Tree for 8-directional movement
- [ ] Configure state transitions

#### Week 5: First-Person Arms
- [ ] Import or create first-person arm model
- [ ] Set up separate Animator for arms
- [ ] Create states:
  - Idle
  - Walk/Run (subtle bob)
  - Sprint (arm pump)
  - ADS (aim down sights)
  - Fire
  - Reload
  - Melee
- [ ] Implement procedural weapon sway

#### Week 6: IK Setup (Animation Rigging)
- [ ] Add Rig Builder component to player
- [ ] Create foot IK setup:
  - Two Bone IK for each leg
  - Raycast ground detection
  - Foot rotation to match surface
- [ ] Create hand IK for weapon holding
- [ ] Test on uneven terrain

**Animation Rigging Foot IK (simpler than FPS Sample):**

```csharp
public class FootIK : MonoBehaviour
{
    public Transform leftFootTarget;
    public Transform rightFootTarget;
    public LayerMask groundLayer;
    public float raycastDistance = 1.5f;
    public float footOffset = 0.1f;
    
    void LateUpdate()
    {
        UpdateFootIK(leftFootTarget);
        UpdateFootIK(rightFootTarget);
    }
    
    void UpdateFootIK(Transform foot)
    {
        if (Physics.Raycast(foot.position + Vector3.up, Vector3.down, 
            out RaycastHit hit, raycastDistance, groundLayer))
        {
            foot.position = hit.point + Vector3.up * footOffset;
            foot.rotation = Quaternion.FromToRotation(Vector3.up, hit.normal) 
                          * transform.rotation;
        }
    }
}
```

---

### Phase 3: Weapons System (2 weeks)

#### Week 7-8: Core Weapon Mechanics
- [ ] Create base Weapon class:
  ```csharp
  public abstract class Weapon : MonoBehaviour
  {
      public WeaponData data; // ScriptableObject
      public Transform firePoint;
      public abstract void Fire();
      public abstract void Reload();
  }
  ```
- [ ] Implement WeaponData ScriptableObject:
  - Damage, fire rate, magazine size
  - Reload time, ADS speed
  - Recoil pattern
  - Audio clips, VFX references
- [ ] Create hitscan weapon (pistol, rifle)
- [ ] Create projectile weapon (rocket launcher) - optional
- [ ] Implement recoil system
- [ ] Add muzzle flash VFX
- [ ] Implement reload mechanics
- [ ] Create weapon switching system

---

### Phase 4: Enemy AI (3-4 weeks)

#### Week 9-10: Zombie Navigation
- [ ] Bake NavMesh for test level
- [ ] Create base Zombie class
- [ ] Implement NavMeshAgent movement
- [ ] Create zombie states:
  - Spawn (climbing through barrier)
  - Idle (waiting for player detection)
  - Chase (following player)
  - Attack (melee range)
  - Death

#### Week 11-12: Zombie Variety & Polish
- [ ] Add zombie animations from Mixamo:
  - Zombie walk variations
  - Zombie run
  - Attack swings
  - Hit reactions
  - Death animations
- [ ] Implement damage system with ragdoll on death
- [ ] Create zombie variants:
  - Regular (slow, low health)
  - Runner (fast, low health)
  - Heavy (slow, high health)
- [ ] Add spawn point system
- [ ] Implement barrier/window system

**Simple Zombie AI:**

```csharp
public class ZombieAI : MonoBehaviour
{
    public enum State { Spawning, Chasing, Attacking, Dead }
    public State currentState;
    
    private NavMeshAgent agent;
    private Transform player;
    private Animator animator;
    
    public float attackRange = 2f;
    public float attackCooldown = 1f;
    public int damage = 20;
    
    void Update()
    {
        switch (currentState)
        {
            case State.Chasing:
                agent.SetDestination(player.position);
                if (Vector3.Distance(transform.position, player.position) < attackRange)
                    currentState = State.Attacking;
                break;
                
            case State.Attacking:
                // Attack logic
                break;
        }
    }
}
```

---

### Phase 5: Game Systems (2-3 weeks)

#### Week 13: Round System
- [ ] Create RoundManager:
  - Track current round number
  - Calculate zombies per round
  - Manage zombie spawning rate
  - Detect round completion
- [ ] Implement zombie count formula:
  ```csharp
  int ZombiesForRound(int round) => Mathf.RoundToInt(6 + round * 0.5f * round);
  ```
- [ ] Add between-round delay
- [ ] Create round start/end events

#### Week 14: Economy System
- [ ] Create PointsManager:
  - Track player points
  - Award points for actions:
    - Kill: 50-100 points
    - Headshot: +50 bonus
    - Melee kill: +80 bonus
    - Board repair: 10 points
- [ ] Implement purchasable items:
  - Wall weapons
  - Mystery box
  - Doors/barriers
  - Perks

#### Week 15: Perks System
- [ ] Create Perk ScriptableObject:
  ```csharp
  [CreateAssetMenu]
  public class PerkData : ScriptableObject
  {
      public string perkName;
      public int cost;
      public Sprite icon;
      public GameObject vfxPrefab;
      public abstract void ApplyEffect(Player player);
  }
  ```
- [ ] Implement core perks:
  - Quick Revive (solo: self-revive)
  - Juggernog (more health)
  - Speed Cola (faster reload)
  - Double Tap (faster fire rate)
- [ ] Create perk machine interactables

---

### Phase 6: UI & Polish (2-3 weeks)

#### Week 16-17: User Interface
- [ ] Create HUD:
  - Health bar
  - Points display
  - Current weapon + ammo
  - Round counter
  - Active perks
- [ ] Create menus:
  - Main menu
  - Pause menu
  - Settings (audio, controls)
  - Game over screen
- [ ] Add damage indicators (screen flash, directional)
- [ ] Implement hitmarkers

#### Week 18: Audio & Polish
- [ ] Implement audio system:
  - Weapon sounds
  - Zombie sounds
  - Ambient sounds
  - Music system (rounds, danger)
- [ ] Add screen shake for impacts
- [ ] Polish animations (transitions, blending)
- [ ] Add particle effects (blood, debris)
- [ ] Implement post-processing (bloom, vignette)

---

### Phase 7: Content & Testing (2-4 weeks)

#### Week 19-20: Level Design
- [ ] Design first playable map
- [ ] Block out with ProBuilder
- [ ] Add spawn points (zombies, player)
- [ ] Place weapons, perks, mystery box
- [ ] Create navigation flow with doors
- [ ] Bake lighting and NavMesh

#### Week 21-22: Testing & Balancing
- [ ] Playtest all rounds (at least to round 20)
- [ ] Balance weapon damage/costs
- [ ] Balance zombie health scaling
- [ ] Adjust perk effects
- [ ] Fix bugs
- [ ] Optimize performance

---

## 5. Detailed System Breakdowns

### Player State Machine

```
PlayerStates:
├── Locomotion
│   ├── Idle
│   ├── Walking
│   ├── Running
│   └── Sprinting
├── Airborne
│   ├── Jumping
│   ├── Falling
│   └── Landing
├── Combat
│   ├── Firing
│   ├── Reloading
│   └── Melee
└── Interaction
    ├── Purchasing
    └── Reviving (if co-op later)
```

### Animator Controller Structure

```
Base Layer (Full Body):
├── Blend Tree: Locomotion (2D Freeform)
│   ├── Idle (0,0)
│   ├── Walk_F (0,1)
│   ├── Walk_B (0,-1)
│   ├── Walk_L (-1,0)
│   ├── Walk_R (1,0)
│   └── Diagonals...
├── Jump_Start → Jump_Air → Jump_Land
└── Sprint

Upper Body Layer (Avatar Mask - Upper Only):
├── Empty (pass through)
├── Aim_Idle
├── Fire
├── Reload
└── Melee

Additive Layer:
├── Breathing_Idle
└── Hit_Reaction
```

### Damage System

```csharp
public interface IDamageable
{
    void TakeDamage(DamageInfo damage);
}

public struct DamageInfo
{
    public int amount;
    public DamageType type;
    public Vector3 hitPoint;
    public Vector3 hitDirection;
    public GameObject source;
}

public enum DamageType
{
    Bullet,
    Explosive,
    Melee,
    Environmental
}
```

---

## 6. Work Effort Estimates

### Solo Developer Timeline

| Phase | Duration | Hours (Est.) |
|-------|----------|--------------|
| Phase 1: Foundation | 2-3 weeks | 40-60 |
| Phase 2: Animation | 2-3 weeks | 40-60 |
| Phase 3: Weapons | 2 weeks | 30-40 |
| Phase 4: Enemy AI | 3-4 weeks | 60-80 |
| Phase 5: Game Systems | 2-3 weeks | 40-60 |
| Phase 6: UI & Polish | 2-3 weeks | 40-60 |
| Phase 7: Content | 2-4 weeks | 40-80 |
| **Total** | **15-23 weeks** | **290-440 hours** |

### With Asset Store Help

If you purchase animation packs and ready-made systems:
- **Animation packs:** Save 20-40 hours
- **UI kit:** Save 10-20 hours
- **FPS controller:** Save 15-25 hours

**Revised estimate with assets:** 200-350 hours (3-5 months part-time)

### By Skill Level

| Skill Level | Time Estimate | Notes |
|-------------|---------------|-------|
| Beginner | 6-12 months | Learning while building |
| Intermediate | 4-6 months | Know Unity basics |
| Advanced | 2-4 months | Experienced with similar projects |

---

## 7. Asset Checklist

### Must-Have Assets

**Player:**
- [ ] First-person arms model
- [ ] Third-person character model (for shadows/future co-op)
- [ ] Locomotion animations (8 directions)
- [ ] Jump/fall/land animations
- [ ] Weapon hold poses per weapon type

**Weapons (minimum 3-5):**
- [ ] Pistol
- [ ] SMG
- [ ] Assault Rifle
- [ ] Shotgun
- [ ] Special weapon (ray gun style)
- [ ] Knife/melee weapon
- [ ] Fire, reload, equip animations per weapon

**Zombies:**
- [ ] At least 2 zombie models (variety)
- [ ] Walk animations (2-3 variations)
- [ ] Run animation
- [ ] Attack animations (2-3 variations)
- [ ] Death animations (3+ variations)
- [ ] Spawn/climb through window animation

**Environment:**
- [ ] Wall textures
- [ ] Floor textures
- [ ] Props (barrels, boxes, furniture)
- [ ] Barrier/window models
- [ ] Door models
- [ ] Perk machine models (4)
- [ ] Mystery box model
- [ ] Wall weapon mounts

**Audio:**
- [ ] Weapon sounds (fire, reload, empty)
- [ ] Zombie sounds (groans, attacks, death)
- [ ] Player sounds (footsteps, damage, death)
- [ ] UI sounds (purchase, denied, round change)
- [ ] Ambient sounds
- [ ] Music tracks

**UI:**
- [ ] HUD elements
- [ ] Font
- [ ] Icons (perks, weapons, ammo types)
- [ ] Crosshair/reticle

---

## Quick Start Recommendation

If you want to start TODAY with minimal investment:

1. **Get Mixamo account** (free)
2. **Download Unity 2022.3 LTS**
3. **Install packages** from Section 1
4. **Download from Mixamo:**
   - Any humanoid character
   - Locomotion pack (idle, walk, run, jump)
   - Zombie character
   - Zombie animation pack
5. **Start with Phase 1, Week 1** tasks

**First Milestone (2 weeks):** Character running around a gray box level with working animation

**Second Milestone (4 weeks):** Shoot zombies that chase you

**Third Milestone (8 weeks):** Complete round 1-5 loop with points

---

## Resources

### Tutorials
- **Brackeys** (YouTube) - Unity basics, FPS controller
- **Code Monkey** (YouTube) - Systems design, AI
- **Gabriel Aguiar** (YouTube) - VFX, shaders
- **iHeartGameDev** (YouTube) - Animation, IK

### Documentation
- [Unity Animation Rigging](https://docs.unity3d.com/Packages/com.unity.animation.rigging@latest)
- [Unity NavMesh](https://docs.unity3d.com/Manual/nav-BuildingNavMesh.html)
- [Unity Input System](https://docs.unity3d.com/Packages/com.unity.inputsystem@latest)

### Asset Store Search Terms
- "FPS controller"
- "Zombie pack"
- "FPS arms"
- "Gun sounds"
- "Horror ambient"

---

*This guide is based on analysis of Unity's FPS Sample project, simplified for singleplayer development.*
