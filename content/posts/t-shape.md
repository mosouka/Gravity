+++
date = '2026-02-25T18:09:45+01:00'
draft = false
title = 'The T-Shape Challenge'
+++

With the locomotion system feeling snappy and "skill-based," it was time to apply it to a specific interaction task. The goal: Manipulate a T-shaped object to match a target ghost-shape in 3D space. This requires both precise translation (positioning) and rotation.

# The Failure of Gravity
Initially, following a suggestion from my project pitch, I tried to use the gravity spheres to move the T-shape. The idea was elegant and resourceful: instead of pulling the player, the spheres would pull the object.

However, in practice, I faced a few struggles making this user-friendly:

* **The Overshoot**: Because the T-shape had to first be non-kinematic to be properly influenced by the physics-based gravity spheres, it would often accelerate toward the sphere, fly past it, and enter a chaotic oscillation.
* **The Rotation**: The T-shape would often rotate uncontrollably when influenced by the spheres, making it difficult to achieve the precise orientation needed to match the target shape.

## The Hacks
I added invisible walls to prevent the T-shape from flying too far away, and I implemented a "kill switch" that would stop all movement and make the T-shape kinematic once its position was close to the target (<0.05). Once in close proximity, an automatic "task sphere" would spawn and pull the player towards the T-shape, allowing them to rotate it with the XR Toolkit's built-in XR Direct Interaction.

While this worked and I was initially happy with the result, it felt like a "hacky" solution that wasn't satisfying to use. The movement felt uncontrollable, and the task sphere sometimes pulled the player too close to the T-shape, making it difficult to rotate. Furthermore, it was frustrating to be "paralyzed" while the interaction task was being completed, as the gravity spheres no longer affected the player and the only means of movement was taken away.

# The Pivot: Ray Guns
I decided to move away from gravitational pull for interaction and instead implemented a specialized Ray Interaction System. This system neatly integrates with the "Gravity Guns" but adds extra behavior for precise object manipulation.

## Interaction Logic
To keep the UI clean and focused on the gravity spheres, the interaction rays are invisible by default. They only activate while the player presses the Grip button.
* **Visual Feedback**: The right gun emits a blue ray; the left gun emits a red ray. The preview spheres become 100% transparent, leaving only a subtle outline to show the "aim" point.
* **Filtering**: The rays only interact with objects on a specific GravityObject layer mask.

## Precise Translation and Rotation
When a ray hits the T-shape, it essentially becomes an extension of the controller.
* Single-Hand Control: By moving the controller, the player can translate and rotate the object using all degrees of freedom. This feels like moving an object with a long pole.

* The Dual-Hand Pivot (Precision Mode): Rotation with one hand can be tricky, especially with guns in your hands. For a more "natural" feeling of rotating the shape with a ray, I implemented a Dual-Hand Control mode to solve this.

* * The Precision Logic: When both rays are attached to the object, the object's position is fixed (stored). The user can then use both hands to "twist" or rotate the object around its center. This separation of translation and rotation allowed for the exact precision needed to match the target ghost-shape.

Next Chapter: Putting it all together - the final user experience and the user study.