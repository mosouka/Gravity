+++
date = '2026-02-25T18:09:45+01:00'
draft = false
title = 'Reverse Classroom - Climbing'
+++

{{< youtube HL0mo_83mMo >}}

As I was unfortunately sick on the day we were supposed to present our reverse classroom assignment, I will present my implementation here instead.

While [Valem](https://www.youtube.com/watch?v=mHHYI7hzZ6M&t=29s) used Unity version 2019.3.8f1 and an older version of the XR Interaction Toolkit in their tutorial, I used Unity 6 and XR Interaction Toolkit version 3.2.2. This meant that I had to make some adjustments to the code and implementation, but the overall logic and structure of the climbing system remained the same. If you're too lazy to implement the climbing system from scratch, the newest XR Interaction Toolkit also comes with a built-in climbing system.

## Requirements
To start, you need a working VR setup with a Character Controller and an XR Direct Interactor. Optionally, you can also add Continuous Movement.

Then, of course, you need a climbing wall, which you can download [here](https://drive.google.com/file/d/1gwLXOBAx5IqPFIDcJJIyJSVAgR9GifKX/view).

While there is a Continuous Movement script included in the XR Toolkit, it apparently was not included in the version Valem used, so here is an implementation adapted to the new Input System and XR Interaction Toolkit:

```csharp
using UnityEngine;
using UnityEngine.XR.Interaction.Toolkit;
using UnityEngine.XR;

public class ContinuousMovement : MonoBehaviour
{
    public float speed = 1.0f;
    public XRNode inputSource;
    private Vector2 inputAxis;
    private CharacterController characterController;

    // Start is called once before the first execution of Update after the MonoBehaviour is created
    void Start()
    {
        characterController = GetComponent<CharacterController>();
    }

    // Update is called once per frame
    void Update()
    {
        InputDevice device = InputDevices.GetDeviceAtXRNode(inputSource);
        device.TryGetFeatureValue(CommonUsages.primary2DAxis, out inputAxis);
    }

    private void FixedUpdate()
    {
        Vector3 direction = new Vector3(inputAxis.x, 0, inputAxis.y);

        characterController.Move(direction * Time.fixedDeltaTime * speed);
    }
}
```

This script allows the player to move around using the joystick. It gets the input from the specified XRNode (left or right hand) and moves the character controller in the direction of the input axis, multiplied by a speed factor and the fixed delta time.

## Climbable Interactable
The next step is to make the climbing knobs interactable. This requires two steps:
1. Add a Rigidbody component to each knob. Make sure to set the Rigidbody to Kinematic, as we do not want the knobs to be affected by physics.
2. Implement a custom interactable script. In my case, I called it `ClimbInteractableManual`, because newer versions of the XR Interaction Toolkit already include a built-in `ClimbInteractable`.

```csharp
using UnityEngine;
using UnityEngine.XR.Interaction.Toolkit;
using UnityEngine.InputSystem.XR;

public class ClimbInteractableManual : UnityEngine.XR.Interaction.Toolkit.Interactables.XRBaseInteractable
{
        protected override void OnSelectEntered(SelectEnterEventArgs args)
        {
            base.OnSelectEntered(args);

            var interactor = args.interactorObject as UnityEngine.XR.Interaction.Toolkit.Interactors.XRBaseInteractor;
            if (interactor != null)
            {
                var climbingHand = interactor.GetComponent<TrackedPoseDriver>();
                if (climbingHand != null)
                {
                    Climber.climbingHand = climbingHand;
                }
            }
        }

        protected override void OnSelectExited(SelectExitEventArgs args)
        {
            base.OnSelectExited(args);
            
           var interactor = args.interactorObject as UnityEngine.XR.Interaction.Toolkit.Interactors.XRBaseInteractor;

            if (interactor != null)
            {
                var climbingHand = interactor.GetComponent<TrackedPoseDriver>();
                if (climbingHand != null && Climber.climbingHand == climbingHand)
                {
                    Climber.climbingHand = null;
                }
            }
        }
}
```
To explain what this script does: it overrides the `OnSelectEntered` and `OnSelectExited` methods of `XRBaseInteractable`. When the player selects (grabs) a climbing knob, the script checks whether the interactor is an `XRBaseInteractor` and whether it has a `TrackedPoseDriver` component. If both conditions are met, it assigns that climbing hand to the `Climber` script. When the player releases the knob, it checks whether the climbing hand is the same one that was previously assigned, and if so, sets it back to null. This is how the climbing hand is registered and deregistered.

## Climber
Now that we have interactable climbing knobs, we need to implement the climbing logic.

First of all, we need to attach a CharacterController component to the player so they can move. We also need a Gravity component so that, when releasing the wall, the player falls back to the ground instead of awkwardly staying in the air. Then, optionally, we can also add our Continuous Movement script to allow movement on the ground.

Next, we need to implement the `Climber` script, which handles the climbing logic. The script checks whether there is a climbing hand registered, and if so, moves the player based on the movement of that climbing hand.

```csharp
using UnityEngine;
using UnityEngine.InputSystem.XR;
using UnityEngine.InputSystem;
using UnityEngine.XR.Interaction.Toolkit.Locomotion.Movement;
using UnityEngine.XR.Interaction.Toolkit.Locomotion.Gravity;

public class Climber : MonoBehaviour
{
    public CharacterController characterController;
    public ContinuousMoveProvider continuousMovement;
    public GravityProvider gravityProvider;
    public static TrackedPoseDriver climbingHand;

    public float climbSpeed = 1.0f;

    private TrackedPoseDriver previousHand;
    private Vector3 previousPos;
    private Vector3 currentVelocity;

    
   void FixedUpdate()
    {
        if (climbingHand != null)
        {
            if (previousHand == null || climbingHand != previousHand)
            {
                previousHand = climbingHand;
                previousPos = climbingHand.positionInput.action.ReadValue<Vector3>();
            }
            continuousMovement.enabled = false;
            gravityProvider.enabled = false;
            Climb();
        }
        else
        {
            continuousMovement.enabled = true;
            gravityProvider.enabled = true;
            previousHand = null;
        }
    }

    // Climbing computation
    private void Climb()
    {
        Vector3 currentPos = climbingHand.positionInput.action.ReadValue<Vector3>();
        currentVelocity = (currentPos - previousPos) / Time.fixedDeltaTime;
        characterController.Move(-currentVelocity * climbSpeed * Time.fixedDeltaTime);
        previousPos = currentPos;
    }
}
```
To explain the script: it checks whether there is a climbing hand registered. If there is, it disables continuous movement and gravity, and then calls the Climb() method. That method calculates the current velocity of the climbing hand by taking the difference between the current position and the previous position, divided by the fixed delta time. It then moves the character controller in the opposite direction of the climbing hand’s movement, multiplied by a climb speed factor and the fixed delta time.

In case this confuses you as much as it confused me: we use the negative velocity because when we move our hand down, we want our body to move up.

Finally, the script updates the previous position to the current position for the next frame. If there is no climbing hand registered, it re-enables continuous movement and gravity.

And there we have it! You are now able to climb in VR.

{{< youtube _Zsk5HSdsmw >}}
