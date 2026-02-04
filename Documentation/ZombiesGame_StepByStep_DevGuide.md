# 🎮 Zombies Game Development Guide
## Complete Step-by-Step with AI Assistance

> **How to Use This Guide:**
> - Follow each section in order
> - Use the provided "AI Prompt" boxes when you need help
> - Copy-paste code templates and modify as needed
> - Check off tasks as you complete them
> - Return to troubleshooting sections when stuck

---

# 📑 Table of Contents

1. [Project Setup](#module-1-project-setup)
2. [Player Controller](#module-2-player-controller)
3. [Animation System (DETAILED)](#module-3-animation-system)
4. [First-Person Arms & Weapons](#module-4-first-person-arms--weapons)
5. [Foot IK System](#module-5-foot-ik-system)
6. [Zombie AI](#module-6-zombie-ai)
7. [Zombie Animations](#module-7-zombie-animations)
8. [Weapons System](#module-8-weapons-system)
9. [Game Systems (Rounds, Points, Perks)](#module-9-game-systems)
10. [UI System](#module-10-ui-system)
11. [Audio](#module-11-audio)
12. [Polish & Effects](#module-12-polish--effects)
13. [Troubleshooting Guide](#troubleshooting-guide)
14. [AI Prompt Templates](#ai-prompt-templates)

---

# Module 1: Project Setup

## 1.1 Create Unity Project

### Steps:
- [ ] Open Unity Hub
- [ ] Click "New Project"
- [ ] Select **Unity 2022.3 LTS** (or Unity 6)
- [ ] Choose **3D (URP)** template
- [ ] Name: `ZombiesSurvival` (or your preferred name)
- [ ] Create Project

### Folder Structure to Create:
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
│   │   └── Weapons/
│   │       └── FirstPerson/
│   ├── Audio/
│   ├── Materials/
│   ├── Models/
│   ├── Prefabs/
│   ├── Scenes/
│   ├── ScriptableObjects/
│   ├── Scripts/
│   └── UI/
└── ThirdParty/
```

## 1.2 Install Required Packages

### Open Package Manager:
Window → Package Manager

### Install These Packages:
- [ ] **Animation Rigging** - Search: `com.unity.animation.rigging`
- [ ] **Cinemachine** - Search: `com.unity.cinemachine`
- [ ] **Input System** - Search: `com.unity.inputsystem`
- [ ] **AI Navigation** - Search: `com.unity.ai.navigation`
- [ ] **TextMeshPro** - Usually included
- [ ] **ProBuilder** (Optional) - Search: `com.unity.probuilder`

## 1.3 Input System Setup

### Create Input Actions:
1. Right-click in `_Project/` → Create → Input Actions
2. Name it `PlayerInputActions`
3. Double-click to open editor

### Actions to Create (Action Map: "Player"):

| Action Name | Type | Binding |
|-------------|------|---------|
| Move | Value (Vector2) | WASD |
| Look | Value (Vector2) | Mouse Delta |
| Jump | Button | Space |
| Sprint | Button | Left Shift |
| Fire | Button | Left Mouse |
| ADS | Button | Right Mouse |
| Reload | Button | R |
| Interact | Button | E |
| Melee | Button | V |

> **AI Prompt - Setup Help:**
> ```
> I am setting up a Unity [VERSION] project for a zombies game.
> I am having trouble with [DESCRIBE ISSUE].
> Can you help me configure this?
> ```

---

# Module 2: Player Controller

## 2.1 Create Player GameObject

### Hierarchy:
```
Player (Empty)
├── PlayerBody (Capsule)
├── CameraHolder (Empty)
│   └── Main Camera
└── GroundCheck (Empty, at feet)
```

## 2.2 Player Controller Script

Create: `_Project/Scripts/Player/PlayerController.cs`

```csharp
using UnityEngine;
using UnityEngine.InputSystem;

[RequireComponent(typeof(CharacterController))]
public class PlayerController : MonoBehaviour
{
    [Header("Movement")]
    [SerializeField] private float walkSpeed = 5f;
    [SerializeField] private float sprintSpeed = 8f;
    [SerializeField] private float jumpForce = 7f;
    [SerializeField] private float gravity = -20f;
    
    [Header("Look")]
    [SerializeField] private float mouseSensitivity = 2f;
    [SerializeField] private float maxLookAngle = 85f;
    [SerializeField] private Transform cameraHolder;
    
    [Header("Ground Check")]
    [SerializeField] private Transform groundCheck;
    [SerializeField] private float groundDistance = 0.3f;
    [SerializeField] private LayerMask groundMask;
    
    private CharacterController controller;
    private PlayerInputActions inputActions;
    
    private Vector3 velocity;
    private float xRotation;
    private bool isGrounded;
    private bool isSprinting;
    
    private Vector2 moveInput;
    private Vector2 lookInput;
    
    private void Awake()
    {
        controller = GetComponent<CharacterController>();
        inputActions = new PlayerInputActions();
        Cursor.lockState = CursorLockMode.Locked;
    }
    
    private void OnEnable()
    {
        inputActions.Player.Enable();
        inputActions.Player.Jump.performed += OnJump;
        inputActions.Player.Sprint.performed += ctx => isSprinting = true;
        inputActions.Player.Sprint.canceled += ctx => isSprinting = false;
    }
    
    private void OnDisable()
    {
        inputActions.Player.Jump.performed -= OnJump;
        inputActions.Player.Disable();
    }
    
    private void Update()
    {
        moveInput = inputActions.Player.Move.ReadValue<Vector2>();
        lookInput = inputActions.Player.Look.ReadValue<Vector2>();
        
        isGrounded = Physics.CheckSphere(groundCheck.position, groundDistance, groundMask);
        
        if (isGrounded && velocity.y < 0)
            velocity.y = -2f;
        
        HandleMovement();
        HandleLook();
        ApplyGravity();
    }
    
    private void HandleMovement()
    {
        Vector3 move = transform.right * moveInput.x + transform.forward * moveInput.y;
        float speed = isSprinting ? sprintSpeed : walkSpeed;
        controller.Move(move * speed * Time.deltaTime);
    }
    
    private void HandleLook()
    {
        float mouseX = lookInput.x * mouseSensitivity;
        transform.Rotate(Vector3.up * mouseX);
        
        float mouseY = lookInput.y * mouseSensitivity;
        xRotation -= mouseY;
        xRotation = Mathf.Clamp(xRotation, -maxLookAngle, maxLookAngle);
        cameraHolder.localRotation = Quaternion.Euler(xRotation, 0f, 0f);
    }
    
    private void ApplyGravity()
    {
        velocity.y += gravity * Time.deltaTime;
        controller.Move(velocity * Time.deltaTime);
    }
    
    private void OnJump(InputAction.CallbackContext context)
    {
        if (isGrounded)
            velocity.y = Mathf.Sqrt(jumpForce * -2f * gravity);
    }
    
    // Getters for other systems
    public Vector2 GetMoveInput() => moveInput;
    public bool IsGrounded() => isGrounded;
    public bool IsSprinting() => isSprinting;
    public float GetVerticalVelocity() => velocity.y;
    public float GetCurrentSpeed() => new Vector3(controller.velocity.x, 0, controller.velocity.z).magnitude;
}
```

## 2.3 Setup Checklist

- [ ] Add CharacterController to Player (Height: 2, Radius: 0.5)
- [ ] Add PlayerController script
- [ ] Assign Camera Holder reference
- [ ] Assign Ground Check reference
- [ ] Create "Ground" layer, assign to floor
- [ ] Set Ground Mask in inspector

---

# Module 3: Animation System (DETAILED)

## 3.1 Getting Animations from Mixamo

### Step-by-Step:

1. Go to https://www.mixamo.com/
2. Sign in (free Adobe account)
3. Upload character OR choose one

### 🎯 IMPORTANT: Rifle vs Unarmed Animations

**Short Answer: Download UNARMED animations for the third-person body.**

Here's why:

| Approach | Pros | Cons | Recommendation |
|----------|------|------|----------------|
| **Unarmed Base** | Works with ANY weapon, flexible, can add rifle layer later | Need upper body override layer | ✅ **RECOMMENDED** |
| **Rifle Holding** | Looks correct immediately | Locked to rifle pose, can't easily switch to pistol/melee/unarmed | ❌ Limiting |

### How This Works in FPS Games:

```
┌─────────────────────────────────────────────────────────────┐
│                    ANIMATION LAYERS                         │
├─────────────────────────────────────────────────────────────┤
│ Layer 3: Action Override (Reload, Melee)    ← Highest      │
│ Layer 2: Upper Body Aim/Hold Pose           ← Weapon layer │
│ Layer 1: Base Locomotion (UNARMED)          ← Your legs    │
└─────────────────────────────────────────────────────────────┘
```

- **Base layer (unarmed):** Controls legs and overall body movement
- **Upper body layer:** Overrides torso/arms with weapon-holding pose
- **This lets you:** Switch weapons, go unarmed, melee, all with same leg animations!

### What To Download for Third-Person Body:

**UNARMED locomotion animations:**

| Animation | Mixamo Search | Settings |
|-----------|---------------|----------|
| Idle | "Breathing Idle" | Loop ✓ |
| Walk Forward | "Walking" | In Place ✓, Loop ✓ |
| Walk Backward | "Walking Backwards" | In Place ✓, Loop ✓ |
| Walk Left | "Left Strafe Walking" | In Place ✓, Loop ✓ |
| Walk Right | "Right Strafe Walking" | In Place ✓, Loop ✓ |
| Run Forward | "Running" | In Place ✓, Loop ✓ |
| Sprint | "Sprint Forward" | In Place ✓, Loop ✓ |
| Jump | "Jump" | In Place ✓ |
| Falling | "Falling Idle" | In Place ✓, Loop ✓ |
| Landing | "Landing" | In Place ✓ |

### What To Download for Weapon Holding (Upper Body Only):

| Animation | Mixamo Search | Use For |
|-----------|---------------|---------|
| Rifle Idle | "Rifle Aiming Idle" | Upper body layer when holding rifle |
| Rifle Walk | "Rifle Walk" | Optional: blend with locomotion |
| Pistol Idle | "Pistol Idle" | When holding pistol |

> **💡 Pro Tip:** You only need 1-2 rifle/pistol poses for the upper body layer. The unarmed legs do all the heavy lifting!

### Download Settings:
- Format: **FBX for Unity**
- Skin: **Without Skin** (for animations only)
- FPS: **30**
- Keyframe Reduction: **none**

**⚠️ "In Place" is important - it keeps character stationary so your code controls movement!**

### First-Person Arms (Separate from Third-Person):

For your first-person view, you WILL want rifle-holding animations, but these are for the **first-person arm rig** (Module 4), not the third-person body. These are typically:
- Separate arm-only models
- Different animations than third-person
- Bought from Asset Store or custom made

> **AI Prompt - If confused about layers:**
> ```
> I'm setting up animation layers in Unity for an FPS game.
> I want unarmed locomotion on the base layer and rifle holding
> on the upper body layer. Can you explain how to set up the 
> Avatar Mask and layer blending for this?
> ```

## 3.2 Import to Unity

### First FBX (with character):
1. Drag to `_Project/Animations/Player/Locomotion/`
2. Select → Inspector → Rig tab
3. Animation Type: **Humanoid**
4. Avatar Definition: **Create From This Model**
5. Click Apply

### Other Animation FBXs:
1. Animation Type: **Humanoid**
2. Avatar Definition: **Copy From Other Avatar**
3. Source: Select first character's Avatar
4. Apply

### Extract Animations:
1. Select FBX
2. Animation tab → select clip
3. Set "Loop Time" if looping
4. Apply
5. Expand FBX, select clip, Ctrl+D to extract

## 3.3 Create Animator Controller

### Create:
Right-click → Create → Animator Controller → Name: `PlayerAnimator`

### Add Parameters:

| Parameter | Type |
|-----------|------|
| Speed | Float |
| VelocityX | Float |
| VelocityZ | Float |
| IsGrounded | Bool |
| IsSprinting | Bool |
| Jump | Trigger |
| VerticalVelocity | Float |

## 3.4 Create Locomotion Blend Tree

### Steps:
1. Open Animator window (double-click controller)
2. Right-click → Create State → From New Blend Tree
3. Name: "Locomotion"
4. Double-click to edit blend tree
5. Blend Type: **2D Freeform Directional**
6. Parameters: VelocityX, VelocityZ

### Add Motions:

| Motion | Pos X | Pos Y |
|--------|-------|-------|
| Idle | 0 | 0 |
| Walk Forward | 0 | 0.5 |
| Walk Backward | 0 | -0.5 |
| Walk Left | -0.5 | 0 |
| Walk Right | 0.5 | 0 |
| Run Forward | 0 | 1 |
| Run Backward | 0 | -1 |
| Run Left | -1 | 0 |
| Run Right | 1 | 0 |

## 3.5 Jump States

### Create States:
- JumpStart
- Falling
- Landing

### Transitions:
```
Locomotion → JumpStart: Jump trigger, No Exit Time
JumpStart → Falling: Exit Time 0.9
Falling → Landing: IsGrounded = true
Landing → Locomotion: Exit Time 0.9
Locomotion → Falling: IsGrounded = false, VerticalVelocity < -1
```

## 3.6 Animation Script

Create: `_Project/Scripts/Player/PlayerAnimator.cs`

```csharp
using UnityEngine;

public class PlayerAnimator : MonoBehaviour
{
    [SerializeField] private PlayerController playerController;
    [SerializeField] private float smoothing = 0.1f;
    
    private Animator animator;
    private Vector2 currentVelocity;
    private Vector2 smoothVelocity;
    
    // Parameter hashes
    private static readonly int VelocityX = Animator.StringToHash("VelocityX");
    private static readonly int VelocityZ = Animator.StringToHash("VelocityZ");
    private static readonly int Speed = Animator.StringToHash("Speed");
    private static readonly int IsGrounded = Animator.StringToHash("IsGrounded");
    private static readonly int IsSprinting = Animator.StringToHash("IsSprinting");
    private static readonly int Jump = Animator.StringToHash("Jump");
    private static readonly int VerticalVelocity = Animator.StringToHash("VerticalVelocity");
    
    private void Awake()
    {
        animator = GetComponent<Animator>();
    }
    
    private void Update()
    {
        Vector2 input = playerController.GetMoveInput();
        float multiplier = playerController.IsSprinting() ? 1f : 0.5f;
        Vector2 target = input * multiplier;
        
        currentVelocity = Vector2.SmoothDamp(currentVelocity, target, ref smoothVelocity, smoothing);
        
        animator.SetFloat(VelocityX, currentVelocity.x);
        animator.SetFloat(VelocityZ, currentVelocity.y);
        animator.SetFloat(Speed, currentVelocity.magnitude);
        animator.SetBool(IsGrounded, playerController.IsGrounded());
        animator.SetBool(IsSprinting, playerController.IsSprinting());
        animator.SetFloat(VerticalVelocity, playerController.GetVerticalVelocity());
    }
    
    public void TriggerJump()
    {
        animator.SetTrigger(Jump);
    }
}
```

> **AI Prompt - Animation Problems:**
> ```
> My Unity character animation has this issue: [DESCRIBE]
> 
> Setup:
> - Using [Blend Tree Type]
> - Parameters: [List them]
> - Values being sent: [Current values]
> 
> What should I check or adjust?
> ```

---

# Module 4: First-Person Arms

## 4.1 Hierarchy Setup

```
Player
├── CameraHolder
│   └── Main Camera
│       └── FPSArms (your arm model)
│           └── WeaponHolder (empty)
└── ...
```

## 4.2 Weapon Sway Script

```csharp
using UnityEngine;

public class WeaponSway : MonoBehaviour
{
    [SerializeField] private float swayAmount = 0.02f;
    [SerializeField] private float maxSway = 0.06f;
    [SerializeField] private float smoothing = 6f;
    
    private Vector3 initialPosition;
    
    private void Start()
    {
        initialPosition = transform.localPosition;
    }
    
    private void Update()
    {
        float mouseX = Input.GetAxis("Mouse X");
        float mouseY = Input.GetAxis("Mouse Y");
        
        float targetX = Mathf.Clamp(-mouseX * swayAmount, -maxSway, maxSway);
        float targetY = Mathf.Clamp(-mouseY * swayAmount, -maxSway, maxSway);
        
        Vector3 target = new Vector3(targetX, targetY, 0) + initialPosition;
        transform.localPosition = Vector3.Lerp(transform.localPosition, target, smoothing * Time.deltaTime);
    }
}
```

## 4.3 Weapon Bob Script

```csharp
using UnityEngine;

public class WeaponBob : MonoBehaviour
{
    [SerializeField] private float walkBobSpeed = 10f;
    [SerializeField] private float walkBobAmount = 0.01f;
    [SerializeField] private float sprintMultiplier = 1.4f;
    [SerializeField] private PlayerController player;
    
    private float defaultY;
    private float timer;
    
    private void Start()
    {
        defaultY = transform.localPosition.y;
    }
    
    private void Update()
    {
        if (!player.IsGrounded() || player.GetCurrentSpeed() < 0.1f)
        {
            timer = 0;
            return;
        }
        
        float speed = player.IsSprinting() ? walkBobSpeed * sprintMultiplier : walkBobSpeed;
        float amount = player.IsSprinting() ? walkBobAmount * sprintMultiplier : walkBobAmount;
        
        timer += Time.deltaTime * speed;
        float newY = defaultY + Mathf.Sin(timer) * amount;
        
        Vector3 pos = transform.localPosition;
        pos.y = newY;
        transform.localPosition = pos;
    }
}
```

---

# Module 5: Foot IK

## 5.1 Setup with Animation Rigging

### Components:
1. Add **Rig Builder** to character
2. Create child "FootIKRig" with **Rig** component
3. Create IK targets for each foot

### Hierarchy:
```
Character
├── (bones...)
└── FootIKRig
    ├── LeftFootTarget
    ├── LeftFootHint
    ├── RightFootTarget
    └── RightFootHint
```

### Add Two Bone IK Constraints:
- Root: Upper leg
- Mid: Lower leg
- Tip: Foot
- Target: FootTarget
- Hint: FootHint (behind knee)

## 5.2 Foot IK Script

```csharp
using UnityEngine;
using UnityEngine.Animations.Rigging;

public class FootIK : MonoBehaviour
{
    [SerializeField] private Transform leftFootTarget;
    [SerializeField] private Transform rightFootTarget;
    [SerializeField] private TwoBoneIKConstraint leftIK;
    [SerializeField] private TwoBoneIKConstraint rightIK;
    [SerializeField] private Transform leftFoot;
    [SerializeField] private Transform rightFoot;
    
    [SerializeField] private LayerMask groundMask;
    [SerializeField] private float rayDistance = 1.5f;
    [SerializeField] private float footOffset = 0.1f;
    [SerializeField] private float ikSpeed = 15f;
    
    [SerializeField] private PlayerController player;
    
    private Vector3 leftPos, rightPos;
    private Quaternion leftRot, rightRot;
    
    private void LateUpdate()
    {
        bool useIK = player.IsGrounded() && player.GetCurrentSpeed() < 3f;
        float weight = useIK ? 1f : 0f;
        
        leftIK.weight = Mathf.Lerp(leftIK.weight, weight, Time.deltaTime * 10f);
        rightIK.weight = Mathf.Lerp(rightIK.weight, weight, Time.deltaTime * 10f);
        
        if (useIK)
        {
            UpdateFoot(leftFoot, ref leftPos, ref leftRot, leftFootTarget);
            UpdateFoot(rightFoot, ref rightPos, ref rightRot, rightFootTarget);
        }
    }
    
    private void UpdateFoot(Transform foot, ref Vector3 pos, ref Quaternion rot, Transform target)
    {
        Vector3 rayStart = foot.position + Vector3.up * 0.5f;
        if (Physics.Raycast(rayStart, Vector3.down, out RaycastHit hit, rayDistance, groundMask))
        {
            Vector3 targetPos = hit.point + Vector3.up * footOffset;
            pos = Vector3.Lerp(pos, targetPos, Time.deltaTime * ikSpeed);
            
            Quaternion targetRot = Quaternion.FromToRotation(Vector3.up, hit.normal) * transform.rotation;
            rot = Quaternion.Slerp(rot, targetRot, Time.deltaTime * ikSpeed);
        }
        
        target.position = pos;
        target.rotation = rot;
    }
}
```

---

# Module 6: Zombie AI

## 6.1 NavMesh Setup

1. Window → AI → Navigation
2. Select floor objects
3. Mark as "Navigation Static"
4. Bake NavMesh

## 6.2 Zombie Script

```csharp
using UnityEngine;
using UnityEngine.AI;

public class ZombieAI : MonoBehaviour
{
    public enum State { Idle, Chasing, Attacking, Dead }
    
    [Header("Stats")]
    [SerializeField] private float maxHealth = 100f;
    [SerializeField] private float damage = 20f;
    [SerializeField] private float attackRange = 2f;
    [SerializeField] private float attackCooldown = 1.5f;
    
    [Header("Movement")]
    [SerializeField] private float walkSpeed = 1.5f;
    [SerializeField] private float runSpeed = 4f;
    [SerializeField] private float runDistance = 10f;
    
    private NavMeshAgent agent;
    private Animator animator;
    private Transform player;
    private State currentState;
    private float currentHealth;
    private float lastAttackTime;
    
    // Animator hashes
    private static readonly int Speed = Animator.StringToHash("Speed");
    private static readonly int Attack = Animator.StringToHash("Attack");
    private static readonly int Die = Animator.StringToHash("Die");
    
    private void Awake()
    {
        agent = GetComponent<NavMeshAgent>();
        animator = GetComponent<Animator>();
        currentHealth = maxHealth;
    }
    
    private void Start()
    {
        player = GameObject.FindGameObjectWithTag("Player").transform;
        currentState = State.Chasing;
    }
    
    private void Update()
    {
        if (currentState == State.Dead) return;
        
        float distance = Vector3.Distance(transform.position, player.position);
        
        switch (currentState)
        {
            case State.Chasing:
                ChasePlayer(distance);
                break;
            case State.Attacking:
                AttackPlayer(distance);
                break;
        }
        
        animator.SetFloat(Speed, agent.velocity.magnitude / runSpeed);
    }
    
    private void ChasePlayer(float distance)
    {
        agent.SetDestination(player.position);
        agent.speed = distance > runDistance ? runSpeed : walkSpeed;
        
        if (distance <= attackRange)
        {
            currentState = State.Attacking;
            agent.isStopped = true;
        }
    }
    
    private void AttackPlayer(float distance)
    {
        // Face player
        Vector3 direction = (player.position - transform.position).normalized;
        direction.y = 0;
        transform.rotation = Quaternion.LookRotation(direction);
        
        if (distance > attackRange * 1.5f)
        {
            currentState = State.Chasing;
            agent.isStopped = false;
            return;
        }
        
        if (Time.time - lastAttackTime >= attackCooldown)
        {
            lastAttackTime = Time.time;
            animator.SetTrigger(Attack);
            // Deal damage in animation event
        }
    }
    
    public void TakeDamage(float amount)
    {
        currentHealth -= amount;
        if (currentHealth <= 0)
        {
            Die();
        }
    }
    
    private void Die()
    {
        currentState = State.Dead;
        agent.isStopped = true;
        animator.SetTrigger(Die);
        GetComponent<Collider>().enabled = false;
        Destroy(gameObject, 5f);
    }
    
    // Called by animation event
    public void DealDamage()
    {
        if (Vector3.Distance(transform.position, player.position) <= attackRange)
        {
            player.GetComponent<PlayerHealth>()?.TakeDamage(damage);
        }
    }
}
```

---

# Module 7: Zombie Animations

## 7.1 Mixamo Zombie Animations

Download these:

| Animation | Search Term | Settings |
|-----------|-------------|----------|
| Walk | "Zombie Walking" | In Place ✓, Loop ✓ |
| Run | "Zombie Running" | In Place ✓, Loop ✓ |
| Attack 1 | "Zombie Attack" | In Place ✓ |
| Attack 2 | "Zombie Punching" | In Place ✓ |
| Hit React | "Hit Reaction" | - |
| Death 1 | "Zombie Death" | - |
| Death 2 | "Dying" | - |
| Idle | "Zombie Idle" | Loop ✓ |

## 7.2 Zombie Animator

### Parameters:
- Speed (Float)
- Attack (Trigger)
- Die (Trigger)
- HitReact (Trigger)

### Structure:
```
Locomotion (Blend Tree: Speed 0-1)
├── Zombie Idle (0)
├── Zombie Walk (0.5)
└── Zombie Run (1)

Attack states (triggered)
Death states (triggered, no exit)
```

---

# Module 8: Weapons System

## 8.1 Weapon Data ScriptableObject

```csharp
using UnityEngine;

[CreateAssetMenu(fileName = "NewWeapon", menuName = "Game/Weapon Data")]
public class WeaponData : ScriptableObject
{
    [Header("Info")]
    public string weaponName;
    public Sprite icon;
    
    [Header("Stats")]
    public float damage = 30f;
    public float fireRate = 0.1f;
    public float range = 100f;
    public int magazineSize = 30;
    public int reserveAmmo = 120;
    public float reloadTime = 2f;
    
    [Header("Recoil")]
    public float recoilX = 1f;
    public float recoilY = 2f;
    public float recoilRecovery = 5f;
    
    [Header("Audio")]
    public AudioClip fireSound;
    public AudioClip reloadSound;
    public AudioClip emptySound;
    
    [Header("Visual")]
    public GameObject prefab;
    public ParticleSystem muzzleFlash;
}
```

## 8.2 Weapon Script

```csharp
using UnityEngine;
using System.Collections;

public class Weapon : MonoBehaviour
{
    [SerializeField] private WeaponData data;
    [SerializeField] private Transform firePoint;
    [SerializeField] private ParticleSystem muzzleFlash;
    
    private int currentAmmo;
    private int currentReserve;
    private float nextFireTime;
    private bool isReloading;
    
    private AudioSource audioSource;
    
    private void Start()
    {
        currentAmmo = data.magazineSize;
        currentReserve = data.reserveAmmo;
        audioSource = GetComponent<AudioSource>();
    }
    
    public void TryFire()
    {
        if (isReloading || Time.time < nextFireTime) return;
        
        if (currentAmmo <= 0)
        {
            audioSource.PlayOneShot(data.emptySound);
            if (currentReserve > 0) StartCoroutine(Reload());
            return;
        }
        
        Fire();
    }
    
    private void Fire()
    {
        currentAmmo--;
        nextFireTime = Time.time + data.fireRate;
        
        // Effects
        if (muzzleFlash) muzzleFlash.Play();
        audioSource.PlayOneShot(data.fireSound);
        
        // Raycast
        if (Physics.Raycast(firePoint.position, firePoint.forward, out RaycastHit hit, data.range))
        {
            if (hit.collider.TryGetComponent<ZombieAI>(out var zombie))
            {
                zombie.TakeDamage(data.damage);
            }
        }
    }
    
    public void TryReload()
    {
        if (!isReloading && currentAmmo < data.magazineSize && currentReserve > 0)
        {
            StartCoroutine(Reload());
        }
    }
    
    private IEnumerator Reload()
    {
        isReloading = true;
        audioSource.PlayOneShot(data.reloadSound);
        
        yield return new WaitForSeconds(data.reloadTime);
        
        int needed = data.magazineSize - currentAmmo;
        int toReload = Mathf.Min(needed, currentReserve);
        
        currentAmmo += toReload;
        currentReserve -= toReload;
        isReloading = false;
    }
    
    public int GetCurrentAmmo() => currentAmmo;
    public int GetReserveAmmo() => currentReserve;
    public bool IsReloading() => isReloading;
}
```

---

# Module 9: Game Systems

## 9.1 Round Manager

```csharp
using UnityEngine;
using UnityEngine.Events;
using System.Collections;

public class RoundManager : MonoBehaviour
{
    public static RoundManager Instance { get; private set; }
    
    [Header("Settings")]
    [SerializeField] private int startingRound = 1;
    [SerializeField] private float timeBetweenRounds = 10f;
    [SerializeField] private GameObject zombiePrefab;
    [SerializeField] private Transform[] spawnPoints;
    
    [Header("Events")]
    public UnityEvent<int> OnRoundStart;
    public UnityEvent<int> OnRoundEnd;
    
    public int CurrentRound { get; private set; }
    public int ZombiesRemaining { get; private set; }
    
    private int zombiesSpawned;
    private int zombiesForRound;
    
    private void Awake()
    {
        Instance = this;
    }
    
    private void Start()
    {
        CurrentRound = startingRound - 1;
        StartCoroutine(StartNextRound());
    }
    
    private IEnumerator StartNextRound()
    {
        yield return new WaitForSeconds(timeBetweenRounds);
        
        CurrentRound++;
        zombiesForRound = CalculateZombiesForRound(CurrentRound);
        ZombiesRemaining = zombiesForRound;
        zombiesSpawned = 0;
        
        OnRoundStart?.Invoke(CurrentRound);
        
        StartCoroutine(SpawnZombies());
    }
    
    private int CalculateZombiesForRound(int round)
    {
        // BO2-style formula
        return Mathf.RoundToInt(0.08f * round * round + 0.8f * round + 6);
    }
    
    private IEnumerator SpawnZombies()
    {
        while (zombiesSpawned < zombiesForRound)
        {
            SpawnZombie();
            zombiesSpawned++;
            yield return new WaitForSeconds(2f / Mathf.Sqrt(CurrentRound));
        }
    }
    
    private void SpawnZombie()
    {
        Transform spawn = spawnPoints[Random.Range(0, spawnPoints.Length)];
        Instantiate(zombiePrefab, spawn.position, spawn.rotation);
    }
    
    public void OnZombieKilled()
    {
        ZombiesRemaining--;
        if (ZombiesRemaining <= 0 && zombiesSpawned >= zombiesForRound)
        {
            OnRoundEnd?.Invoke(CurrentRound);
            StartCoroutine(StartNextRound());
        }
    }
}
```

## 9.2 Points System

```csharp
using UnityEngine;
using UnityEngine.Events;

public class PointsManager : MonoBehaviour
{
    public static PointsManager Instance { get; private set; }
    
    public int Points { get; private set; }
    public UnityEvent<int> OnPointsChanged;
    
    // Point values
    private const int KILL_POINTS = 50;
    private const int HEADSHOT_BONUS = 50;
    private const int MELEE_BONUS = 80;
    
    private void Awake()
    {
        Instance = this;
    }
    
    public void AddKillPoints(bool headshot = false, bool melee = false)
    {
        int points = KILL_POINTS;
        if (headshot) points += HEADSHOT_BONUS;
        if (melee) points += MELEE_BONUS;
        AddPoints(points);
    }
    
    public void AddPoints(int amount)
    {
        Points += amount;
        OnPointsChanged?.Invoke(Points);
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
}
```

## 9.3 Perk System

```csharp
using UnityEngine;
using System.Collections.Generic;

[CreateAssetMenu(fileName = "NewPerk", menuName = "Game/Perk")]
public class PerkData : ScriptableObject
{
    public string perkName;
    public int cost;
    public Sprite icon;
    public Color color;
    [TextArea] public string description;
}

public class PerkManager : MonoBehaviour
{
    public static PerkManager Instance { get; private set; }
    
    [SerializeField] private PlayerController player;
    
    private List<PerkData> activatedPerks = new List<PerkData>();
    
    // Perk identifiers
    public bool HasJuggernog => HasPerk("Juggernog");
    public bool HasSpeedCola => HasPerk("Speed Cola");
    public bool HasDoubleTap => HasPerk("Double Tap");
    public bool HasQuickRevive => HasPerk("Quick Revive");
    
    private void Awake()
    {
        Instance = this;
    }
    
    public bool TryPurchasePerk(PerkData perk)
    {
        if (activatedPerks.Contains(perk)) return false;
        
        if (PointsManager.Instance.SpendPoints(perk.cost))
        {
            activatedPerks.Add(perk);
            ApplyPerkEffects(perk);
            return true;
        }
        return false;
    }
    
    private void ApplyPerkEffects(PerkData perk)
    {
        switch (perk.perkName)
        {
            case "Juggernog":
                // Increase max health
                break;
            case "Speed Cola":
                // Decrease reload time
                break;
            case "Double Tap":
                // Increase fire rate
                break;
        }
    }
    
    private bool HasPerk(string name)
    {
        return activatedPerks.Exists(p => p.perkName == name);
    }
}
```

---

# Module 10: UI System

## 10.1 HUD Script

```csharp
using UnityEngine;
using TMPro;
using UnityEngine.UI;

public class GameHUD : MonoBehaviour
{
    [Header("Health")]
    [SerializeField] private Slider healthBar;
    [SerializeField] private Image healthFill;
    [SerializeField] private Gradient healthGradient;
    
    [Header("Ammo")]
    [SerializeField] private TextMeshProUGUI ammoText;
    
    [Header("Points")]
    [SerializeField] private TextMeshProUGUI pointsText;
    
    [Header("Round")]
    [SerializeField] private TextMeshProUGUI roundText;
    
    [Header("Perks")]
    [SerializeField] private Transform perkContainer;
    [SerializeField] private GameObject perkIconPrefab;
    
    private PlayerHealth playerHealth;
    private Weapon currentWeapon;
    
    private void Start()
    {
        playerHealth = FindObjectOfType<PlayerHealth>();
        
        RoundManager.Instance.OnRoundStart.AddListener(UpdateRound);
        PointsManager.Instance.OnPointsChanged.AddListener(UpdatePoints);
    }
    
    private void Update()
    {
        UpdateHealth();
        UpdateAmmo();
    }
    
    private void UpdateHealth()
    {
        if (playerHealth == null) return;
        
        float healthPercent = playerHealth.GetHealthPercent();
        healthBar.value = healthPercent;
        healthFill.color = healthGradient.Evaluate(healthPercent);
    }
    
    private void UpdateAmmo()
    {
        if (currentWeapon == null) return;
        
        ammoText.text = $"{currentWeapon.GetCurrentAmmo()} / {currentWeapon.GetReserveAmmo()}";
    }
    
    private void UpdatePoints(int points)
    {
        pointsText.text = points.ToString();
    }
    
    private void UpdateRound(int round)
    {
        roundText.text = round.ToString();
    }
}
```

---

# Troubleshooting Guide

## Animation Issues

### Feet Sliding
**Cause:** Animation movement doesn't match code movement
**Fix:**
1. Ensure "In Place" is checked when downloading from Mixamo
2. In Unity, check "Root Transform Position (XZ)" is set to "Original"

### Snapping Between Animations
**Cause:** Transition settings too fast
**Fix:**
1. Increase Transition Duration (try 0.2-0.3)
2. Enable "Has Exit Time" where appropriate

### Wrong Animation Playing
**Cause:** Parameter values not matching blend tree positions
**Fix:**
1. Add Debug.Log to print parameter values
2. Check blend tree positions match expected input range

## Movement Issues

### Player Slides After Stopping
**Fix:** Increase friction or add dampening:
```csharp
moveDirection = Vector3.Lerp(moveDirection, Vector3.zero, Time.deltaTime * 10f);
```

### Jittery Camera
**Fix:** Move camera code to LateUpdate or use Cinemachine

### Falling Through Floor
**Fix:**
1. Check Ground layer is assigned
2. Increase ground check radius
3. Check CharacterController step offset

---

# AI Prompt Templates

Copy these when you need help:

## General Problem
```
I am making a Unity zombies game and have this issue:
[DESCRIBE THE PROBLEM]

What I expected: [EXPECTED BEHAVIOR]
What happens: [ACTUAL BEHAVIOR]

Relevant code:
[PASTE CODE]

Unity version: [VERSION]
```

## Animation Help
```
I need help with my Unity animation:

Problem: [DESCRIBE]
Character rig: Humanoid/Generic
Controller type: Animator Controller
Blend tree type: [IF APPLICABLE]
Current parameter values: [LIST]

What should I adjust?
```

## Bug Fix
```
I have a bug in my Unity game:

Error message (if any): [PASTE]
When it happens: [DESCRIBE TRIGGER]
What I've tried: [LIST ATTEMPTS]

Code causing issue:
[PASTE CODE]
```

## Code Request
```
I need Unity C# code for: [DESCRIBE FEATURE]

Requirements:
- [REQUIREMENT 1]
- [REQUIREMENT 2]

It should work with my existing:
- [RELATED SYSTEMS]

Please provide the complete script.
```

## Optimization Help
```
My Unity game has performance issues:

Problem: [FPS drop/Stutter/Loading]
When: [DESCRIBE SITUATION]
Platform: [PC/Mobile/Console]

I have: [NUMBER] of [OBJECTS/EFFECTS]

What should I optimize?
```

---

# Quick Reference Card

## Common Mistakes
- [ ] Forgetting to Apply after changing import settings
- [ ] Not checking "In Place" on locomotion animations
- [ ] Wrong Avatar assignment
- [ ] Missing parameter names (typos)
- [ ] Not tagging Player as "Player"
- [ ] Not setting layers correctly

## Key Shortcuts (Unity)
- `Ctrl+S` - Save
- `Ctrl+D` - Duplicate
- `Ctrl+P` - Play/Stop
- `F` - Focus selected object
- `Ctrl+6` - Animation window

## Files You Should Backup
- Input Actions asset
- Animator Controllers
- ScriptableObjects
- All scripts
- Prefabs

---

*Guide created based on Unity FPS Sample analysis. Use with AI assistance for best results!*
