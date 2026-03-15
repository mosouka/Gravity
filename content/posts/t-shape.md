+++
date = '2026-03-11T12:00:00+01:00'
draft = false
title = 'The T-Shape Challenge'
topics = ['implementation']
+++

Being satisfied with my locomotion system, it was time to tackle the interaction task. The goal: manipulate a T-shaped object to match a target T-shape in 3D space. This requires both precise translation (positioning) and rotation.

## The Failure of Gravity
### Gravity Hands
My first idea for the interaction was to implement "Gravity Hands". The player's hands would act as localized gravity sources that could pull objects toward them. I thought this would be a natural extension of the gravitational locomotion system and would allow for a consistent interaction paradigm. The player could pull the T-shape toward them with their hands and have it follow. For rotation, they could "pull" from both sides so that the T-shape would levitate in the center and then rotate it by moving one hand around the other.

{{< youtube iTC2RIt7LYA >}}

As the video shows, I wasn't able to implement what I had in mind. While pulling the object worked just fine, the movement was incredibly jerky. Because the force was constantly recalculating based on the distance between the hand and the object's center, the cube would stutter through the air. Rotation was even worse. The object would simply spin uncontrollably, making orientation impossible. Sadly (or luckily?), I quickly dismissed this idea and moved on to a different approach.

### Recycling the Spheres
During the project pitch, my professor suggested that I recycle the gravitational spheres for interaction. Once starting the object task, the spheres would switch to pulling the T-shape instead of the player. As the user was now unable to move, I implemented an auto task-sphere, which would pull the player to the T-shape target once the T-shape was close enough to it. This would then allow the player to rotate the T-shape with the XR Toolkit's built-in XR Direct Interaction.

However, this approach also had its issues:
* **The Overshoot**: Because the T-shape first had to be non-kinematic to be properly influenced by the physics-based gravity spheres, it would often accelerate toward the sphere, fly past it, and enter a chaotic oscillation.
* **The Rotation**: The T-shape would often rotate uncontrollably when influenced by the spheres, making it difficult to achieve the precise orientation needed to match the target shape.

To fix these issues, I added invisible walls to prevent the T-shape from flying too far away, and I implemented a "kill switch" that would stop all movement and make the T-shape kinematic once its position was close enough to the target, i.e. when the task sphere would appear.

{{< youtube Vmk1FSY5NyE >}}

While this worked, and I was initially happy with the results, it still felt like a "hacky" solution that wasn't satisfying to use. The movement felt uncontrollable, and the task sphere sometimes pulled the player too close to the T-shape, making it difficult to rotate. Furthermore, it was frustrating to be "paralyzed" while the interaction task was being completed, as the gravity spheres no longer affected the player and my only means of movement was taken away.

## The Pivot: Ray Guns
I decided to move away from gravitational pull for interaction and instead, inspired by the game Portal, implemented a specialized Ray Interaction System. This system was also the reason I decided to go for the "gun" aesthetic for the spawners, as it fit well with the idea of shooting rays to manipulate objects.

My idea was to have the player shoot invisible rays from their controllers to "attach" to the T-shape. Once attached, the ray would essentially become an extension of the controller, allowing for precise translation and rotation by moving the controller in space. This approach would give the player more direct control over the T-shape's movement and orientation, without relying on the physics-based forces that had caused issues in the previous implementations.

### Interaction Logic
To keep the UI clean and focused on the gravity spheres, the interaction rays are invisible by default. They only activate while the player presses the Grip button.
* **Visual Feedback**: The right gun emits a blue ray; the left gun emits a red ray. The preview spheres become fully transparent, leaving only a subtle outline to show the "aim" point.
* **Filtering**: The rays only interact with objects on a specific `GravityObject` layer mask.

### Precise Translation and Rotation
* **Single-Hand Control**: By moving the controller, the player can translate and rotate the object using all degrees of freedom. It feels a bit like moving an object with a long pole.

* **The Dual-Hand Pivot (Precision Mode)**: Rotation with one hand can be tricky, especially with guns in your hands. For a more "natural" feeling of rotating the shape with a ray, I implemented a dual-hand control mode to solve this.

  * **The Precision Logic**: When both rays are attached to the object, the object's position is fixed. The user can then use both hands to "twist" or rotate the object around its center. This separation of translation and rotation allowed for the exact precision needed to match the target shape.

{{< youtube 7EnEj26gays >}}

The interaction task was by far my biggest challenge, and I went through several iterations and ideas before landing on the ray gun approach. While it was a bit of a pivot from my original idea of using gravity for interaction, I am happy with the final result. The ray gun system felt much more controllable, and I think it still fits the sci-fi/space theme.

Next post: [The Toolkit - Manual, Prefabs, and Controls]({{<ref environment_prefabs >}})
