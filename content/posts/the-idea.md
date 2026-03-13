+++
date = '2026-02-25T18:09:45+01:00'
draft = false
title = 'The Idea'
featured = true
+++
## Introduction: The HCI Challenge
For my "Interaction in Virtual and Artifical Reality" class, we were assigned to develop a novel and creative way to interact and move in VR/AR. We were completely free in choosing what to do and how to do it. We could freely choose how to go about challenges like presence, motion sickness, interactivity, story line, and so on.

Not being the most creative person, I had some trouble coming up with an idea at first. After a while, I had the idea of implementing a kind of Human: Fall Flat style locomotion and interaction system, as I remember it as a very fun but challenging game. It had the added benefit that climbing is a great way to avoid cyber sickness. But this idea was not quite original, as it wasn't my own. So I talked to everyone I thought could have good ideas.

## The Spark: A Lunch with my Dad
One day, I was having lunch with my dad, and I told him about the project. He is a physicist, and I thought he might have some interesting ideas. He suggested that I could use the concept of gravity to create a new way of moving in VR. He explained that an exponential increase in velocity could be a fun way to move in VR. Thruth be told, I didn't completely understand it at first, but it sounded like it could be fun. However, I knew there would be a huge trade-off with motion sickness, so I wasn't completely sold on it. But because the idea was actually novel, unlike my own, I decided to pitch it to my professor. He seemed to like it, and I decided to build a prototype to see if it would work.

## The Blueprint: Gravitational Locomotion
The player is given two ray guns (blue = right, red = left) to spawn gravitational spheres. When a sphere is active, the player rigidbody's gravity is turned off, as otherwise the default global gravity would dominate and interfere with the effect of the sphere. Based on Newton's Law of Gravitational Force ($F = G \frac{m_1 m_2}{r^2}$), the player is pulled towards the sphere with a force proportional to $1/r^2$, where r is the distance between the player and the sphere. The player can move by placing the sphere in different locations, and can stop by either disabling the sphere or colliding with it.

## The first Prototype: Core Gravitational Interaction
```
public class GravitationalField : MonoBehaviour
{
    public float mass = 20f; // arbitrary mass of the sphere
    public float innerRadius = 0.1f;

    public Rigidbody rigidbody;

    void Start()
    {
        rigidbody.useGravity = false; // Turn off default gravity
    }

    void FixedUpdate()
    {
        Vector3 toCenter = transform.position - rigidbody.position; // Direction from character to center of sphere
        float distance = toCenter.magnitude; // Distance between character and sphere

        if (distance < innerRadius)
        {
            rigidbody.linearVelocity = Vector3.zero; // Stop the character if it's within the inner radius
            return;
        }

        Vector3 forceDirection = toCenter.normalized; // Normalize the direction vector

        rigidbody.AddForce((mass / (distance * distance))* forceDirection, forceMode.Acceleration); // Apply gravitational force
    }
}
```
For the gravitational sphere to control the motion, I first had to disable the Rigidbody’s built‑in gravity; otherwise, the default global gravity would dominate and interfere with the effect of the sphere. The gravitational force is applied in FixedUpdate, which runs at a fixed time step and is therefore better suited for physics calculations than the regular Update loop. Each frame, the script computes the direction and distance to the sphere’s center, calculates the force magnitude as proportional to $1/r^22$, and then applies it to the character’s Rigidbody using AddForce. Inside a small inner radius, the character is simply stopped to avoid unstable behavior very close to the center.

{{< youtube dDaV9fnUTUE  >}}

The video shows my first prototype of the gravitational interaction using a single sphere. I experimented with moving this sphere around the character to see how it influenced their path and to get a feeling for whether this interaction could actually serve as a locomotion method in VR. Trying it in VR myself, it felt like a playful and somewhat intuitive way to move around. At least if you are not too prone to motion sickness. However, it could also easily cause uncontrollable oscillation and overshooting, which presented one of the challanges I would have to face. 


### It Works! But Is It Playable?
The physics worked as they should, but that’s just the first step. "Physically accurate" turned out to not be synonymous with "user-friendly." The character could move, but controlling that movement was a whole different story.

With the rigidbody gravity turned off, there was no way to stop unwanted movement, which led to two main issues:

1. **Overshooting**: The character would often overshoot the target location, especially when trying to stop or change direction. This was because the gravitational force would continue to pull the character even after they had passed the desired point. While this effect can be fun and useful in some contexts, it made precise movement difficult and could lead to frustration.
2. **Oscillation**: If the sphere was placed in a way that the character's trajectory wasn't straight, the character would end up orbiting the sphere. While this would eventually lead to a stop, it could take a long time, and isn't exactly the most fun way to get there.

To make this a viable locomotion system, I needed to find a way to give the player total control over the gravitational forces.

Next post: [Gravitational Interaction]({{<ref gravitational-interaction >}})