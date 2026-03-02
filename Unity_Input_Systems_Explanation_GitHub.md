# Unity Input Systems --- Complete Beginner Guide

*A clear and practical explanation of both input systems in Unity.*

------------------------------------------------------------------------

# 📚 Project Structure Reference

This explanation follows the same structure we used in the project:

    InputReader        → Handles input
    GameManager        → Handles state and timing
    Microgames         → Handle win/lose logic
    DebugSelector      → Hidden development tool

**Core Principle:**\
Input must be separated from game logic.

------------------------------------------------------------------------

# 🕹 PART 1 --- Old Input System (Legacy / Deprecated)

## Namespace

``` csharp
using UnityEngine;
```

## Main Class

``` csharp
Input
```

The old system reads hardware state every frame.

------------------------------------------------------------------------

## 1️⃣ Input.GetKey()

``` csharp
Input.GetKey(KeyCode.Space)
```

### What it does

Returns `true` every frame while the key is being held.

### When to use

Continuous input (movement, charging, holding).

------------------------------------------------------------------------

## 2️⃣ Input.GetKeyDown()

``` csharp
Input.GetKeyDown(KeyCode.Space)
```

### What it does

Returns `true` only on the frame the key is pressed.

### When to use

Single actions like jumping or confirming.

------------------------------------------------------------------------

## 3️⃣ Input.GetKeyUp()

``` csharp
Input.GetKeyUp(KeyCode.Space)
```

### What it does

Returns `true` only on the frame the key is released.

------------------------------------------------------------------------

## ⚠ Why This System Is Deprecated

-   Limited flexibility
-   Difficult control rebinding
-   Hard to support multiple devices
-   Not modular
-   Can conflict with the new system

------------------------------------------------------------------------

# 🎮 PART 2 --- New Input System (Recommended)

## Namespace

``` csharp
using UnityEngine.InputSystem;
```

## Core Classes

-   `Keyboard`
-   `Mouse`
-   `InputAction`

This system is event-based and modular.

------------------------------------------------------------------------

# Method 1 --- Direct Device Access

Example:

``` csharp
Keyboard.current.spaceKey.wasPressedThisFrame
```

### Equivalent to:

``` csharp
Input.GetKeyDown(KeyCode.Space)
```

------------------------------------------------------------------------

Other equivalents:

  Old System   New System
  ------------ ----------------------
  GetKey       isPressed
  GetKeyDown   wasPressedThisFrame
  GetKeyUp     wasReleasedThisFrame

------------------------------------------------------------------------

# Method 2 --- InputAction (Recommended)

Example from our project:

``` csharp
confirmAction = new InputAction(
    name: "Confirm",
    type: InputActionType.Button,
    binding: "<Keyboard>/space"
);
```

Enable:

``` csharp
confirmAction.Enable();
```

Check:

``` csharp
confirmAction.WasPressedThisFrame();
```

------------------------------------------------------------------------

## Why InputAction Is Better

Instead of checking:

> "Is the Space key pressed?"

We check:

> "Did the Confirm action happen?"

This allows:

-   Keyboard
-   Mouse
-   Gamepad
-   Touch

Without changing game logic.

------------------------------------------------------------------------

# 🔎 Technical Concepts (Research Later)

-   Action Maps
-   Device Abstraction
-   Input Events
-   Control Rebinding

These enable scalable architecture and multiplayer input separation.

------------------------------------------------------------------------

# 🏗 How It Fits Our Project

## InputReader

-   Handles ALL input
-   Uses InputAction
-   Contains no game logic

## GameManager

-   Does not read keys
-   Manages states and timers

## Microgames

-   Check InputReader
-   Decide Win or Lose

## DebugSelector

-   Uses `Keyboard.current.digit1Key.wasPressedThisFrame`
-   Development-only tool

------------------------------------------------------------------------

# 🧠 Final Advice

1.  Never mix both systems.
2.  Separate input from game logic.
3.  Prefer InputAction for real projects.
4.  Use direct keyboard access only for debug tools.
5.  Think in terms of player intentions, not keys.

------------------------------------------------------------------------

# End of Document
