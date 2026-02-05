# 🎮 COMPLETE Zombies Game Development Guide
## Ultra-Detailed Step-by-Step with Full Explanations

> **Version:** Unity 2022.3 LTS or Unity 6
> **Estimated Time:** 4-6 months (solo developer, part-time)
> **Difficulty:** Intermediate

---

# 📑 MASTER TABLE OF CONTENTS

## PART 1: PROJECT FOUNDATION
- [1.1 Creating the Unity Project](#11-creating-the-unity-project)
- [1.2 Installing Required Packages](#12-installing-required-packages)
- [1.3 Project Folder Structure](#13-project-folder-structure)
- [1.4 Setting Up Layers and Tags](#14-setting-up-layers-and-tags)
- [1.5 Input System Configuration](#15-input-system-configuration)

## PART 2: PLAYER CONTROLLER
- [2.1 Player GameObject Hierarchy](#21-player-gameobject-hierarchy)
- [2.2 CharacterController Setup](#22-charactercontroller-setup)
- [2.3 PlayerController Script (Complete)](#23-playercontroller-script-complete)
- [2.4 Testing Movement](#24-testing-movement)

## PART 3: PLAYER ANIMATION SYSTEM
- [3.1 Getting Animations from Mixamo](#31-getting-animations-from-mixamo)
- [3.2 Importing Animations to Unity](#32-importing-animations-to-unity)
- [3.3 Creating the Avatar](#33-creating-the-avatar)
- [3.4 Animator Controller Setup](#34-animator-controller-setup)
- [3.5 Blend Trees for Locomotion](#35-blend-trees-for-locomotion)
- [3.6 Jump State Machine](#36-jump-state-machine)
- [3.7 PlayerAnimator Script](#37-playeranimator-script)
- [3.8 Connecting Animation to Player](#38-connecting-animation-to-player)

## PART 4: FOOT IK SYSTEM (DETAILED)
- [4.1 Understanding Foot IK](#41-understanding-foot-ik)
- [4.2 Animation Rigging Package Setup](#42-animation-rigging-package-setup)
- [4.3 Creating the Rig Hierarchy](#43-creating-the-rig-hierarchy)
- [4.4 Two Bone IK Constraints Setup](#44-two-bone-ik-constraints-setup)
- [4.5 FootIK Script](#45-footik-script)
- [4.6 Testing and Troubleshooting](#46-testing-and-troubleshooting)

## PART 5: FIRST-PERSON ARMS & CAMERA
- [5.1 First-Person Setup Overview](#51-first-person-setup-overview)
- [5.2 Camera System](#52-camera-system)
- [5.3 First-Person Arms Model](#53-first-person-arms-model)
- [5.4 Weapon Sway and Bob](#54-weapon-sway-and-bob)

## PART 6: WEAPONS SYSTEM
- [6.1 Weapon Architecture](#61-weapon-architecture)
- [6.2 WeaponData ScriptableObject](#62-weapondata-scriptableobject)
- [6.3 Base Weapon Script](#63-base-weapon-script)
- [6.4 Hitscan Weapons](#64-hitscan-weapons)
- [6.5 Weapon Switching](#65-weapon-switching)
- [6.6 Reloading System](#66-reloading-system)

## PART 7: ZOMBIE AI SYSTEM
- [7.1 NavMesh Setup](#71-navmesh-setup)
- [7.2 Zombie State Machine](#72-zombie-state-machine)
- [7.3 ZombieAI Script](#73-zombieai-script)
- [7.4 Zombie Animations](#74-zombie-animations)
- [7.5 Zombie Spawning](#75-zombie-spawning)

## PART 8: GAME SYSTEMS
- [8.1 Health System](#81-health-system)
- [8.2 Points/Economy System](#82-pointseconomy-system)
- [8.3 Round Manager](#83-round-manager)
- [8.4 Perk System](#84-perk-system)
- [8.5 Mystery Box](#85-mystery-box)
- [8.6 Purchasable Doors](#86-purchasable-doors)

## PART 9: USER INTERFACE
- [9.1 HUD Setup](#91-hud-setup)
- [9.2 Main Menu](#92-main-menu)
- [9.3 Pause Menu](#93-pause-menu)
- [9.4 Game Over Screen](#94-game-over-screen)

## PART 10: AUDIO
- [10.1 Audio Manager](#101-audio-manager)
- [10.2 Weapon Sounds](#102-weapon-sounds)
- [10.3 Zombie Sounds](#103-zombie-sounds)
- [10.4 Music System](#104-music-system)

## PART 11: POLISH & EFFECTS
- [11.1 Post-Processing](#111-post-processing)
- [11.2 Particle Effects](#112-particle-effects)
- [11.3 Screen Effects](#113-screen-effects)

## PART 12: BUILDING & TESTING
- [12.1 Build Settings](#121-build-settings)
- [12.2 Optimization](#122-optimization)
- [12.3 Final Testing Checklist](#123-final-testing-checklist)

---

# PART 1: PROJECT FOUNDATION

## 1.1 Creating the Unity Project

### Step-by-Step Instructions:

**Step 1: Open Unity Hub**
1. Launch Unity Hub
2. Make sure you're signed into your Unity account

**Step 2: Check Unity Version**
1. Click "Installs" in left sidebar
2. Verify you have **Unity 2022.3 LTS** installed
3. If not, click "Install Editor" → Select 2022.3.x LTS
4. Check these modules during install:
   - ✅ Windows Build Support (IL2CPP)
   - ✅ Documentation
   - ✅ WebGL Build Support (optional)

**Step 3: Create New Project**
1. Click "Projects" in left sidebar
2. Click "New Project" button (top right)
3. Select Unity version: **2022.3.x LTS**
4. Select template: **3D (URP)** - Universal Render Pipeline
   
   > **Why URP?** It offers good performance with modern graphics features.
   > HDRP is overkill for most projects and harder to optimize.

5. Project name: `ZombiesSurvival` (or your preference)
6. Location: Choose your projects folder
7. Check "Connect to Unity Cloud" if desired
8. Click **Create Project**

**Step 4: Wait for Initial Import**
- First import takes 5-15 minutes
- Unity will configure packages and compile scripts
- DON'T close Unity during this process!

### Verify Project Created Successfully:
- [ ] Project opens without errors
- [ ] Console window is mostly clear (some warnings are okay)
- [ ] You can see the SampleScene in the Hierarchy

---

## 1.2 Installing Required Packages

### What Are Packages?
Packages are pre-made features/tools created by Unity. We need several.

### Step-by-Step Package Installation:

**Step 1: Open Package Manager**
1. Top menu: Window → Package Manager
2. Wait for it to load (shows "Loading..." at first)

**Step 2: Show Unity Registry**
1. In Package Manager, click dropdown that says "Packages: In Project"
2. Change to "Packages: Unity Registry"
3. This shows ALL available Unity packages

**Step 3: Install Each Package**

For EACH package below:
1. Search for it in the search bar
2. Click on it in the list
3. Click "Install" button (bottom right)
4. Wait for installation to complete

### Required Packages:

| Package Name | Search Term | Purpose |
|--------------|-------------|---------|
| Animation Rigging | `animation rigging` | Foot IK, hand IK |
| Cinemachine | `cinemachine` | Camera system |
| Input System | `input system` | Modern input handling |
| AI Navigation | `ai navigation` | Zombie pathfinding |
| TextMeshPro | `textmesh` | Better UI text |
| ProBuilder | `probuilder` | Level prototyping (optional) |

### Special Steps After Installing Input System:

When you install Input System, Unity will show a popup:

```
"This project is using the new Input System package but the native
input manager is not enabled. The new input system requires restarting
the Editor."
```

1. Click **"Yes"** to enable new input system
2. Unity will restart
3. This is normal!

### Verify Packages Installed:

After Unity restarts:
1. Open Package Manager again
2. Change dropdown to "Packages: In Project"
3. Verify all packages show in the list:
   - [ ] Animation Rigging
   - [ ] Cinemachine
   - [ ] Input System
   - [ ] AI Navigation
   - [ ] TextMeshPro
   - [ ] ProBuilder (if installed)

---

## 1.3 Project Folder Structure

### Why Folder Structure Matters:
- Keeps your project organized
- Makes finding assets easy
- Prevents Unity from slowing down
- Makes collaboration possible

### Create This Folder Structure:

**Step 1: Open Project Window**
- If not visible: Window → General → Project

**Step 2: Create Main Folder**
1. Right-click in Project window (in Assets folder)
2. Create → Folder
3. Name it: `_Project`

> **Why underscore?** The `_` makes it sort to the top of the list!

**Step 3: Create Subfolders**

Right-click `_Project` → Create → Folder for each:

```
Assets/
├── _Project/
│   ├── Animations/
│   │   ├── Player/
│   │   │   ├── Locomotion/
│   │   │   ├── Combat/
│   │   │   └── Controllers/
│   │   ├── Zombies/
│   │   │   ├── Movement/
│   │   │   ├── Combat/
│   │   │   └── Controllers/
│   │   ├── Weapons/
│   │   └── FirstPerson/
│   ├── Audio/
│   │   ├── Music/
│   │   ├── SFX/
│   │   │   ├── Weapons/
│   │   │   ├── Player/
│   │   │   └── Zombies/
│   │   └── Ambient/
│   ├── Materials/
│   ├── Models/
│   │   ├── Characters/
│   │   │   ├── Player/
│   │   │   └── Zombies/
│   │   ├── Weapons/
│   │   └── Environment/
│   ├── Prefabs/
│   │   ├── Player/
│   │   ├── Enemies/
│   │   ├── Weapons/
│   │   ├── Interactables/
│   │   └── Effects/
│   ├── Scenes/
│   ├── ScriptableObjects/
│   │   ├── Weapons/
│   │   ├── Perks/
│   │   ├── Enemies/
│   │   └── Audio/
│   ├── Scripts/
│   │   ├── Player/
│   │   ├── Enemies/
│   │   ├── Weapons/
│   │   ├── Systems/
│   │   ├── UI/
│   │   └── Utilities/
│   ├── Shaders/
│   ├── Textures/
│   └── UI/
│       ├── Sprites/
│       ├── Fonts/
│       └── Prefabs/
└── ThirdParty/
    └── (Asset Store imports go here)
```

### Quick Folder Creation Tip:
You can create nested folders quickly:
1. Right-click → Create → Folder
2. Name it: `Animations`
3. Double-click to enter it
4. Right-click → Create → Folder → `Player`
5. Continue...

---

## 1.4 Setting Up Layers and Tags

### What Are Layers?
Layers control:
- What objects the camera sees
- What objects can collide with each other
- What raycasts hit

### What Are Tags?
Tags are labels to identify objects (like "Player", "Enemy").

### Step-by-Step Setup:

**Step 1: Open Tags and Layers**
1. Edit → Project Settings
2. Click "Tags and Layers" in left sidebar

**Step 2: Create Tags**
Under "Tags", click the + button to add each:

| Tag Name | Used For |
|----------|----------|
| Player | The player character |
| Enemy | Zombies |
| Weapon | Weapon pickups |
| Interactable | Doors, mystery box, perks |
| Ground | Floor/terrain |
| Obstacle | Walls, barriers |

**Step 3: Create Layers**
Under "Layers", find empty slots (User Layer 6 and up):

| Layer # | Name | Purpose |
|---------|------|---------|
| 6 | Ground | Floor, terrain, walkable surfaces |
| 7 | Player | Player collision |
| 8 | Enemy | Zombie collision |
| 9 | Weapon | Weapon objects |
| 10 | Interactable | Doors, machines |
| 11 | Obstacle | Walls, barriers |
| 12 | IgnoreRaycast | Objects rays should ignore |
| 13 | FirstPerson | First-person arms (camera culling) |

**Step 4: Configure Collision Matrix**
1. Edit → Project Settings → Physics
2. Scroll down to "Layer Collision Matrix"
3. Uncheck boxes where objects SHOULDN'T collide:

Example settings:
- Player ↔ Player: OFF (no self-collision)
- Enemy ↔ Enemy: OFF (zombies clip through each other, CoD style)
- FirstPerson ↔ Everything except Default: OFF

---

## 1.5 Input System Configuration

### Understanding the New Input System:
The new Input System is:
- More flexible than the old `Input.GetKey()`
- Supports multiple input devices (keyboard, controller)
- Uses "Actions" that you define

### Step-by-Step Setup:

**Step 1: Create Input Actions Asset**
1. Navigate to `_Project/` folder
2. Right-click → Create → Input Actions
3. Name it: `PlayerInputActions`
4. Double-click to open the Input Actions editor

**Step 2: Create Action Map**
1. In the left column, click "+" next to "Action Maps"
2. Name it: `Player`

**Step 3: Create Actions**

For each action in the table below:
1. Click "+" next to "Actions" (middle column)
2. Name it as shown
3. Set the "Action Type" in the right panel
4. Add bindings by clicking "+" next to the action

### Complete Action List:

| Action Name | Action Type | Bindings |
|-------------|-------------|----------|
| Move | Value (Vector2) | W/A/S/D (2D Vector Composite), Left Stick |
| Look | Value (Vector2) | Mouse Delta, Right Stick |
| Jump | Button | Space, Gamepad South (A/X) |
| Sprint | Button | Left Shift, Left Stick Press |
| Fire | Button | Left Mouse Button, Right Trigger |
| ADS | Button | Right Mouse Button, Left Trigger |
| Reload | Button | R, Gamepad West (X/Square) |
| Interact | Button | E, Gamepad North (Y/Triangle) |
| Melee | Button | V, Right Stick Press |
| SwitchWeapon | Button | Q, Gamepad Right Shoulder |
| Pause | Button | Escape, Start Button |

### Detailed Setup for "Move" Action (2D Composite):

1. Click "Move" action
2. In right panel, set Action Type: **Value**
3. Set Control Type: **Vector 2**
4. Click "+" next to Move → Add 2D Vector Composite
5. Name it "WASD"
6. Expand it, you'll see: Up, Down, Left, Right
7. Click each and set:
   - Up: W [Keyboard]
   - Down: S [Keyboard]
   - Left: A [Keyboard]
   - Right: D [Keyboard]
8. Click "+" again → Add Binding
9. Set to: Left Stick [Gamepad]

### Detailed Setup for "Look" Action:

1. Click "Look" action
2. Action Type: **Value**
3. Control Type: **Vector 2**
4. Add Binding → Mouse → Delta
5. Add Binding → Gamepad → Right Stick

### Detailed Setup for Button Actions:

For each button action (Jump, Sprint, Fire, etc.):
1. Click the action
2. Action Type: **Button**
3. Add Binding → Click "Listen" → Press the key you want
4. For gamepad, add another binding

**Step 4: Save and Generate C# Class**
1. Click "Save Asset" button (top of Input Actions window)
2. In Project window, select `PlayerInputActions` asset
3. In Inspector, check ✅ "Generate C# Class"
4. Click "Apply"
5. Unity generates `PlayerInputActions.cs` automatically

**Step 5: Verify**
- [ ] `PlayerInputActions.cs` file exists
- [ ] No errors in Console
- [ ] All actions show in the Input Actions editor

---

# PART 2: PLAYER CONTROLLER

## 2.1 Player GameObject Hierarchy

### Understanding the Hierarchy:

Your player structure should look like this:

```
Player                          ← Main object (has scripts)
├── Alien                       ← Your character model
│   ├── Ch44                    ← Skinned Mesh (the visible mesh)
│   └── mixamorig:Hips          ← Skeleton root
│       ├── mixamorig:Spine
│       │   └── ... (more bones)
│       ├── mixamorig:LeftUpLeg
│       │   ├── mixamorig:LeftLeg
│       │   │   └── mixamorig:LeftFoot
│       │   │       └── mixamorig:LeftToeBase
│       └── mixamorig:RightUpLeg
│           ├── mixamorig:RightLeg
│           │   └── mixamorig:RightFoot
│               └── mixamorig:RightToeBase
├── CameraHolder                ← Empty object at eye level
│   └── Main Camera             ← The camera
├── GroundCheck                 ← Empty object at feet
└── FootIKRig                   ← IK rig (we'll add this later)
    ├── LeftFootIK
    │   ├── LeftFootTarget
    │   └── LeftFootHint
    └── RightFootIK
        ├── RightFootTarget
        └── RightFootHint
```

### Step-by-Step Creation:

**Step 1: Create Player Parent**
1. In Hierarchy, right-click → Create Empty
2. Name it: `Player`
3. Position: (0, 0, 0)
4. Add Tag: "Player" (Inspector → Tag dropdown)

**Step 2: Add Character Model**
1. Import your Mixamo character FBX to `_Project/Models/Characters/Player/`
2. Drag the FBX into the scene as a child of Player
3. Name it something recognizable (like "Alien" or "Character")
4. Position: (0, 0, 0) relative to Player
5. Rotation: (0, 0, 0)

> **If you don't have a character yet:**
> Go to Mixamo.com, download any character as FBX with skin.

**Step 3: Create CameraHolder**
1. Right-click Player → Create Empty
2. Name: `CameraHolder`
3. Local Position: (0, 1.6, 0)  ← Eye level height

> **Why 1.6?** Average human eye height is about 1.6m from ground.
> Adjust based on your character's height!

**Step 4: Move Main Camera**
1. Find "Main Camera" in Hierarchy
2. Drag it onto CameraHolder (makes it a child)
3. Set Main Camera's local position: (0, 0, 0)
4. Set local rotation: (0, 0, 0)

**Step 5: Create GroundCheck**
1. Right-click Player → Create Empty
2. Name: `GroundCheck`
3. Local Position: (0, 0, 0)  ← At feet level (Player's origin should be at feet)

### Verify Positions:

With Player selected, in Scene view:
- The player origin (orange dot) should be at the CHARACTER'S FEET
- CameraHolder should be at eye level
- GroundCheck should be at/just below feet

If your character's feet aren't at origin:
1. Select the character model child
2. Adjust its Y position until feet touch the Player's origin point
3. Usually this means Position Y = 0 for the model

---

## 2.2 CharacterController Setup

### What is CharacterController?
Unity's CharacterController is a component for moving characters that:
- Handles collision detection
- Handles stepping up stairs
- Handles slopes
- Does NOT use Rigidbody physics

### Step-by-Step Setup:

**Step 1: Add CharacterController**
1. Select the `Player` object
2. Inspector → Add Component
3. Search: "Character Controller"
4. Click to add it

**Step 2: Configure Settings**

In the CharacterController component:

| Setting | Value | Explanation |
|---------|-------|-------------|
| Slope Limit | 45 | Max angle (degrees) player can walk up |
| Step Offset | 0.3 | Max step height player can climb |
| Skin Width | 0.08 | Collision skin thickness (prevents clipping) |
| Min Move Distance | 0.001 | Ignore movements smaller than this |
| Center | (0, 1, 0) | Center of capsule (should be at chest height) |
| Radius | 0.35 | Capsule radius (half of character width) |
| Height | 2 | Capsule height (should match character) |

**Step 3: Visualize the Capsule**
- The green wireframe capsule in Scene view shows the collider
- It should fully contain your character
- But not be too much bigger!

**Step 4: Adjust If Needed**
If your character is different size:
- Measure your character's height in Scene view
- Set Height to match
- Set Center Y to half the Height
- Set Radius to about half the character's width

### Common Issues:

| Problem | Solution |
|---------|----------|
| Player falls through floor | Increase Skin Width slightly |
| Player can't walk up stairs | Increase Step Offset (max 0.4) |
| Player gets stuck on small obstacles | Decrease Skin Width |
| Collider too big/small | Adjust Center, Radius, Height |

---

## 2.3 PlayerController Script (Complete)

### Create the Script:

**Step 1: Create Script File**
1. Navigate to `_Project/Scripts/Player/`
2. Right-click → Create → C# Script
3. Name: `PlayerController`
4. Wait for Unity to compile
5. Double-click to open in your code editor

**Step 2: Replace Everything with This Code**

```csharp
using UnityEngine;
using UnityEngine.InputSystem;

/// <summary>
/// Main player controller handling movement, jumping, and looking.
/// Requires: CharacterController component on the same GameObject.
/// </summary>
[RequireComponent(typeof(CharacterController))]
public class PlayerController : MonoBehaviour
{
    #region Serialized Fields
    
    [Header("Movement Settings")]
    [Tooltip("Walking speed in units per second")]
    [SerializeField] private float walkSpeed = 5f;
    
    [Tooltip("Sprinting speed in units per second")]
    [SerializeField] private float sprintSpeed = 8f;
    
    [Tooltip("Speed while crouching")]
    [SerializeField] private float crouchSpeed = 2.5f;
    
    [Tooltip("Speed while aiming down sights")]
    [SerializeField] private float adsSpeed = 3f;
    
    [Header("Jump Settings")]
    [Tooltip("Initial jump velocity")]
    [SerializeField] private float jumpForce = 7f;
    
    [Tooltip("Gravity acceleration")]
    [SerializeField] private float gravity = -20f;
    
    [Tooltip("Extra gravity when falling (makes jumps feel snappier)")]
    [SerializeField] private float fallMultiplier = 1.5f;
    
    [Header("Look Settings")]
    [Tooltip("Mouse sensitivity")]
    [SerializeField] private float mouseSensitivity = 2f;
    
    [Tooltip("Maximum vertical look angle")]
    [SerializeField] private float maxLookAngle = 85f;
    
    [Tooltip("Reference to the camera holder")]
    [SerializeField] private Transform cameraHolder;
    
    [Header("Ground Check")]
    [Tooltip("Transform at the player's feet for ground detection")]
    [SerializeField] private Transform groundCheck;
    
    [Tooltip("Radius of ground check sphere")]
    [SerializeField] private float groundCheckRadius = 0.3f;
    
    [Tooltip("Layers considered as ground")]
    [SerializeField] private LayerMask groundMask;
    
    #endregion
    
    #region Private Fields
    
    // Components
    private CharacterController controller;
    private PlayerInputActions inputActions;
    
    // Movement state
    private Vector3 velocity;
    private Vector3 moveDirection;
    
    // Look state
    private float xRotation;
    
    // Input values
    private Vector2 moveInput;
    private Vector2 lookInput;
    
    // State flags
    private bool isGrounded;
    private bool isSprinting;
    private bool isAiming;
    private bool isCrouching;
    
    #endregion
    
    #region Public Properties
    
    /// <summary>Returns current move input as Vector2 (x = strafe, y = forward/back)</summary>
    public Vector2 MoveInput => moveInput;
    
    /// <summary>Returns true if player is on the ground</summary>
    public bool IsGrounded => isGrounded;
    
    /// <summary>Returns true if player is sprinting</summary>
    public bool IsSprinting => isSprinting && !isAiming && moveInput.y > 0;
    
    /// <summary>Returns true if player is aiming down sights</summary>
    public bool IsAiming => isAiming;
    
    /// <summary>Returns current vertical velocity</summary>
    public float VerticalVelocity => velocity.y;
    
    /// <summary>Returns current horizontal speed</summary>
    public float CurrentSpeed => new Vector3(controller.velocity.x, 0, controller.velocity.z).magnitude;
    
    #endregion
    
    #region Unity Callbacks
    
    private void Awake()
    {
        // Get components
        controller = GetComponent<CharacterController>();
        inputActions = new PlayerInputActions();
        
        // Lock and hide cursor
        Cursor.lockState = CursorLockMode.Locked;
        Cursor.visible = false;
        
        // Validate required references
        if (cameraHolder == null)
        {
            Debug.LogError("PlayerController: Camera Holder not assigned!");
        }
        if (groundCheck == null)
        {
            Debug.LogError("PlayerController: Ground Check not assigned!");
        }
    }
    
    private void OnEnable()
    {
        // Enable input
        inputActions.Player.Enable();
        
        // Subscribe to button events
        inputActions.Player.Jump.performed += OnJump;
        inputActions.Player.Sprint.performed += ctx => isSprinting = true;
        inputActions.Player.Sprint.canceled += ctx => isSprinting = false;
        inputActions.Player.ADS.performed += ctx => isAiming = true;
        inputActions.Player.ADS.canceled += ctx => isAiming = false;
    }
    
    private void OnDisable()
    {
        // Unsubscribe from events
        inputActions.Player.Jump.performed -= OnJump;
        
        // Disable input
        inputActions.Player.Disable();
    }
    
    private void Update()
    {
        // Read input values
        moveInput = inputActions.Player.Move.ReadValue<Vector2>();
        lookInput = inputActions.Player.Look.ReadValue<Vector2>();
        
        // Perform checks
        CheckGround();
        
        // Handle systems
        HandleMovement();
        HandleLook();
        ApplyGravity();
    }
    
    #endregion
    
    #region Movement
    
    private void CheckGround()
    {
        // Sphere check at feet position
        isGrounded = Physics.CheckSphere(
            groundCheck.position, 
            groundCheckRadius, 
            groundMask,
            QueryTriggerInteraction.Ignore
        );
        
        // Reset downward velocity when grounded
        if (isGrounded && velocity.y < 0)
        {
            velocity.y = -2f; // Small negative value keeps us grounded
        }
    }
    
    private void HandleMovement()
    {
        // Calculate move direction relative to where player is facing
        moveDirection = transform.right * moveInput.x + transform.forward * moveInput.y;
        
        // Determine current speed based on state
        float currentSpeed = GetCurrentMoveSpeed();
        
        // Apply movement
        controller.Move(moveDirection * currentSpeed * Time.deltaTime);
    }
    
    private float GetCurrentMoveSpeed()
    {
        if (isAiming) return adsSpeed;
        if (isCrouching) return crouchSpeed;
        if (IsSprinting) return sprintSpeed;
        return walkSpeed;
    }
    
    private void HandleLook()
    {
        // Get mouse delta
        float mouseX = lookInput.x * mouseSensitivity;
        float mouseY = lookInput.y * mouseSensitivity;
        
        // Rotate player body horizontally
        transform.Rotate(Vector3.up * mouseX);
        
        // Rotate camera vertically (clamped)
        xRotation -= mouseY;
        xRotation = Mathf.Clamp(xRotation, -maxLookAngle, maxLookAngle);
        
        if (cameraHolder != null)
        {
            cameraHolder.localRotation = Quaternion.Euler(xRotation, 0f, 0f);
        }
    }
    
    private void ApplyGravity()
    {
        // Apply stronger gravity when falling for snappier feel
        float currentGravity = velocity.y < 0 ? gravity * fallMultiplier : gravity;
        
        velocity.y += currentGravity * Time.deltaTime;
        
        // Apply vertical movement
        controller.Move(velocity * Time.deltaTime);
    }
    
    private void OnJump(InputAction.CallbackContext context)
    {
        if (isGrounded)
        {
            // Calculate jump velocity: v = sqrt(2 * height * gravity)
            // But we use direct force for more control
            velocity.y = Mathf.Sqrt(jumpForce * -2f * gravity);
        }
    }
    
    #endregion
    
    #region Public Methods
    
    /// <summary>
    /// Get the current move input (for animation system).
    /// </summary>
    public Vector2 GetMoveInput()
    {
        return moveInput;
    }
    
    /// <summary>
    /// Get the current vertical velocity (for animation system).
    /// </summary>
    public float GetVerticalVelocity()
    {
        return velocity.y;
    }
    
    /// <summary>
    /// Check if player is currently grounded (for animation system).
    /// </summary>
    public bool GetIsGrounded()
    {
        return isGrounded;
    }
    
    /// <summary>
    /// Check if player is currently sprinting (for animation system).
    /// </summary>
    public bool GetIsSprinting()
    {
        return IsSprinting;
    }
    
    /// <summary>
    /// Teleport the player to a position.
    /// </summary>
    public void Teleport(Vector3 position)
    {
        controller.enabled = false;
        transform.position = position;
        controller.enabled = true;
    }
    
    #endregion
    
    #region Debug
    
    private void OnDrawGizmosSelected()
    {
        // Draw ground check sphere
        if (groundCheck != null)
        {
            Gizmos.color = isGrounded ? Color.green : Color.red;
            Gizmos.DrawWireSphere(groundCheck.position, groundCheckRadius);
        }
    }
    
    #endregion
}
```

**Step 3: Save the Script**
- Ctrl+S (or Cmd+S on Mac)
- Return to Unity
- Wait for compilation (bottom right progress bar)

**Step 4: Add Script to Player**
1. Select `Player` object in Hierarchy
2. In Inspector, click "Add Component"
3. Search "PlayerController"
4. Click to add

**Step 5: Assign References**

In the PlayerController component:
- Camera Holder: Drag `CameraHolder` from Hierarchy
- Ground Check: Drag `GroundCheck` from Hierarchy
- Ground Mask: Click dropdown → Check "Ground" layer

**Step 6: Verify**
- [ ] No errors in Console
- [ ] All references assigned (no "None" fields)
- [ ] Camera Holder shows your CameraHolder
- [ ] Ground Mask shows "Ground"

---

## 2.4 Testing Movement

### Create a Test Floor:

**Step 1: Create Ground**
1. Hierarchy → 3D Object → Plane
2. Name: `Ground`
3. Position: (0, 0, 0)
4. Scale: (5, 1, 5) for a 50x50 unit floor
5. Layer: **Ground** (IMPORTANT!)
6. Tag: **Ground**

**Step 2: Position Player**
1. Select Player
2. Position: (0, 1, 0) - slightly above ground

**Step 3: Enter Play Mode**
1. Press Play button (or Ctrl+P)
2. Test controls:

| Control | Expected Result |
|---------|-----------------|
| WASD | Move in 4 directions |
| Mouse | Look around |
| Space | Jump |
| Shift (hold) + W | Sprint forward |
| Right Mouse (hold) | Aim mode (slower) |

### Troubleshooting:

| Problem | Cause | Solution |
|---------|-------|----------|
| Can't move | Input not working | Check Input Actions asset has bindings |
| Falls through floor | Ground layer wrong | Set ground to "Ground" layer |
| Camera doesn't move | Camera Holder not assigned | Assign in Inspector |
| No jump | Not detecting ground | Check Ground Mask includes Ground layer |
| Moves too fast/slow | Speed values | Adjust walkSpeed, sprintSpeed |
| Look too sensitive | Sensitivity too high | Lower mouseSensitivity |
| Stuck on ground | Ground check radius too small | Increase groundCheckRadius |

### Debug Visualization:

When Player is selected:
- You should see a green/red sphere at the feet
- Green = grounded, Red = not grounded
- If you never see green, ground check isn't working

---

# PART 3: PLAYER ANIMATION SYSTEM

## 3.1 Getting Animations from Mixamo

### Before You Start:
You need a Mixamo account (free, uses Adobe ID).

### Step-by-Step Mixamo Workflow:

**Step 1: Go to Mixamo**
1. Open browser
2. Go to: https://www.mixamo.com/
3. Sign in with Adobe account

**Step 2: Upload or Select Character**

**If you already have a character FBX:**
1. Click "Upload Character" button
2. Select your FBX file
3. Wait for auto-rigging
4. Adjust if needed (place markers on chin, wrists, elbows, knees, groin)
5. Click "Next" until done

**If using Mixamo character:**
1. Click "Characters" tab
2. Browse and select one you like
3. It's automatically rigged

**Step 3: Download Locomotion Animations**

For EACH animation in the list below:
1. Click "Animations" tab
2. Search for the animation name
3. Click to preview
4. Adjust settings (see table)
5. Click "Download"
6. Settings: Format: **FBX for Unity (.fbx)**, Skin: **Without Skin**
7. Click "Download"

### Master Animation Download List:


#### UNARMED Locomotion (Base Layer):

| # | Animation Name | Mixamo Search | In Place | Loop | Notes |
|---|---------------|---------------|----------|------|-------|
| 1 | Idle | "Breathing Idle" | ❌ | ✅ | Subtle breathing motion |
| 2 | Walk Forward | "Walking" | ✅ | ✅ | Standard walk |
| 3 | Walk Backward | "Walking Backwards" | ✅ | ✅ | Backward walk |
| 4 | Walk Left | "Left Strafe Walking" | ✅ | ✅ | Sidestep left |
| 5 | Walk Right | "Right Strafe Walking" | ✅ | ✅ | Sidestep right |
| 6 | Run Forward | "Running" | ✅ | ✅ | Standard run |
| 7 | Run Backward | "Running Backward" | ✅ | ✅ | Backward run |
| 8 | Sprint | "Fast Run" or "Sprint Forward" | ✅ | ✅ | Full sprint |
| 9 | Jump Start | "Jump" | ✅ | ❌ | Jump takeoff |
| 10 | Falling | "Falling Idle" | ✅ | ✅ | In-air pose |
| 11 | Landing | "Landing" | ✅ | ❌ | Land impact |

#### Weapon Holding (Upper Body Layer):

| # | Animation Name | Mixamo Search | In Place | Loop | Notes |
|---|---------------|---------------|----------|------|-------|
| 12 | Rifle Idle | "Rifle Aiming Idle" | ❌ | ✅ | Holding rifle pose |
| 13 | Rifle Walk | "Rifle Walk" | ✅ | ✅ | Optional: walk with rifle |
| 14 | Pistol Idle | "Pistol Idle" | ❌ | ✅ | Holding pistol |

> **⚠️ IMPORTANT SETTING: "In Place"**
> 
> When downloading locomotion animations (walk, run, etc.):
> - Check the ✅ "In Place" checkbox
> - This prevents the animation from moving the character
> - Your script controls movement, animation is just visual!

### Download Settings Explanation:

| Setting | Value | Why |
|---------|-------|-----|
| Format | FBX for Unity | Compatible with Unity |
| Skin | Without Skin | We already have the character, just need motion |
| Framerate | 30 | Standard for games |
| Keyframe Reduction | none | Keeps full quality |

---

## 3.2 Importing Animations to Unity

### Step-by-Step Import:

**Step 1: Organize Downloads**
1. Create folder: `_Project/Animations/Player/Locomotion/`
2. Move all downloaded FBX files there
3. Rename them clearly:
   - `Idle.fbx`
   - `WalkForward.fbx`
   - `WalkBackward.fbx`
   - `WalkLeft.fbx`
   - `WalkRight.fbx`
   - `RunForward.fbx`
   - `RunBackward.fbx`
   - `Sprint.fbx`
   - `JumpStart.fbx`
   - `Falling.fbx`
   - `Landing.fbx`

**Step 2: Import to Unity**
1. In Unity, navigate to the folder
2. All FBX files should appear
3. Wait for import (progress bar in bottom right)

**Step 3: Configure Each Animation FBX**

For EACH FBX file:
1. Select it in Project window
2. Look at Inspector (right side)
3. Click "Rig" tab
4. Set:
   - Animation Type: **Humanoid**
   - Avatar Definition: **Create From This Model** (first one) or **Copy From Other Avatar** (rest)
5. Click **Apply**
6. Click "Animation" tab
7. Select the clip in the list
8. Set:
   - Loop Time: ✅ (if it should loop)
   - Root Transform Rotation: Bake Into Pose ✅, Based Upon: Original
   - Root Transform Position (Y): Bake Into Pose ✅, Based Upon: Original
   - Root Transform Position (XZ): Bake Into Pose ✅, Based Upon: Original
9. Click **Apply**

### Why These Settings?

| Setting | Why |
|---------|-----|
| Humanoid | Allows retargeting to any humanoid character |
| Bake Into Pose | Prevents animation from moving the character |
| Loop Time | For continuous animations like walk/run |

---

## 3.3 Creating the Avatar

### Understanding Avatars:
An Avatar is Unity's mapping of bones to a standard humanoid structure. This allows:
- One animation to work on any humanoid character
- Easy retargeting between different models

### Setting Up Your Character's Avatar:

**Step 1: Select Character FBX**
1. Go to `_Project/Models/Characters/Player/`
2. Select your character's FBX file

**Step 2: Configure Rig**
1. In Inspector → Rig tab
2. Animation Type: **Humanoid**
3. Avatar Definition: **Create From This Model**
4. Click **Configure...**

**Step 3: Verify Bone Mapping**
1. Unity shows a T-pose figure
2. Green circles = bones mapped correctly
3. Red circles = problems!
4. If red circles exist:
   - Click on the red body part
   - Drag the correct bone from Hierarchy
5. Common Mixamo bone names:
   - Hips: mixamorig:Hips
   - Spine: mixamorig:Spine
   - Left Arm: mixamorig:LeftArm
   - etc.

**Step 4: Apply and Done**
1. Click **Done** button
2. Click **Apply**

**Step 5: Set Other Animations to Use This Avatar**
1. Select each animation FBX
2. Rig tab → Avatar Definition: **Copy From Other Avatar**
3. Source: Select your character's avatar (drag from Project)
4. Apply

---

## 3.4 Animator Controller Setup

### Understanding Animator Controllers:
The Animator Controller is a state machine that:
- Defines what animations exist
- Defines transitions between animations
- Uses parameters to control states

### Create Animator Controller:

**Step 1: Create**
1. Navigate to `_Project/Animations/Player/Controllers/`
2. Right-click → Create → Animator Controller
3. Name: `PlayerAnimatorController`
4. Double-click to open Animator window

**Step 2: Create Parameters**
1. In Animator window, click "Parameters" tab (left side)
2. Click "+" button for each parameter:

| Parameter | Type | Default | Purpose |
|-----------|------|---------|---------|
| Speed | Float | 0 | Movement speed magnitude |
| VelocityX | Float | 0 | Left/right movement |
| VelocityZ | Float | 0 | Forward/back movement |
| IsGrounded | Bool | true | On ground? |
| IsSprinting | Bool | false | Sprinting? |
| VerticalVelocity | Float | 0 | Up/down speed |
| Jump | Trigger | - | Jump command |

**Step 3: Create Layers**
1. Click "Layers" tab
2. You have "Base Layer" by default
3. Click "+" to add:
   - "Upper Body" (for weapon holding)
   - "Additive" (for reactions)

**Step 4: Configure Upper Body Layer**
1. Click gear icon next to "Upper Body"
2. Weight: 1
3. Mask: We'll create this later
4. Blending: Override

---

## 3.5 Blend Trees for Locomotion

### What Are Blend Trees?
Blend Trees smoothly blend between multiple animations based on parameters.
For movement, we use a 2D Blend Tree with X (strafe) and Y (forward/back).

### Create Locomotion Blend Tree:

**Step 1: Create Blend Tree State**
1. In Animator window, Base Layer
2. Right-click empty area → Create State → From New Blend Tree
3. Name it: "Locomotion"
4. This is now your default state (orange)

**Step 2: Configure Blend Tree**
1. Double-click "Locomotion" to enter it
2. Select the Blend Tree node
3. In Inspector:
   - Blend Type: **2D Freeform Directional**
   - Parameters: **VelocityX** (horizontal), **VelocityZ** (vertical)

**Step 3: Add Animation Clips**

Click "+" → Add Motion Field, then configure each:

| Motion | Pos X | Pos Y | Animation Clip |
|--------|-------|-------|----------------|
| Idle | 0 | 0 | Idle |
| Walk Forward | 0 | 0.5 | WalkForward |
| Walk Backward | 0 | -0.5 | WalkBackward |
| Walk Left | -0.5 | 0 | WalkLeft |
| Walk Right | 0.5 | 0 | WalkRight |
| Walk Forward-Left | -0.35 | 0.35 | WalkForward |
| Walk Forward-Right | 0.35 | 0.35 | WalkForward |
| Walk Backward-Left | -0.35 | -0.35 | WalkBackward |
| Walk Backward-Right | 0.35 | -0.35 | WalkBackward |
| Run Forward | 0 | 1 | RunForward |
| Run Backward | 0 | -1 | RunBackward |
| Run Left | -1 | 0 | WalkLeft |
| Run Right | 1 | 0 | WalkRight |

**Step 4: Drag Animation Clips**
1. For each motion field, the "Motion" column shows "None"
2. Drag the corresponding animation clip from Project window

### Visual Reference:
```
         (0, 1) Run Forward
              ↑
              │
   (-1,0)←────┼────→(1,0)
    Run Left  │    Run Right
              │
              ↓
         (0,-1) Run Backward
         
    Inner ring (0.5): Walk speeds
    Outer ring (1.0): Run speeds
    Center (0,0): Idle
```

---

## 3.6 Jump State Machine

### Understanding Jump Flow:

```
┌─────────────┐     Jump      ┌─────────────┐
│ Locomotion  │──────────────→│  JumpStart  │
│ (Blend Tree)│               │ (~0.3 sec)  │
└──────┬──────┘               └──────┬──────┘
       ↑                             │ After animation
       │                             ↓
       │                      ┌─────────────┐
       │                      │   Falling   │
       │                      │   (Loop)    │
       │                      └──────┬──────┘
       │                             │ IsGrounded = true
       │                             ↓
       │                      ┌─────────────┐
       └──────────────────────│   Landing   │
        After animation       │ (~0.3 sec)  │
                              └─────────────┘
```

### Create Jump States:

**Step 1: Return to Base Layer**
1. Click "Base Layer" in Animator breadcrumb (top)

**Step 2: Create States**
For each state:
1. Right-click → Create State → Empty
2. Name it
3. Assign animation in Inspector

| State Name | Animation | Loop Time |
|------------|-----------|-----------|
| JumpStart | JumpStart.anim | No |
| Falling | Falling.anim | Yes |
| Landing | Landing.anim | No |

**Step 3: Create Transitions**

**Locomotion → JumpStart:**
1. Right-click Locomotion → Make Transition
2. Click on JumpStart
3. Select the arrow
4. In Inspector:
   - Has Exit Time: ❌ NO
   - Transition Duration: 0.1
   - Conditions: Click "+" → Jump (trigger)

**JumpStart → Falling:**
1. Right-click JumpStart → Make Transition → Falling
2. Select arrow
3. Settings:
   - Has Exit Time: ✅ YES
   - Exit Time: 0.9
   - Transition Duration: 0.1
   - Conditions: (none needed, uses exit time)

**Falling → Landing:**
1. Right-click Falling → Make Transition → Landing
2. Settings:
   - Has Exit Time: ❌ NO
   - Transition Duration: 0.05
   - Conditions: IsGrounded = true

**Landing → Locomotion:**
1. Right-click Landing → Make Transition → Locomotion
2. Settings:
   - Has Exit Time: ✅ YES
   - Exit Time: 0.8
   - Transition Duration: 0.2
   - Conditions: (none)

**Locomotion → Falling (walking off ledge):**
1. Right-click Locomotion → Make Transition → Falling
2. Settings:
   - Has Exit Time: ❌ NO
   - Transition Duration: 0.1
   - Conditions: 
     - IsGrounded = false
     - VerticalVelocity Less than -1

---

## 3.7 PlayerAnimator Script

### Create the Script:

**Step 1: Create File**
1. `_Project/Scripts/Player/` → Create → C# Script
2. Name: `PlayerAnimator`

**Step 2: Full Code:**

```csharp
using UnityEngine;

/// <summary>
/// Handles player animation based on PlayerController state.
/// Attach to the same object as Animator (usually the character model).
/// </summary>
[RequireComponent(typeof(Animator))]
public class PlayerAnimator : MonoBehaviour
{
    #region Serialized Fields
    
    [Header("References")]
    [Tooltip("Reference to the PlayerController script")]
    [SerializeField] private PlayerController playerController;
    
    [Header("Smoothing")]
    [Tooltip("How quickly animation values change (lower = smoother, slower)")]
    [SerializeField] private float smoothTime = 0.1f;
    
    #endregion
    
    #region Private Fields
    
    // Component reference
    private Animator animator;
    
    // Smoothing variables
    private Vector2 currentVelocity;       // Current smoothed velocity
    private Vector2 smoothDampVelocity;    // Used by SmoothDamp internally
    
    // Animator parameter hashes (for performance)
    private static readonly int VelocityXHash = Animator.StringToHash("VelocityX");
    private static readonly int VelocityZHash = Animator.StringToHash("VelocityZ");
    private static readonly int SpeedHash = Animator.StringToHash("Speed");
    private static readonly int IsGroundedHash = Animator.StringToHash("IsGrounded");
    private static readonly int IsSprintingHash = Animator.StringToHash("IsSprinting");
    private static readonly int JumpHash = Animator.StringToHash("Jump");
    private static readonly int VerticalVelocityHash = Animator.StringToHash("VerticalVelocity");
    
    #endregion
    
    #region Unity Callbacks
    
    private void Awake()
    {
        // Get Animator component
        animator = GetComponent<Animator>();
        
        // Validate references
        if (playerController == null)
        {
            Debug.LogError("PlayerAnimator: PlayerController reference not assigned!");
        }
    }
    
    private void Update()
    {
        if (playerController == null || animator == null) return;
        
        UpdateLocomotion();
        UpdateStateParameters();
    }
    
    #endregion
    
    #region Animation Updates
    
    /// <summary>
    /// Updates locomotion blend tree parameters.
    /// </summary>
    private void UpdateLocomotion()
    {
        // Get raw input from player controller
        Vector2 moveInput = playerController.GetMoveInput();
        
        // Scale velocity based on sprint state
        // Sprint = full speed (1.0), Walk = half speed (0.5)
        float speedMultiplier = playerController.GetIsSprinting() ? 1f : 0.5f;
        Vector2 targetVelocity = moveInput * speedMultiplier;
        
        // Smooth the velocity change for fluid animation transitions
        // This prevents jarring snaps when changing direction
        currentVelocity = Vector2.SmoothDamp(
            currentVelocity,        // Current value
            targetVelocity,         // Target value
            ref smoothDampVelocity, // Velocity tracking (Vector2)
            smoothTime              // Smoothing time (float)
        );
        
        // Send values to animator
        animator.SetFloat(VelocityXHash, currentVelocity.x);
        animator.SetFloat(VelocityZHash, currentVelocity.y);
        animator.SetFloat(SpeedHash, currentVelocity.magnitude);
    }
    
    /// <summary>
    /// Updates state-related animator parameters.
    /// </summary>
    private void UpdateStateParameters()
    {
        animator.SetBool(IsGroundedHash, playerController.GetIsGrounded());
        animator.SetBool(IsSprintingHash, playerController.GetIsSprinting());
        animator.SetFloat(VerticalVelocityHash, playerController.GetVerticalVelocity());
    }
    
    #endregion
    
    #region Public Methods
    
    /// <summary>
    /// Call this when the player jumps.
    /// </summary>
    public void TriggerJump()
    {
        if (animator != null)
        {
            animator.SetTrigger(JumpHash);
        }
    }
    
    /// <summary>
    /// Play a specific animation by name.
    /// </summary>
    public void PlayAnimation(string animationName)
    {
        if (animator != null)
        {
            animator.Play(animationName);
        }
    }
    
    #endregion
}
```

**Step 3: Connect Jump Trigger**

In `PlayerController.cs`, update the `OnJump` method:

```csharp
// Add this field at the top with other serialized fields:
[Header("Animation")]
[SerializeField] private PlayerAnimator playerAnimator;

// Update the OnJump method:
private void OnJump(InputAction.CallbackContext context)
{
    if (isGrounded)
    {
        velocity.y = Mathf.Sqrt(jumpForce * -2f * gravity);
        
        // Trigger jump animation
        if (playerAnimator != null)
        {
            playerAnimator.TriggerJump();
        }
    }
}
```

---

## 3.8 Connecting Animation to Player

### Step-by-Step Setup:

**Step 1: Add Animator Component**
1. Select your character model (child of Player, e.g., "Alien")
2. It should already have an Animator component
3. If not: Add Component → Animator

**Step 2: Assign Animator Controller**
1. In Animator component
2. Controller: Drag `PlayerAnimatorController` from Project

**Step 3: Assign Avatar**
1. In Animator component
2. Avatar: Should auto-assign from your FBX
3. If blank: Drag your character's Avatar

**Step 4: Add PlayerAnimator Script**
1. Select the character model
2. Add Component → PlayerAnimator
3. Player Controller: Drag the `Player` object (parent)

**Step 5: Connect in PlayerController**
1. Select `Player` object
2. In PlayerController component
3. Player Animator: Drag the character model (child)

### Verify Connections:
```
Player (has PlayerController)
    ↓ references
Alien (has Animator + PlayerAnimator)
    ↑ references
PlayerController
```

---

# PART 4: FOOT IK SYSTEM (DETAILED)

## 4.1 Understanding Foot IK

### What is Foot IK?
Inverse Kinematics (IK) calculates how to position bones to reach a target.

**Without Foot IK:**
- Feet float above ground on slopes
- Feet clip through ground on uneven terrain
- Looks unnatural

**With Foot IK:**
- Feet plant on the actual ground surface
- Adjusts to slopes and stairs
- Looks professional

### How It Works:
1. Raycast from each foot downward
2. Find where the ray hits the ground
3. Move the IK target to that position
4. Two Bone IK rotates leg bones to reach target
5. Feet align to ground surface

---

## 4.2 Animation Rigging Package Setup

### Verify Package Installed:
1. Window → Package Manager
2. Check "In Project"
3. Confirm "Animation Rigging" is listed

### Understanding the Components:

| Component | Purpose |
|-----------|---------|
| Rig Builder | Master controller on the root animated object |
| Rig | Contains IK constraints |
| Two Bone IK Constraint | Controls 2-bone chain (leg) |

---

## 4.3 Creating the Rig Hierarchy

### Your Current Hierarchy:
```
Player (rigidbody, playercontroller)
└── Alien (player model)
    ├── Ch44 (Skin Mesh Renderer)
    └── mixamorig:Hips (bones)
        ├── mixamorig:Spine → ...
        ├── mixamorig:LeftUpLeg → LeftLeg → LeftFoot
        └── mixamorig:RightUpLeg → RightLeg → RightFoot
```

### What You Need to Create:
```
Player
└── Alien
    ├── Ch44
    ├── mixamorig:Hips (bones)
    └── FootIKRig  ← CREATE THIS
        ├── LeftFootIK  ← CREATE THIS
        │   ├── LeftFootTarget  ← CREATE THIS
        │   └── LeftFootHint    ← CREATE THIS
        └── RightFootIK  ← CREATE THIS
            ├── RightFootTarget  ← CREATE THIS
            └── RightFootHint    ← CREATE THIS
```

### Step-by-Step Creation:

**Step 1: Create FootIKRig**
1. Right-click `Alien` → Create Empty
2. Name: `FootIKRig`
3. Position: (0, 0, 0) - same as parent
4. Add Component: **Rig** (search for it)

**Step 2: Create LeftFootIK**
1. Right-click `FootIKRig` → Create Empty
2. Name: `LeftFootIK`
3. Position: (0, 0, 0)
4. Add Component: **Two Bone IK Constraint**

**Step 3: Create Left Foot Target**
1. Right-click `LeftFootIK` → Create Empty
2. Name: `LeftFootTarget`
3. Position it at the LEFT FOOT's current position:
   - Find `mixamorig:LeftFoot` bone in hierarchy
   - Note its world position
   - Set LeftFootTarget to same position

**Step 4: Create Left Foot Hint**
1. Right-click `LeftFootIK` → Create Empty
2. Name: `LeftFootHint`
3. Position it BEHIND the LEFT KNEE:
   - Find `mixamorig:LeftLeg` bone (the knee)
   - Position the hint slightly behind it (negative Z)
   - This controls which way the knee bends!

**Step 5: Repeat for Right Leg**
1. Create `RightFootIK` under FootIKRig
2. Add Two Bone IK Constraint
3. Create `RightFootTarget` (at right foot position)
4. Create `RightFootHint` (behind right knee)

### Visual Positioning Guide:
```
       Top View (looking down)
       
           Forward (Z+)
              ↑
              │
    Left ←────┼────→ Right
              │
              ↓
           Back (Z-)

Hints should be BEHIND the knees (negative Z from knee)

       Side View
       
       ●──● Thigh
          │
     Hint ○│● Knee
          │
          ● Foot ← Target
    ═══════════════ Ground
```

---

## 4.4 Two Bone IK Constraints Setup

### This is the Critical Part!

You need **2 Two Bone IK Constraints** - one for EACH leg.

### Left Leg Setup:

**Step 1: Select LeftFootIK**
1. Click on `LeftFootIK` in Hierarchy
2. Find `Two Bone IK Constraint` component

**Step 2: Configure Constraint**

| Setting | Value | How to Assign |
|---------|-------|---------------|
| Weight | 1 | Default |
| **Root** | mixamorig:LeftUpLeg | Drag from Hierarchy |
| **Mid** | mixamorig:LeftLeg | Drag from Hierarchy |
| **Tip** | mixamorig:LeftFoot | Drag from Hierarchy |
| **Target** | LeftFootTarget | Drag from Hierarchy |
| Target Position Weight | 1 | Default |
| Target Rotation Weight | 1 | Default |
| **Hint** | LeftFootHint | Drag from Hierarchy |
| Hint Weight | 1 | Default |

### Right Leg Setup:

**Step 1: Select RightFootIK**

**Step 2: Configure Constraint**

| Setting | Value | How to Assign |
|---------|-------|---------------|
| Weight | 1 | Default |
| **Root** | mixamorig:RightUpLeg | Drag from Hierarchy |
| **Mid** | mixamorig:RightLeg | Drag from Hierarchy |
| **Tip** | mixamorig:RightFoot | Drag from Hierarchy |
| **Target** | RightFootTarget | Drag from Hierarchy |
| Target Position Weight | 1 | |
| Target Rotation Weight | 1 | |
| **Hint** | RightFootHint | Drag from Hierarchy |
| Hint Weight | 1 | |

### Add Rig Builder:

**Step 1: Select Alien (character model)**
1. Click on `Alien` in Hierarchy
2. Add Component → **Rig Builder**

**Step 2: Assign Rig**
1. In Rig Builder component
2. Click "+" under "Rig Layers"
3. Drag `FootIKRig` into the slot

### Verify Setup:
- [ ] FootIKRig has Rig component
- [ ] LeftFootIK has Two Bone IK Constraint with all bones assigned
- [ ] RightFootIK has Two Bone IK Constraint with all bones assigned
- [ ] Alien has Rig Builder with FootIKRig in Rig Layers

---

## 4.5 FootIK Script

### Create the Script:

`_Project/Scripts/Player/FootIK.cs`

```csharp
using UnityEngine;
using UnityEngine.Animations.Rigging;

/// <summary>
/// Controls foot IK to make feet align with ground surface.
/// Requires Two Bone IK Constraints set up for each leg.
/// </summary>
public class FootIK : MonoBehaviour
{
    #region Serialized Fields
    
    [Header("IK Constraints")]
    [Tooltip("Two Bone IK Constraint for left leg")]
    [SerializeField] private TwoBoneIKConstraint leftFootIK;
    
    [Tooltip("Two Bone IK Constraint for right leg")]
    [SerializeField] private TwoBoneIKConstraint rightFootIK;
    
    [Header("IK Targets")]
    [Tooltip("Target transform for left foot")]
    [SerializeField] private Transform leftFootTarget;
    
    [Tooltip("Target transform for right foot")]
    [SerializeField] private Transform rightFootTarget;
    
    [Header("Foot Bones (for reference positions)")]
    [Tooltip("The actual left foot bone")]
    [SerializeField] private Transform leftFootBone;
    
    [Tooltip("The actual right foot bone")]
    [SerializeField] private Transform rightFootBone;
    
    [Header("Raycast Settings")]
    [Tooltip("Layers to check for ground")]
    [SerializeField] private LayerMask groundMask;
    
    [Tooltip("How far up to start the raycast")]
    [SerializeField] private float raycastOriginHeight = 0.5f;
    
    [Tooltip("How far down to raycast")]
    [SerializeField] private float raycastDistance = 1.5f;
    
    [Tooltip("How high above ground to place foot")]
    [SerializeField] private float footOffset = 0.1f;
    
    [Header("IK Behavior")]
    [Tooltip("How fast IK interpolates to target")]
    [SerializeField] private float ikSpeed = 15f;
    
    [Tooltip("Enable/disable IK at runtime")]
    [SerializeField] private bool enableIK = true;
    
    [Tooltip("Only apply IK when standing still or walking slowly")]
    [SerializeField] private float maxSpeedForIK = 3f;
    
    [Header("References")]
    [Tooltip("Reference to player controller for speed check")]
    [SerializeField] private PlayerController playerController;
    
    #endregion
    
    #region Private Fields
    
    // Current IK positions and rotations
    private Vector3 leftFootIKPosition;
    private Vector3 rightFootIKPosition;
    private Quaternion leftFootIKRotation;
    private Quaternion rightFootIKRotation;
    
    // IK weight (0 = no IK, 1 = full IK)
    private float currentIKWeight = 0f;
    
    #endregion
    
    #region Unity Callbacks
    
    private void Start()
    {
        // Initialize IK positions to current foot positions
        if (leftFootBone != null)
        {
            leftFootIKPosition = leftFootBone.position;
            leftFootIKRotation = leftFootBone.rotation;
        }
        if (rightFootBone != null)
        {
            rightFootIKPosition = rightFootBone.position;
            rightFootIKRotation = rightFootBone.rotation;
        }
    }
    
    private void LateUpdate()
    {
        if (!enableIK) 
        {
            SetIKWeight(0f);
            return;
        }
        
        // Determine if IK should be active
        bool shouldUseIK = ShouldApplyIK();
        float targetWeight = shouldUseIK ? 1f : 0f;
        
        // Smoothly transition IK weight
        currentIKWeight = Mathf.Lerp(currentIKWeight, targetWeight, Time.deltaTime * 10f);
        SetIKWeight(currentIKWeight);
        
        if (shouldUseIK)
        {
            // Update foot IK
            UpdateFootIK(leftFootBone, ref leftFootIKPosition, ref leftFootIKRotation, leftFootTarget);
            UpdateFootIK(rightFootBone, ref rightFootIKPosition, ref rightFootIKRotation, rightFootTarget);
        }
    }
    
    #endregion
    
    #region IK Logic
    
    /// <summary>
    /// Determines if IK should be applied based on player state.
    /// </summary>
    private bool ShouldApplyIK()
    {
        if (playerController == null) return true;
        
        // Only apply IK when grounded and moving slowly
        bool isGrounded = playerController.GetIsGrounded();
        bool isMovingSlowly = playerController.CurrentSpeed < maxSpeedForIK;
        
        return isGrounded && isMovingSlowly;
    }
    
    /// <summary>
    /// Updates IK for a single foot.
    /// </summary>
    private void UpdateFootIK(Transform footBone, ref Vector3 ikPosition, ref Quaternion ikRotation, Transform target)
    {
        if (footBone == null || target == null) return;
        
        // Calculate raycast origin (above the foot)
        Vector3 rayOrigin = footBone.position + Vector3.up * raycastOriginHeight;
        
        // Perform raycast
        if (Physics.Raycast(rayOrigin, Vector3.down, out RaycastHit hit, raycastDistance, groundMask))
        {
            // Calculate target position (on ground + offset)
            Vector3 targetPosition = hit.point + Vector3.up * footOffset;
            
            // Smoothly move toward target
            ikPosition = Vector3.Lerp(ikPosition, targetPosition, Time.deltaTime * ikSpeed);
            
            // Calculate rotation to align foot with surface normal
            Quaternion slopeRotation = Quaternion.FromToRotation(Vector3.up, hit.normal);
            Quaternion targetRotation = slopeRotation * transform.rotation;
            
            // Smoothly rotate toward target
            ikRotation = Quaternion.Slerp(ikRotation, targetRotation, Time.deltaTime * ikSpeed);
        }
        
        // Apply to target
        target.position = ikPosition;
        target.rotation = ikRotation;
    }
    
    /// <summary>
    /// Sets the weight of both IK constraints.
    /// </summary>
    private void SetIKWeight(float weight)
    {
        if (leftFootIK != null) leftFootIK.weight = weight;
        if (rightFootIK != null) rightFootIK.weight = weight;
    }
    
    #endregion
    
    #region Debug
    
    private void OnDrawGizmosSelected()
    {
        // Draw raycast lines
        if (leftFootBone != null)
        {
            Vector3 origin = leftFootBone.position + Vector3.up * raycastOriginHeight;
            Gizmos.color = Color.green;
            Gizmos.DrawLine(origin, origin + Vector3.down * raycastDistance);
            Gizmos.DrawWireSphere(leftFootIKPosition, 0.05f);
        }
        
        if (rightFootBone != null)
        {
            Vector3 origin = rightFootBone.position + Vector3.up * raycastOriginHeight;
            Gizmos.color = Color.red;
            Gizmos.DrawLine(origin, origin + Vector3.down * raycastDistance);
            Gizmos.DrawWireSphere(rightFootIKPosition, 0.05f);
        }
    }
    
    #endregion
}
```

### Assign References:

1. Add `FootIK` script to the character model (Alien)
2. Assign in Inspector:

| Field | Drag From |
|-------|-----------|
| Left Foot IK | LeftFootIK object |
| Right Foot IK | RightFootIK object |
| Left Foot Target | LeftFootTarget object |
| Right Foot Target | RightFootTarget object |
| Left Foot Bone | mixamorig:LeftFoot |
| Right Foot Bone | mixamorig:RightFoot |
| Ground Mask | Check "Ground" layer |
| Player Controller | Player object |

---

## 4.6 Testing and Troubleshooting

### Test Setup:

1. Create uneven ground:
   - Add cubes as steps
   - Add tilted planes as ramps
   - Set all to "Ground" layer!

2. Enter Play mode

3. Walk on uneven surfaces

### What to Look For:

| ✅ Working Correctly | ❌ Problem |
|---------------------|-----------|
| Feet land on surfaces | Feet float above ground |
| Feet follow slopes | Feet clip through ground |
| Smooth transitions | Jerky foot movement |
| Natural knee bend | Knees bend backwards |

### Common Problems and Solutions:

**Feet floating above ground:**
- Decrease `footOffset`
- Increase `raycastDistance`
- Check Ground layer on surfaces

**Feet clipping through ground:**
- Increase `footOffset`
- Check raycast is hitting correctly (use Gizmos)

**Knees bending backwards:**
- Move hint transforms further behind knees
- Make sure hints are in correct position

**IK not working at all:**
- Check Rig Builder has Rig assigned
- Check Rig component exists on FootIKRig
- Check Two Bone IK bones are assigned
- Check Weight is 1 on constraints

**IK snapping/jerky:**
- Increase `ikSpeed` for faster response
- Decrease `ikSpeed` for smoother blending
- Check if IK weight is transitioning properly

---

# PART 5: FIRST-PERSON ARMS & CAMERA

## 5.1 First-Person Setup Overview

### Understanding First-Person vs Third-Person:

In FPS games, you typically have:
- **First-Person (1P)**: What YOU see - just arms and weapon
- **Third-Person (3P)**: What OTHERS see - full body (for multiplayer shadows)

For singleplayer, you CAN use only first-person arms and hide the 3P body.

### Setup Options:

**Option A: First-Person Only (Simpler)**
```
Player
├── CameraHolder
│   └── Main Camera
│       └── FPSArms      ← Arm model parented to camera
│           └── WeaponHolder
└── (No 3P body visible)
```

**Option B: Both 1P and 3P (More Complete)**
```
Player
├── CameraHolder
│   └── Main Camera (culls 3P)
│       └── FPSArms (Layer: FirstPerson)
└── ThirdPersonBody (Layer: ThirdPerson, camera culled)
```

### For This Guide, We'll Use Option A (Simpler)

---

## 5.2 Camera System

### Basic First-Person Camera:

The camera is already set up from Part 2:
- CameraHolder rotates vertically (pitch)
- Player rotates horizontally (yaw)
- Camera is child of CameraHolder

### Adding Cinemachine (Optional but Recommended):

**Step 1: Create Virtual Camera**
1. Hierarchy → Cinemachine → Virtual Camera
2. Name: `PlayerFollowCam`

**Step 2: Configure**
1. Select PlayerFollowCam
2. Follow: CameraHolder
3. Look At: (leave empty)
4. Body: Do Nothing
5. Aim: Do Nothing

This gives you Cinemachine's features (screenshake, etc.) with manual control.

---

## 5.3 First-Person Arms Model

### Getting FPS Arms:

**Option A: Asset Store**
- Search "FPS Arms" - many free and paid options
- Usually includes hand models and basic animations

**Option B: Use Your Character's Arms**
- Duplicate just the arm bones from your character
- More complex but matches your character

**Option C: Create Simple Placeholder**
- For prototyping, use simple capsules as arms
- Replace later with real model

### Setting Up Arms:

**Step 1: Import FPS Arms Model**
1. Import to `_Project/Models/Characters/Player/`
2. Configure rig as Humanoid (if full body) or Generic (if arms only)

**Step 2: Create Hierarchy**
```
CameraHolder
└── Main Camera
    └── FPSArms (the arm model)
        └── WeaponHolder (empty transform for weapon)
```

**Step 3: Position Arms**
1. FPSArms local position: (0, -0.2, 0.3) approximately
2. Adjust until arms look natural in Game view
3. Enter Play mode to verify position while looking around

**Step 4: Camera Culling**
1. Create layer "FirstPerson" (if not already)
2. Set FPSArms and children to FirstPerson layer
3. Main Camera → Culling Mask → Include FirstPerson

---

## 5.4 Weapon Sway and Bob

### Weapon Sway Script:

`_Project/Scripts/Player/WeaponSway.cs`

```csharp
using UnityEngine;

/// <summary>
/// Makes the weapon sway when looking around for more immersion.
/// Attach to the weapon or FPS arms.
/// </summary>
public class WeaponSway : MonoBehaviour
{
    [Header("Position Sway")]
    [SerializeField] private float swayAmount = 0.02f;
    [SerializeField] private float maxSway = 0.06f;
    [SerializeField] private float positionSmoothing = 6f;
    
    [Header("Rotation Sway")]
    [SerializeField] private float rotationSwayAmount = 4f;
    [SerializeField] private float maxRotationSway = 5f;
    [SerializeField] private float rotationSmoothing = 12f;
    
    [Header("Settings")]
    [SerializeField] private bool invertX = false;
    [SerializeField] private bool invertY = false;
    
    private Vector3 initialPosition;
    private Quaternion initialRotation;
    
    private void Start()
    {
        initialPosition = transform.localPosition;
        initialRotation = transform.localRotation;
    }
    
    private void Update()
    {
        // Get mouse input
        float mouseX = Input.GetAxis("Mouse X");
        float mouseY = Input.GetAxis("Mouse Y");
        
        if (invertX) mouseX = -mouseX;
        if (invertY) mouseY = -mouseY;
        
        // Calculate position sway
        float targetX = Mathf.Clamp(-mouseX * swayAmount, -maxSway, maxSway);
        float targetY = Mathf.Clamp(-mouseY * swayAmount, -maxSway, maxSway);
        Vector3 targetPosition = initialPosition + new Vector3(targetX, targetY, 0);
        
        // Calculate rotation sway
        float targetRotX = Mathf.Clamp(-mouseY * rotationSwayAmount, -maxRotationSway, maxRotationSway);
        float targetRotY = Mathf.Clamp(mouseX * rotationSwayAmount, -maxRotationSway, maxRotationSway);
        Quaternion targetRotation = initialRotation * Quaternion.Euler(targetRotX, targetRotY, 0);
        
        // Apply with smoothing
        transform.localPosition = Vector3.Lerp(transform.localPosition, targetPosition, positionSmoothing * Time.deltaTime);
        transform.localRotation = Quaternion.Slerp(transform.localRotation, targetRotation, rotationSmoothing * Time.deltaTime);
    }
}
```

### Weapon Bob Script:

`_Project/Scripts/Player/WeaponBob.cs`

```csharp
using UnityEngine;

/// <summary>
/// Makes the weapon bob while moving for more immersion.
/// Attach to the weapon or FPS arms.
/// </summary>
public class WeaponBob : MonoBehaviour
{
    [Header("Bob Settings")]
    [SerializeField] private float walkBobSpeed = 10f;
    [SerializeField] private float walkBobAmount = 0.01f;
    [SerializeField] private float sprintBobSpeed = 14f;
    [SerializeField] private float sprintBobAmount = 0.025f;
    [SerializeField] private float idleReturnSpeed = 5f;
    
    [Header("References")]
    [SerializeField] private PlayerController playerController;
    
    private float defaultYPos;
    private float defaultXPos;
    private float timer;
    
    private void Start()
    {
        defaultYPos = transform.localPosition.y;
        defaultXPos = transform.localPosition.x;
    }
    
    private void Update()
    {
        if (playerController == null) return;
        
        // Check if moving and grounded
        bool isMoving = playerController.CurrentSpeed > 0.1f;
        bool isGrounded = playerController.GetIsGrounded();
        
        if (!isGrounded || !isMoving)
        {
            // Return to default position
            timer = 0;
            ReturnToDefault();
            return;
        }
        
        // Determine bob parameters
        bool isSprinting = playerController.GetIsSprinting();
        float currentBobSpeed = isSprinting ? sprintBobSpeed : walkBobSpeed;
        float currentBobAmount = isSprinting ? sprintBobAmount : walkBobAmount;
        
        // Calculate bob
        timer += Time.deltaTime * currentBobSpeed;
        
        float newY = defaultYPos + Mathf.Sin(timer) * currentBobAmount;
        float newX = defaultXPos + Mathf.Cos(timer / 2) * currentBobAmount * 0.5f;
        
        // Apply
        Vector3 newPos = transform.localPosition;
        newPos.y = Mathf.Lerp(newPos.y, newY, Time.deltaTime * 10f);
        newPos.x = Mathf.Lerp(newPos.x, newX, Time.deltaTime * 10f);
        transform.localPosition = newPos;
    }
    
    private void ReturnToDefault()
    {
        Vector3 newPos = transform.localPosition;
        newPos.y = Mathf.Lerp(newPos.y, defaultYPos, Time.deltaTime * idleReturnSpeed);
        newPos.x = Mathf.Lerp(newPos.x, defaultXPos, Time.deltaTime * idleReturnSpeed);
        transform.localPosition = newPos;
    }
}
```

---

# PART 6: WEAPONS SYSTEM

## 6.1 Weapon Architecture

### Overview:

```
WeaponData (ScriptableObject)
    ↓ provides stats
Weapon (MonoBehaviour)
    ↓ handles logic
WeaponManager (handles switching)
```

---

## 6.2 WeaponData ScriptableObject

`_Project/Scripts/Weapons/WeaponData.cs`

```csharp
using UnityEngine;

/// <summary>
/// Holds all data for a weapon type.
/// Create instances via: Create → Game → Weapon Data
/// </summary>
[CreateAssetMenu(fileName = "NewWeapon", menuName = "Game/Weapon Data")]
public class WeaponData : ScriptableObject
{
    [Header("Basic Info")]
    public string weaponName = "Weapon";
    public Sprite icon;
    public GameObject prefab;
    
    [Header("Damage")]
    public float damage = 30f;
    public float headshotMultiplier = 2f;
    public float range = 100f;
    
    [Header("Fire")]
    public float fireRate = 0.1f;  // Time between shots
    public bool isAutomatic = true;
    public int pelletsPerShot = 1;  // For shotguns
    public float spreadAngle = 0f;
    
    [Header("Ammo")]
    public int magazineSize = 30;
    public int maxReserveAmmo = 120;
    public float reloadTime = 2f;
    
    [Header("Recoil")]
    public float recoilX = 0.5f;   // Horizontal recoil
    public float recoilY = 1f;     // Vertical recoil
    public float recoilRecovery = 5f;
    
    [Header("ADS")]
    public float adsSpeed = 0.2f;
    public float adsZoom = 1.2f;
    
    [Header("Economy")]
    public int wallPrice = 500;
    public int ammoPrice = 250;
    public int packAPunchPrice = 5000;
    
    [Header("Audio")]
    public AudioClip fireSound;
    public AudioClip reloadSound;
    public AudioClip emptySound;
    public AudioClip equipSound;
    
    [Header("VFX")]
    public GameObject muzzleFlashPrefab;
    public GameObject bulletHolePrefab;
    public GameObject bulletTracerPrefab;
}
```

### Create Weapon Data:

1. Right-click `_Project/ScriptableObjects/Weapons/`
2. Create → Game → Weapon Data
3. Name: "Pistol", "SMG", "AssaultRifle", etc.
4. Fill in all fields

---

## 6.3 Base Weapon Script

`_Project/Scripts/Weapons/Weapon.cs`

```csharp
using UnityEngine;
using System.Collections;

/// <summary>
/// Base weapon class handling shooting, reloading, and ammo.
/// </summary>
public class Weapon : MonoBehaviour
{
    [Header("Weapon Data")]
    [SerializeField] private WeaponData data;
    
    [Header("References")]
    [SerializeField] private Transform firePoint;
    [SerializeField] private ParticleSystem muzzleFlash;
    [SerializeField] private AudioSource audioSource;
    [SerializeField] private Camera playerCamera;
    
    // Current state
    private int currentAmmo;
    private int reserveAmmo;
    private float nextFireTime;
    private bool isReloading;
    private bool isAiming;
    
    // Events
    public System.Action OnFire;
    public System.Action OnReload;
    public System.Action OnAmmoChanged;
    
    #region Properties
    
    public WeaponData Data => data;
    public int CurrentAmmo => currentAmmo;
    public int ReserveAmmo => reserveAmmo;
    public bool IsReloading => isReloading;
    public bool IsAiming => isAiming;
    
    #endregion
    
    #region Unity Callbacks
    
    private void Start()
    {
        if (data != null)
        {
            currentAmmo = data.magazineSize;
            reserveAmmo = data.maxReserveAmmo;
        }
        
        if (audioSource == null)
        {
            audioSource = GetComponent<AudioSource>();
        }
        
        if (playerCamera == null)
        {
            playerCamera = Camera.main;
        }
    }
    
    #endregion
    
    #region Public Methods
    
    /// <summary>
    /// Attempt to fire the weapon.
    /// </summary>
    public void TryFire()
    {
        if (data == null) return;
        if (isReloading) return;
        if (Time.time < nextFireTime) return;
        
        if (currentAmmo <= 0)
        {
            // Play empty click sound
            if (data.emptySound != null && audioSource != null)
            {
                audioSource.PlayOneShot(data.emptySound);
            }
            
            // Auto reload if we have reserve ammo
            if (reserveAmmo > 0)
            {
                StartCoroutine(ReloadCoroutine());
            }
            return;
        }
        
        Fire();
    }
    
    /// <summary>
    /// Start reloading if possible.
    /// </summary>
    public void TryReload()
    {
        if (data == null) return;
        if (isReloading) return;
        if (currentAmmo >= data.magazineSize) return;
        if (reserveAmmo <= 0) return;
        
        StartCoroutine(ReloadCoroutine());
    }
    
    /// <summary>
    /// Add ammo to reserve.
    /// </summary>
    public void AddAmmo(int amount)
    {
        reserveAmmo = Mathf.Min(reserveAmmo + amount, data.maxReserveAmmo);
        OnAmmoChanged?.Invoke();
    }
    
    /// <summary>
    /// Refill all ammo.
    /// </summary>
    public void RefillAmmo()
    {
        currentAmmo = data.magazineSize;
        reserveAmmo = data.maxReserveAmmo;
        OnAmmoChanged?.Invoke();
    }
    
    #endregion
    
    #region Private Methods
    
    private void Fire()
    {
        currentAmmo--;
        nextFireTime = Time.time + data.fireRate;
        
        // Fire sound
        if (data.fireSound != null && audioSource != null)
        {
            audioSource.PlayOneShot(data.fireSound);
        }
        
        // Muzzle flash
        if (muzzleFlash != null)
        {
            muzzleFlash.Play();
        }
        
        // Perform raycast(s)
        for (int i = 0; i < data.pelletsPerShot; i++)
        {
            FireRaycast();
        }
        
        OnFire?.Invoke();
        OnAmmoChanged?.Invoke();
    }
    
    private void FireRaycast()
    {
        // Calculate spread
        Vector3 direction = playerCamera.transform.forward;
        if (data.spreadAngle > 0)
        {
            direction = Quaternion.Euler(
                Random.Range(-data.spreadAngle, data.spreadAngle),
                Random.Range(-data.spreadAngle, data.spreadAngle),
                0
            ) * direction;
        }
        
        // Raycast
        Ray ray = new Ray(playerCamera.transform.position, direction);
        if (Physics.Raycast(ray, out RaycastHit hit, data.range))
        {
            // Deal damage
            if (hit.collider.TryGetComponent<IDamageable>(out var damageable))
            {
                float damage = data.damage;
                
                // Check for headshot
                if (hit.collider.CompareTag("Head"))
                {
                    damage *= data.headshotMultiplier;
                }
                
                damageable.TakeDamage(damage, hit.point, ray.direction);
            }
            
            // Spawn bullet hole
            if (data.bulletHolePrefab != null)
            {
                Instantiate(data.bulletHolePrefab, hit.point + hit.normal * 0.01f, 
                    Quaternion.LookRotation(hit.normal));
            }
        }
    }
    
    private IEnumerator ReloadCoroutine()
    {
        isReloading = true;
        
        // Play reload sound
        if (data.reloadSound != null && audioSource != null)
        {
            audioSource.PlayOneShot(data.reloadSound);
        }
        
        OnReload?.Invoke();
        
        // Wait for reload
        yield return new WaitForSeconds(data.reloadTime);
        
        // Calculate how much ammo to reload
        int ammoNeeded = data.magazineSize - currentAmmo;
        int ammoToReload = Mathf.Min(ammoNeeded, reserveAmmo);
        
        currentAmmo += ammoToReload;
        reserveAmmo -= ammoToReload;
        
        isReloading = false;
        OnAmmoChanged?.Invoke();
    }
    
    #endregion
}
```

### Create Damage Interface:

`_Project/Scripts/Interfaces/IDamageable.cs`

```csharp
using UnityEngine;

/// <summary>
/// Interface for anything that can take damage.
/// </summary>
public interface IDamageable
{
    void TakeDamage(float damage, Vector3 hitPoint, Vector3 hitDirection);
}
```

---

## 6.4 Hitscan Weapons

The base Weapon script already handles hitscan (raycast-based) weapons. For projectile weapons (rockets, grenades), you'd create a separate ProjectileWeapon class.

---

## 6.5 Weapon Switching

`_Project/Scripts/Weapons/WeaponManager.cs`

```csharp
using UnityEngine;
using System.Collections.Generic;

/// <summary>
/// Manages weapon inventory and switching.
/// </summary>
public class WeaponManager : MonoBehaviour
{
    [Header("Weapon Slots")]
    [SerializeField] private Transform weaponHolder;
    [SerializeField] private int maxWeapons = 2;
    
    [Header("Starting Weapon")]
    [SerializeField] private WeaponData startingWeapon;
    
    // Current weapons
    private List<Weapon> weapons = new List<Weapon>();
    private int currentWeaponIndex = -1;
    
    // Input
    private PlayerInputActions inputActions;
    
    #region Properties
    
    public Weapon CurrentWeapon => currentWeaponIndex >= 0 ? weapons[currentWeaponIndex] : null;
    public List<Weapon> AllWeapons => weapons;
    
    #endregion
    
    #region Unity Callbacks
    
    private void Awake()
    {
        inputActions = new PlayerInputActions();
    }
    
    private void OnEnable()
    {
        inputActions.Player.Enable();
        inputActions.Player.Fire.performed += ctx => CurrentWeapon?.TryFire();
        inputActions.Player.Reload.performed += ctx => CurrentWeapon?.TryReload();
        inputActions.Player.SwitchWeapon.performed += ctx => SwitchWeapon();
    }
    
    private void OnDisable()
    {
        inputActions.Player.Disable();
    }
    
    private void Start()
    {
        // Equip starting weapon
        if (startingWeapon != null)
        {
            AddWeapon(startingWeapon);
        }
    }
    
    private void Update()
    {
        // Hold to fire for automatic weapons
        if (inputActions.Player.Fire.ReadValue<float>() > 0)
        {
            if (CurrentWeapon != null && CurrentWeapon.Data.isAutomatic)
            {
                CurrentWeapon.TryFire();
            }
        }
    }
    
    #endregion
    
    #region Public Methods
    
    /// <summary>
    /// Add a weapon to inventory.
    /// </summary>
    public bool AddWeapon(WeaponData weaponData)
    {
        // Check if already have this weapon type
        foreach (var weapon in weapons)
        {
            if (weapon.Data == weaponData)
            {
                // Refill ammo instead
                weapon.RefillAmmo();
                return false;
            }
        }
        
        // Check if have room
        if (weapons.Count >= maxWeapons)
        {
            // Replace current weapon
            RemoveCurrentWeapon();
        }
        
        // Spawn weapon
        if (weaponData.prefab != null)
        {
            GameObject weaponObj = Instantiate(weaponData.prefab, weaponHolder);
            weaponObj.transform.localPosition = Vector3.zero;
            weaponObj.transform.localRotation = Quaternion.identity;
            
            Weapon weapon = weaponObj.GetComponent<Weapon>();
            if (weapon != null)
            {
                weapons.Add(weapon);
                EquipWeapon(weapons.Count - 1);
                return true;
            }
        }
        
        return false;
    }
    
    /// <summary>
    /// Switch to next weapon.
    /// </summary>
    public void SwitchWeapon()
    {
        if (weapons.Count <= 1) return;
        
        int nextIndex = (currentWeaponIndex + 1) % weapons.Count;
        EquipWeapon(nextIndex);
    }
    
    #endregion
    
    #region Private Methods
    
    private void EquipWeapon(int index)
    {
        // Hide current weapon
        if (currentWeaponIndex >= 0 && currentWeaponIndex < weapons.Count)
        {
            weapons[currentWeaponIndex].gameObject.SetActive(false);
        }
        
        // Show new weapon
        currentWeaponIndex = index;
        if (currentWeaponIndex >= 0 && currentWeaponIndex < weapons.Count)
        {
            weapons[currentWeaponIndex].gameObject.SetActive(true);
        }
    }
    
    private void RemoveCurrentWeapon()
    {
        if (currentWeaponIndex < 0 || currentWeaponIndex >= weapons.Count) return;
        
        Weapon weaponToRemove = weapons[currentWeaponIndex];
        weapons.RemoveAt(currentWeaponIndex);
        Destroy(weaponToRemove.gameObject);
        
        currentWeaponIndex = Mathf.Clamp(currentWeaponIndex - 1, 0, weapons.Count - 1);
    }
    
    #endregion
}
```

---

## 6.6 Reloading System

Reloading is handled in the Weapon script via the `ReloadCoroutine`. To add animations:

1. Add reload animation to FPS Arms animator
2. Call animation from Weapon script
3. Sync timing with `data.reloadTime`

---

# PART 7: ZOMBIE AI SYSTEM

## 7.1 NavMesh Setup

### Understanding NavMesh:
NavMesh (Navigation Mesh) is a map of walkable areas that AI uses for pathfinding.

### Bake NavMesh:

**Step 1: Open Navigation Window**
1. Window → AI → Navigation

**Step 2: Mark Walkable Surfaces**
1. Select your ground/floor objects
2. In Inspector, set them to Static → Navigation Static
3. Or just check "Navigation Static" checkbox

**Step 3: Configure Agent Settings**
1. In Navigation window, click "Agents" tab
2. Default agent is usually fine
3. Settings:
   - Agent Radius: 0.5 (zombie width / 2)
   - Agent Height: 2 (zombie height)
   - Max Slope: 45
   - Step Height: 0.4

**Step 4: Bake**
1. Click "Bake" tab
2. Click "Bake" button
3. Wait for process to complete
4. Blue overlay shows walkable areas

### Verify:
- Blue areas cover where zombies should walk
- No blue on walls, obstacles
- Stairs/ramps show connectivity

---

## 7.2 Zombie State Machine

### States:

```
┌──────────────────────────────────────────────────────────────┐
│                       ZOMBIE STATES                          │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│   ┌─────────┐         ┌─────────┐         ┌─────────┐       │
│   │  Spawn  │────────→│  Chase  │────────→│ Attack  │       │
│   │(climb in)│        │(pathfind)│←───────│(damage) │       │
│   └─────────┘         └────┬────┘         └─────────┘       │
│                            │                                 │
│                            │ Health <= 0                     │
│                            ↓                                 │
│                       ┌─────────┐                            │
│                       │  Dead   │                            │
│                       │(ragdoll)│                            │
│                       └─────────┘                            │
└──────────────────────────────────────────────────────────────┘
```

---

## 7.3 ZombieAI Script

`_Project/Scripts/Enemies/ZombieAI.cs`

```csharp
using UnityEngine;
using UnityEngine.AI;

/// <summary>
/// Controls zombie behavior, pathfinding, and attacks.
/// </summary>
[RequireComponent(typeof(NavMeshAgent))]
public class ZombieAI : MonoBehaviour, IDamageable
{
    #region Enums
    
    public enum ZombieState
    {
        Spawning,
        Idle,
        Chasing,
        Attacking,
        Dead
    }
    
    #endregion
    
    #region Serialized Fields
    
    [Header("Stats")]
    [SerializeField] private float maxHealth = 150f;
    [SerializeField] private float damage = 30f;
    [SerializeField] private float attackRange = 2f;
    [SerializeField] private float attackCooldown = 1.5f;
    
    [Header("Movement")]
    [SerializeField] private float walkSpeed = 1.5f;
    [SerializeField] private float runSpeed = 4f;
    [SerializeField] private float runDistanceThreshold = 15f;
    
    [Header("Points")]
    [SerializeField] private int killPoints = 50;
    [SerializeField] private int headshotBonusPoints = 50;
    
    [Header("Audio")]
    [SerializeField] private AudioClip[] idleSounds;
    [SerializeField] private AudioClip[] attackSounds;
    [SerializeField] private AudioClip[] deathSounds;
    [SerializeField] private AudioClip[] hitSounds;
    
    #endregion
    
    #region Private Fields
    
    // Components
    private NavMeshAgent agent;
    private Animator animator;
    private AudioSource audioSource;
    private Collider mainCollider;
    
    // State
    private ZombieState currentState = ZombieState.Spawning;
    private float currentHealth;
    private float lastAttackTime;
    
    // Target
    private Transform target;
    
    // Animator hashes
    private static readonly int SpeedHash = Animator.StringToHash("Speed");
    private static readonly int AttackHash = Animator.StringToHash("Attack");
    private static readonly int DieHash = Animator.StringToHash("Die");
    private static readonly int HitHash = Animator.StringToHash("Hit");
    
    #endregion
    
    #region Properties
    
    public ZombieState CurrentState => currentState;
    public float HealthPercent => currentHealth / maxHealth;
    
    #endregion
    
    #region Unity Callbacks
    
    private void Awake()
    {
        agent = GetComponent<NavMeshAgent>();
        animator = GetComponent<Animator>();
        audioSource = GetComponent<AudioSource>();
        mainCollider = GetComponent<Collider>();
        
        currentHealth = maxHealth;
    }
    
    private void Start()
    {
        // Find player
        GameObject playerObj = GameObject.FindGameObjectWithTag("Player");
        if (playerObj != null)
        {
            target = playerObj.transform;
        }
        
        // Start in chase state
        SetState(ZombieState.Chasing);
    }
    
    private void Update()
    {
        if (currentState == ZombieState.Dead) return;
        
        switch (currentState)
        {
            case ZombieState.Chasing:
                UpdateChasing();
                break;
            case ZombieState.Attacking:
                UpdateAttacking();
                break;
        }
        
        // Update animator
        if (animator != null)
        {
            animator.SetFloat(SpeedHash, agent.velocity.magnitude / runSpeed);
        }
    }
    
    #endregion
    
    #region State Updates
    
    private void UpdateChasing()
    {
        if (target == null) return;
        
        // Update destination
        agent.SetDestination(target.position);
        
        // Adjust speed based on distance
        float distance = Vector3.Distance(transform.position, target.position);
        agent.speed = distance > runDistanceThreshold ? runSpeed : walkSpeed;
        
        // Check if in attack range
        if (distance <= attackRange)
        {
            SetState(ZombieState.Attacking);
        }
        
        // Play occasional idle sounds
        if (Random.value < 0.001f && idleSounds.Length > 0)
        {
            PlaySound(idleSounds[Random.Range(0, idleSounds.Length)]);
        }
    }
    
    private void UpdateAttacking()
    {
        if (target == null)
        {
            SetState(ZombieState.Chasing);
            return;
        }
        
        // Stop moving
        agent.isStopped = true;
        
        // Face target
        Vector3 direction = (target.position - transform.position).normalized;
        direction.y = 0;
        if (direction != Vector3.zero)
        {
            transform.rotation = Quaternion.Slerp(
                transform.rotation,
                Quaternion.LookRotation(direction),
                Time.deltaTime * 10f
            );
        }
        
        // Check if still in range
        float distance = Vector3.Distance(transform.position, target.position);
        if (distance > attackRange * 1.5f)
        {
            agent.isStopped = false;
            SetState(ZombieState.Chasing);
            return;
        }
        
        // Try to attack
        if (Time.time - lastAttackTime >= attackCooldown)
        {
            PerformAttack();
        }
    }
    
    #endregion
    
    #region Actions
    
    private void PerformAttack()
    {
        lastAttackTime = Time.time;
        
        // Play animation
        if (animator != null)
        {
            animator.SetTrigger(AttackHash);
        }
        
        // Play sound
        if (attackSounds.Length > 0)
        {
            PlaySound(attackSounds[Random.Range(0, attackSounds.Length)]);
        }
        
        // Damage will be dealt via animation event (DealDamage)
    }
    
    /// <summary>
    /// Called by animation event when attack should hit.
    /// </summary>
    public void DealDamage()
    {
        if (target == null) return;
        if (currentState == ZombieState.Dead) return;
        
        float distance = Vector3.Distance(transform.position, target.position);
        if (distance <= attackRange)
        {
            // Get player health component
            if (target.TryGetComponent<PlayerHealth>(out var playerHealth))
            {
                playerHealth.TakeDamage(damage);
            }
        }
    }
    
    private void Die(bool headshot = false)
    {
        SetState(ZombieState.Dead);
        
        // Stop movement
        agent.isStopped = true;
        agent.enabled = false;
        
        // Disable collider
        if (mainCollider != null)
        {
            mainCollider.enabled = false;
        }
        
        // Play animation
        if (animator != null)
        {
            animator.SetTrigger(DieHash);
        }
        
        // Play sound
        if (deathSounds.Length > 0)
        {
            PlaySound(deathSounds[Random.Range(0, deathSounds.Length)]);
        }
        
        // Award points
        int points = killPoints + (headshot ? headshotBonusPoints : 0);
        if (PointsManager.Instance != null)
        {
            PointsManager.Instance.AddPoints(points);
        }
        
        // Notify round manager
        if (RoundManager.Instance != null)
        {
            RoundManager.Instance.OnZombieKilled();
        }
        
        // Destroy after delay
        Destroy(gameObject, 5f);
    }
    
    #endregion
    
    #region IDamageable
    
    public void TakeDamage(float damage, Vector3 hitPoint, Vector3 hitDirection)
    {
        if (currentState == ZombieState.Dead) return;
        
        currentHealth -= damage;
        
        // Play hit sound
        if (hitSounds.Length > 0)
        {
            PlaySound(hitSounds[Random.Range(0, hitSounds.Length)]);
        }
        
        // Hit animation
        if (animator != null)
        {
            animator.SetTrigger(HitHash);
        }
        
        // Check for death
        if (currentHealth <= 0)
        {
            // Check if headshot (you'd determine this from hit collider tag)
            Die(false);
        }
    }
    
    #endregion
    
    #region Helpers
    
    private void SetState(ZombieState newState)
    {
        currentState = newState;
    }
    
    private void PlaySound(AudioClip clip)
    {
        if (audioSource != null && clip != null)
        {
            audioSource.PlayOneShot(clip);
        }
    }
    
    #endregion
}
```

---

## 7.4 Zombie Animations

### Mixamo Zombie Animations:

Download these:

| Animation | Mixamo Search | Settings |
|-----------|---------------|----------|
| Zombie Walk | "Zombie Walking" | In Place ✓, Loop ✓ |
| Zombie Run | "Zombie Running" | In Place ✓, Loop ✓ |
| Zombie Attack 1 | "Zombie Attack" | In Place ✓ |
| Zombie Attack 2 | "Zombie Punching" | In Place ✓ |
| Zombie Death 1 | "Zombie Death" | In Place ✓ |
| Zombie Death 2 | "Dying" | In Place ✓ |
| Zombie Idle | "Zombie Idle" | Loop ✓ |
| Hit Reaction | "Hit Reaction" | In Place ✓ |

### Zombie Animator Controller:

Parameters:
- Speed (Float)
- Attack (Trigger)
- Die (Trigger)
- Hit (Trigger)

State Machine similar to player, but simpler.

---

## 7.5 Zombie Spawning

`_Project/Scripts/Systems/ZombieSpawner.cs`

```csharp
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

/// <summary>
/// Handles zombie spawn points and spawning logic.
/// </summary>
public class ZombieSpawner : MonoBehaviour
{
    [Header("Spawn Points")]
    [SerializeField] private Transform[] spawnPoints;
    
    [Header("Zombie Prefabs")]
    [SerializeField] private GameObject[] zombiePrefabs;
    
    [Header("Spawn Settings")]
    [SerializeField] private float initialSpawnDelay = 2f;
    [SerializeField] private float minSpawnInterval = 0.5f;
    [SerializeField] private float maxSpawnInterval = 3f;
    
    // Active zombies
    private List<GameObject> activeZombies = new List<GameObject>();
    
    /// <summary>
    /// Spawn a specific number of zombies.
    /// </summary>
    public IEnumerator SpawnZombies(int count, int round)
    {
        yield return new WaitForSeconds(initialSpawnDelay);
        
        for (int i = 0; i < count; i++)
        {
            SpawnZombie(round);
            
            // Variable spawn interval based on round
            float interval = Mathf.Lerp(maxSpawnInterval, minSpawnInterval, round / 20f);
            yield return new WaitForSeconds(interval);
        }
    }
    
    private void SpawnZombie(int round)
    {
        if (spawnPoints.Length == 0 || zombiePrefabs.Length == 0) return;
        
        // Random spawn point
        Transform spawnPoint = spawnPoints[Random.Range(0, spawnPoints.Length)];
        
        // Random zombie type
        GameObject prefab = zombiePrefabs[Random.Range(0, zombiePrefabs.Length)];
        
        // Spawn
        GameObject zombie = Instantiate(prefab, spawnPoint.position, spawnPoint.rotation);
        activeZombies.Add(zombie);
        
        // Scale stats with round
        if (zombie.TryGetComponent<ZombieAI>(out var ai))
        {
            // Health scales with round
            // (Done via the zombie prefab or additional configuration)
        }
    }
    
    /// <summary>
    /// Clean up dead zombie references.
    /// </summary>
    public void CleanupDeadZombies()
    {
        activeZombies.RemoveAll(z => z == null);
    }
}
```

---

# PART 8: GAME SYSTEMS

## 8.1 Health System

`_Project/Scripts/Player/PlayerHealth.cs`

```csharp
using UnityEngine;
using UnityEngine.Events;

/// <summary>
/// Manages player health and damage.
/// </summary>
public class PlayerHealth : MonoBehaviour
{
    [Header("Health Settings")]
    [SerializeField] private float maxHealth = 100f;
    [SerializeField] private float regenDelay = 5f;
    [SerializeField] private float regenRate = 10f;
    
    [Header("Events")]
    public UnityEvent<float> OnHealthChanged;
    public UnityEvent OnDamaged;
    public UnityEvent OnDeath;
    
    private float currentHealth;
    private float lastDamageTime;
    private bool isDead;
    
    #region Properties
    
    public float CurrentHealth => currentHealth;
    public float MaxHealth => maxHealth;
    public float HealthPercent => currentHealth / maxHealth;
    public bool IsDead => isDead;
    
    #endregion
    
    private void Start()
    {
        currentHealth = maxHealth;
    }
    
    private void Update()
    {
        // Health regeneration
        if (!isDead && currentHealth < maxHealth)
        {
            if (Time.time - lastDamageTime >= regenDelay)
            {
                currentHealth = Mathf.Min(currentHealth + regenRate * Time.deltaTime, maxHealth);
                OnHealthChanged?.Invoke(HealthPercent);
            }
        }
    }
    
    public void TakeDamage(float damage)
    {
        if (isDead) return;
        
        currentHealth -= damage;
        lastDamageTime = Time.time;
        
        OnDamaged?.Invoke();
        OnHealthChanged?.Invoke(HealthPercent);
        
        if (currentHealth <= 0)
        {
            Die();
        }
    }
    
    public void Heal(float amount)
    {
        if (isDead) return;
        
        currentHealth = Mathf.Min(currentHealth + amount, maxHealth);
        OnHealthChanged?.Invoke(HealthPercent);
    }
    
    public void SetMaxHealth(float newMax)
    {
        maxHealth = newMax;
        currentHealth = Mathf.Min(currentHealth, maxHealth);
    }
    
    private void Die()
    {
        isDead = true;
        OnDeath?.Invoke();
        
        // Game over logic
        if (GameManager.Instance != null)
        {
            GameManager.Instance.GameOver();
        }
    }
}
```

---

## 8.2 Points/Economy System

`_Project/Scripts/Systems/PointsManager.cs`

```csharp
using UnityEngine;
using UnityEngine.Events;

/// <summary>
/// Manages player points (currency).
/// </summary>
public class PointsManager : MonoBehaviour
{
    public static PointsManager Instance { get; private set; }
    
    [Header("Events")]
    public UnityEvent<int> OnPointsChanged;
    
    [Header("Point Values")]
    [SerializeField] private int hitPoints = 10;
    [SerializeField] private int killPoints = 50;
    [SerializeField] private int headshotBonus = 50;
    [SerializeField] private int meleeKillBonus = 80;
    
    public int Points { get; private set; }
    
    private void Awake()
    {
        if (Instance == null)
        {
            Instance = this;
        }
        else
        {
            Destroy(gameObject);
        }
    }
    
    public void AddPoints(int amount)
    {
        Points += amount;
        OnPointsChanged?.Invoke(Points);
    }
    
    public void AddHitPoints()
    {
        AddPoints(hitPoints);
    }
    
    public void AddKillPoints(bool headshot = false, bool melee = false)
    {
        int total = killPoints;
        if (headshot) total += headshotBonus;
        if (melee) total += meleeKillBonus;
        AddPoints(total);
    }
    
    public bool SpendPoints(int amount)
    {
        if (Points >= amount)
        {
            Points -= amount;
            OnPointsChanged?.Invoke(Points);
            return true;
        }
        return false;
    }
    
    public bool CanAfford(int amount)
    {
        return Points >= amount;
    }
}
```

---

## 8.3 Round Manager

`_Project/Scripts/Systems/RoundManager.cs`

```csharp
using UnityEngine;
using UnityEngine.Events;
using System.Collections;

/// <summary>
/// Manages round progression and zombie spawning.
/// </summary>
public class RoundManager : MonoBehaviour
{
    public static RoundManager Instance { get; private set; }
    
    [Header("Round Settings")]
    [SerializeField] private int startRound = 1;
    [SerializeField] private float timeBetweenRounds = 15f;
    
    [Header("Spawning")]
    [SerializeField] private ZombieSpawner zombieSpawner;
    
    [Header("Events")]
    public UnityEvent<int> OnRoundStart;
    public UnityEvent<int> OnRoundEnd;
    
    public int CurrentRound { get; private set; }
    public int ZombiesRemaining { get; private set; }
    public bool RoundInProgress { get; private set; }
    
    private int zombiesSpawnedThisRound;
    private int zombiesToSpawnThisRound;
    
    private void Awake()
    {
        if (Instance == null)
        {
            Instance = this;
        }
        else
        {
            Destroy(gameObject);
        }
    }
    
    private void Start()
    {
        CurrentRound = startRound - 1;
        StartCoroutine(StartNextRound());
    }
    
    private IEnumerator StartNextRound()
    {
        yield return new WaitForSeconds(timeBetweenRounds);
        
        CurrentRound++;
        zombiesToSpawnThisRound = CalculateZombiesForRound(CurrentRound);
        ZombiesRemaining = zombiesToSpawnThisRound;
        zombiesSpawnedThisRound = 0;
        RoundInProgress = true;
        
        OnRoundStart?.Invoke(CurrentRound);
        
        // Start spawning
        StartCoroutine(zombieSpawner.SpawnZombies(zombiesToSpawnThisRound, CurrentRound));
    }
    
    /// <summary>
    /// Calculate zombies for a given round (BO2-style formula).
    /// </summary>
    private int CalculateZombiesForRound(int round)
    {
        // Approximate BO2 formula
        if (round <= 1) return 6;
        return Mathf.RoundToInt(0.08f * round * round + 0.8f * round + 6);
    }
    
    /// <summary>
    /// Called when a zombie dies.
    /// </summary>
    public void OnZombieKilled()
    {
        ZombiesRemaining--;
        
        if (ZombiesRemaining <= 0 && RoundInProgress)
        {
            EndRound();
        }
    }
    
    private void EndRound()
    {
        RoundInProgress = false;
        OnRoundEnd?.Invoke(CurrentRound);
        StartCoroutine(StartNextRound());
    }
}
```

---

## 8.4 Perk System

`_Project/Scripts/ScriptableObjects/PerkData.cs`

```csharp
using UnityEngine;

/// <summary>
/// Data for a perk machine.
/// </summary>
[CreateAssetMenu(fileName = "NewPerk", menuName = "Game/Perk Data")]
public class PerkData : ScriptableObject
{
    public string perkName;
    public string description;
    public int cost;
    public Sprite icon;
    public Color color = Color.white;
    public AudioClip jingleSound;
    public AudioClip drinkSound;
}
```

`_Project/Scripts/Systems/PerkManager.cs`

```csharp
using UnityEngine;
using System.Collections.Generic;
using UnityEngine.Events;

/// <summary>
/// Manages player perks.
/// </summary>
public class PerkManager : MonoBehaviour
{
    public static PerkManager Instance { get; private set; }
    
    [Header("Settings")]
    [SerializeField] private int maxPerks = 4;
    
    [Header("Events")]
    public UnityEvent<PerkData> OnPerkPurchased;
    public UnityEvent<PerkData> OnPerkLost;
    
    private List<PerkData> activePerks = new List<PerkData>();
    
    // References to apply effects
    private PlayerHealth playerHealth;
    private PlayerController playerController;
    
    #region Perk Checks
    
    public bool HasPerk(string perkName) => 
        activePerks.Exists(p => p.perkName == perkName);
    
    public bool HasJuggernog => HasPerk("Juggernog");
    public bool HasSpeedCola => HasPerk("Speed Cola");
    public bool HasDoubleTap => HasPerk("Double Tap");
    public bool HasQuickRevive => HasPerk("Quick Revive");
    
    #endregion
    
    private void Awake()
    {
        if (Instance == null)
        {
            Instance = this;
        }
        else
        {
            Destroy(gameObject);
        }
    }
    
    private void Start()
    {
        playerHealth = GetComponent<PlayerHealth>();
        playerController = GetComponent<PlayerController>();
    }
    
    public bool TryPurchasePerk(PerkData perk)
    {
        // Check if already have
        if (HasPerk(perk.perkName)) return false;
        
        // Check if at max
        if (activePerks.Count >= maxPerks) return false;
        
        // Check if can afford
        if (!PointsManager.Instance.SpendPoints(perk.cost)) return false;
        
        // Add perk
        activePerks.Add(perk);
        ApplyPerkEffect(perk);
        OnPerkPurchased?.Invoke(perk);
        
        return true;
    }
    
    private void ApplyPerkEffect(PerkData perk)
    {
        switch (perk.perkName)
        {
            case "Juggernog":
                if (playerHealth != null)
                {
                    playerHealth.SetMaxHealth(250f);
                    playerHealth.Heal(250f);
                }
                break;
                
            case "Speed Cola":
                // Applied via WeaponManager checking HasSpeedCola
                break;
                
            case "Double Tap":
                // Applied via Weapon checking HasDoubleTap
                break;
                
            case "Quick Revive":
                // For solo: allows self-revive
                break;
        }
    }
    
    public void LoseAllPerks()
    {
        foreach (var perk in activePerks)
        {
            OnPerkLost?.Invoke(perk);
        }
        activePerks.Clear();
        
        // Reset health to normal
        if (playerHealth != null)
        {
            playerHealth.SetMaxHealth(100f);
        }
    }
}
```

---

## 8.5 Mystery Box

`_Project/Scripts/Interactables/MysteryBox.cs`

```csharp
using UnityEngine;
using System.Collections;

/// <summary>
/// Mystery box that gives random weapons.
/// </summary>
public class MysteryBox : MonoBehaviour, IInteractable
{
    [Header("Settings")]
    [SerializeField] private int useCost = 950;
    [SerializeField] private float rollTime = 4f;
    
    [Header("Weapons")]
    [SerializeField] private WeaponData[] possibleWeapons;
    
    [Header("References")]
    [SerializeField] private Transform weaponDisplayPoint;
    [SerializeField] private Transform lidTransform;
    [SerializeField] private AudioSource audioSource;
    [SerializeField] private AudioClip openSound;
    [SerializeField] private AudioClip rollSound;
    [SerializeField] private AudioClip closeSound;
    
    private bool isInUse;
    private WeaponData currentWeapon;
    
    public string GetInteractText()
    {
        if (isInUse) return "";
        return $"Hold [E] to use Mystery Box [{useCost}]";
    }
    
    public bool CanInteract()
    {
        return !isInUse && PointsManager.Instance.CanAfford(useCost);
    }
    
    public void Interact(GameObject interactor)
    {
        if (!CanInteract()) return;
        
        if (PointsManager.Instance.SpendPoints(useCost))
        {
            StartCoroutine(RollWeapon(interactor));
        }
    }
    
    private IEnumerator RollWeapon(GameObject player)
    {
        isInUse = true;
        
        // Open lid
        if (openSound != null) audioSource.PlayOneShot(openSound);
        // Animate lid opening...
        
        // Roll through weapons
        if (rollSound != null) audioSource.PlayOneShot(rollSound);
        
        float elapsed = 0;
        while (elapsed < rollTime)
        {
            // Show random weapon
            currentWeapon = possibleWeapons[Random.Range(0, possibleWeapons.Length)];
            // Update display...
            
            elapsed += 0.1f;
            yield return new WaitForSeconds(0.1f);
        }
        
        // Final weapon
        currentWeapon = possibleWeapons[Random.Range(0, possibleWeapons.Length)];
        
        // Wait for player to take it
        float takeTime = 10f;
        while (takeTime > 0)
        {
            // Check if player takes weapon
            // If player interacts again, give weapon
            takeTime -= Time.deltaTime;
            yield return null;
        }
        
        // Close if not taken
        if (closeSound != null) audioSource.PlayOneShot(closeSound);
        currentWeapon = null;
        isInUse = false;
    }
}
```

---

## 8.6 Purchasable Doors

`_Project/Scripts/Interactables/PurchasableDoor.cs`

```csharp
using UnityEngine;

/// <summary>
/// Door/barrier that can be purchased to open.
/// </summary>
public class PurchasableDoor : MonoBehaviour, IInteractable
{
    [Header("Settings")]
    [SerializeField] private int cost = 750;
    [SerializeField] private string areaName = "Area";
    
    [Header("Door")]
    [SerializeField] private GameObject doorObject;
    [SerializeField] private bool destroyOnOpen = true;
    
    [Header("Audio")]
    [SerializeField] private AudioClip openSound;
    
    private bool isOpen;
    
    public string GetInteractText()
    {
        if (isOpen) return "";
        return $"Hold [E] to open {areaName} [{cost}]";
    }
    
    public bool CanInteract()
    {
        return !isOpen && PointsManager.Instance.CanAfford(cost);
    }
    
    public void Interact(GameObject interactor)
    {
        if (!CanInteract()) return;
        
        if (PointsManager.Instance.SpendPoints(cost))
        {
            Open();
        }
    }
    
    private void Open()
    {
        isOpen = true;
        
        if (openSound != null)
        {
            AudioSource.PlayClipAtPoint(openSound, transform.position);
        }
        
        if (destroyOnOpen)
        {
            Destroy(doorObject);
        }
        else
        {
            doorObject.SetActive(false);
        }
    }
}
```

---

# Quick Reference: Interactable Interface

`_Project/Scripts/Interfaces/IInteractable.cs`

```csharp
using UnityEngine;

/// <summary>
/// Interface for interactable objects.
/// </summary>
public interface IInteractable
{
    string GetInteractText();
    bool CanInteract();
    void Interact(GameObject interactor);
}
```

---

*This guide continues with UI, Audio, Polish, and Building sections...*

*Document size limit reached. Additional sections available on request.*

---

# QUICK REFERENCE CARD

## Foot IK Summary:

```
You need 2 Two Bone IK Constraints:

LEFT LEG:
├── Root: mixamorig:LeftUpLeg
├── Mid: mixamorig:LeftLeg
├── Tip: mixamorig:LeftFoot
├── Target: LeftFootTarget
└── Hint: LeftFootHint (behind knee)

RIGHT LEG:
├── Root: mixamorig:RightUpLeg
├── Mid: mixamorig:RightLeg
├── Tip: mixamorig:RightFoot
├── Target: RightFootTarget
└── Hint: RightFootHint (behind knee)
```

## Animation Layer Order:
1. Base Layer (unarmed locomotion)
2. Upper Body (weapon holding - uses Avatar Mask)
3. Additive (reactions, breathing)

## Common Errors:

| Error | Solution |
|-------|----------|
| CS0029 float to Vector2 | Check SmoothDamp parameters |
| Null reference | Assign Inspector references |
| Animation not playing | Check Animator Controller assigned |
| IK not working | Check Rig Builder has Rig assigned |
| Feet floating | Decrease footOffset |
| Knees backward | Move Hint behind knee |

