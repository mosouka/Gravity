+++
date = '2026-03-09T15:23:05+01:00'
draft = false
title = 'Gravitational Interaction'
topics = ['implementation']
+++
Once the core physics were verified, the project moved from a single-script test into a functional VR traversal system. This required building a robust "hand" system and a way to manage the state of the world's gravity.

## Spawning and Controlling the Spheres
I implemented two spawners Blue (Right) and Red (Left), each managing one gravity sphere. In my final iteration, these spawners were conceptualized as ray guns. This introduces a specific tactical layer to the locomotion: you can have two forces acting on you simultaneously, allowing for smoother locomotion and more complex trajectories.

### The Gravity Logic: The Global Toggle

A key technical challenge was managing the player’s relationship with "normal" world gravity. For the spheres to work effectively, I had to ensure the default Unity gravity didn't interfere. To do this, I consulted Perplexity to implement a tracker to monitor the active spheres:

1. Activation: When a sphere is spawned via the trigger, a counter increases and the player Rigidbody’s useGravity is set to false.

2. Persistence: As long as at least one sphere is active, global gravity stays off. This allows for the "slingshot" effect, where you whip past a sphere's center.

3. The Kill-Switch: Global gravity only re-activates once all spheres are despawned. In case you get stuck in an uncontrollable trajectory, you can simply despawn both spheres to reset your momentum and regain control.


## Iterative Design: Previewing and Manipulation
It took me several iterations and ideas to get to the final version of the gravitational interaction. These steps included:

### 1. Spawning
To make the spheres more controllable, I implemented a preview system that shows where the sphere will spawn based on the raycast from the controller. A sphere is placed by pressing the trigger, and despawned by either pressing the trigger again or by the player physically colliding with it (OnTriggerEnter), which feels like "collecting" the sphere.

### 2. Distance Manipulation
Next, I added depth control. By moving the joystick in the Y-direction, the player can slide the preview sphere further away or pull it closer. This was essential for reaching specific locations and creating more complex trajectories.

### 3. Size (Mass) Scaling
I implemented scale manipulation via the joystick X-direction. Because the mass of the sphere is tied to its scale, making a sphere larger increases the gravitational pull. This gives the player "analog" control over their acceleration.

### 4. Shoot Animation
Finally, because I added the guns for the final version, I thought a shooting animation would be nice touch. When pressing the Trigger, the sphere fires from the tip of the gun to the target position, scaling up from 5\% of its final size during flight.

## Refining the "Feel" of the Force
The gravitational spheres themselves also had an iterative tweaking process to make locomotion smoother and feel more controllable.

### Stop?
Initially, the rigidbody's velocity would be set to zero once the player got close enough to the sphere ($r^2 < 1e-6$). I thought this could be a good way to have the player "land", stop their movement and then move on from there. However, this also slowed down movement. During one of the discussions in class, my professor suggested I could use a trigger collider or time-limited spheres. Comparing the two, the trigger collider makes the movement so much smoother and faster, especially because "collecting" the sphere allows for quicker successive movements. 

### Gravitational Softening
I was telling a friend (and one of the people that participated in the user study) that there was a catapulting effect when passing through the center of a gravitational sphere, which felt uncontrollable to me. He suggested using a softening constant to the force calculation. So, I took to Perplexity, telling it to add gravitational softening to my code. Perplexity implemented a Plummer-like softening, which is apparently also used in galactic simulations. For anyone interested, this is the equation:
$$F = G \frac{m_1 m_2}{(r^2 + \epsilon^2)^{3/2}}$$

Where $\epsilon$ is the softening constant. This adjustment significantly reduced the "catapulting" effect, making the movement feel more controlled and less prone to wild oscillations.

### Force Amplification
During my final presentation and first game demo, my professor and classmates gave me the feedback that the start of the acceleration felt very (too) slow. So, I added an amplification factor, so that accerlation starts faster even with small masses and larger distances. While my desire to make a physically accurate simulation was strong, I also wanted the system to be fun and feel good to use. My professor rightfully told me "Realistic went out the window once you were able to create gravitational force fields". True that.


I can honestly say that I am quite happy with the result of my gravitational spheres and the spawning system. While it can be hard to control, it still feels very fun to me and I am happy I was able to make a (kind of) physically accurate gravity simulation, albeit with the help of my good friend Perplexity. It was also fun to experiment with the different parameters and see how they influenced the feel of the movement. While I didn't implement Human: Fall Flat, this locomotion system brings a similar sense of chaos.

Next post: [The T-Shape Challenge]({{<ref t-shape >}})