+++
date = '2026-03-09T15:23:05+01:00'
draft = false
title = 'Gravitational Interaction'
topics = ['implementation']
+++
Once the core physics were verified, the project moved from a single-script test into a functional VR traversal system. This required building a robust "hand" system and a way to manage the state of the world's gravity.

## Spawning and Controlling the Spheres
I implemented two spawners Blue (Right) and Red (Left), each managing one gravity sphere. In my final iteration, these spawners were conceptualized as ray guns. This introduces a specific tactical layer to the locomotion: you can have two "pulls" acting on you simultaneously, allowing for complex curved trajectories.

### The Gravity Logic: The Global Toggle

A key technical challenge was managing the player’s relationship with "normal" world gravity. For the spheres to work effectively, I had to ensure the default Unity gravity didn't interfere. I implemented a tracker to monitor the active spheres:

1. Activation: When a sphere is spawned via the trigger, a counter increases and the player Rigidbody’s useGravity is set to false.

2. Persistence: As long as at least one sphere is active, global gravity stays off. This allows for the "slingshot" effect—where you whip past a sphere's center—to be a deliberate, skillful maneuver rather than a physics glitch.

3. The Kill-Switch: Global gravity only re-activates once all spheres are despawned.


## Iterative Design: Previewing and Manipulation
It took me several iterations and ideas to get to the final version of the gravitational interaction. These steps included:

### 1. Spawning
To make the spheres more controllable, I implemented a preview system that shows where the sphere will spawn based on the raycast from the controller. A sphere is placed by pressing the trigger, and despawned by either pressing the trigger again or by the player physically colliding with it (OnTriggerEnter), which feels like "collecting" the sphere.

### 2. Distance Manipulation
Next, I added depth control. By moving the joystick in the Y-direction, the player can slide the preview sphere further away or pull it closer. This was essential for reaching specific locations and creating more complex trajectories.

### 3. Size (Mass) Scaling
Finally, I implemented scale manipulation via the joystick X-direction. Because the mass of the sphere is tied to its scale, making a sphere larger increases the gravitational pull. This gives the player "analog" control over their acceleration.

## Refining the "Feel" of the Force
To make the movement feel less like a mathematical simulation and more like a game, I applied two critical refinements:

* Gravitational Softening: I was telling a friend (and one of the people that participated in the user study) that there was a catapulting effect when passing through the center of a gravitational sphere, which felt uncontrollable to me. He suggested using a softening constant to the force calculation. While I now despawn the sphere upon collision—which largely mitigates this—the softening constant remains in the code as a safeguard against extreme acceleration.

* Force Amplification: Based on feedback from my professor, I added an amplification factor. Newton’s laws can feel "heavy" or slow to start; amplification ensured that even small spheres provided enough initial "tug" to keep the locomotion snappy and responsive.

The result is a system where the player is the pilot of their own momentum. You choose the mass, you choose the distance, and you choose when to let go. While this method has a learning curve, the sense of mastery once you harness those gravitational forces is incredibly rewarding.

Next post: [The T-Shape Challenge]({{<ref t-shape >}})