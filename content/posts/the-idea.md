+++
date = '2025-12-09T10:45:45+01:00'
draft = false
title = 'The Idea'
+++

## Introduction: The HCI Challenge
For my "Interaction in Virtual and Artifical Reality" class, we were assigned to develop a novel and creative way to interact and move in VR/AR. We were completely free in choosing what to do and how to do it. That also meant we were free to decide how to approach challenges like presence, motion sickness, interactivity, story line, and so on.

I had some trouble coming up with an idea at first. After a while, I thought about implementing a kind of Human: Fall Flat-style locomotion and interaction system, as I remember it as a very fun but challenging game. It also had the added benefit that climbing is a great way to avoid cybersickness. But the idea did not feel quite original enough, since it was not really my own. So I started talking to basically everyone I thought might have good ideas.

## The Spark: A Lunch with my Dad
One day, I was having lunch with my dad, and I told him about the project. He is a physicist, so I thought he might have some interesting ideas. He suggested that I could use the concept of gravity to create a new way of moving in VR. He explained that an exponential increase in velocity could be a fun way to move in VR. Truth be told, I did not completely understand it at first, but it sounded like it could be fun. At the same time, I knew there would be a huge trade-off in terms of motion sickness, so I was not completely sold on it. But because the idea actually felt novel, I decided to pitch it to my professor. He seemed to like it, and I decided to build a prototype to see if it would work.

## The Blueprint: Localized Gravity Spheres
The player is able to spawn one gravitational sphere per hand: blue (right) and red (left). Based on Newton's Law of Gravitational Force, the player is pulled toward the active sphere with a force proportional to 1/r², where r is the distance between the player and the sphere.

The player can move by placing the sphere in different locations and using gravity to traverse the environment, much like a space explorer using gravity assists to slingshot around planets.

## The First Prototype: Core Gravitational Interaction
*Note: During the development and debugging of the C# scripts for this project, I utilized LLMs (specifically Perplexity and ChatGPT).*

```csharp
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

        rigidbody.AddForce((mass / (distance * distance)) * forceDirection, ForceMode.Acceleration); // Apply gravitational force
    }
}
```
For the gravitational sphere to control the motion, I first had to disable the Rigidbody’s built-in gravity; otherwise, the default global gravity would dominate. The gravitational force is applied in FixedUpdate. Each frame, the script computes the direction and distance to the sphere’s center, calculates the force magnitude, and then applies it to the character’s Rigidbody using ForceMode.Acceleration.

Inside a small inner radius, the character is simply stopped to avoid the "infinite force" spike that occurs as r approaches zero, preventing unstable behavior at the center.

{{< youtube dDaV9fnUTUE >}}

The video shows my first prototype of the gravitational interaction using a single sphere. I experimented with moving this sphere around the character to see how it influenced their path and to get a feeling for whether this interaction could actually work as a locomotion method in VR. Trying it in VR myself, it felt like a playful and somewhat intuitive way to move around, at least if you are not too prone to motion sickness. However, it could also very easily cause uncontrollable oscillation and overshooting, which became one of the main challenges I would have to face.


### It Works! But Is It Playable?
The physics worked as they should, but that was only the first step. "Physically accurate" turned out not to be synonymous with "user-friendly." The character could move, but controlling that movement was a whole different story.

With the Rigidbody gravity turned off, there was no easy way to stop unwanted movement, which led to two main issues:

Overshooting: The character would often overshoot the target location, especially when trying to stop or change direction. This was because the gravitational force would continue to pull the character even after they had passed the desired point. While this effect can be fun and useful in some contexts, it made precise movement difficult and could lead to frustration.

Oscillation: If the sphere was placed in a way that made the character's trajectory non-linear, the character could end up orbiting the sphere. While this would eventually lead to a stop, it could take a long time, and that is not exactly the most fun way to get there.

To make this a viable locomotion system, I needed to find a way to give the player more control over the gravitational forces.

Next post: [Gravitational Interaction]({{<ref gravitational-interaction >}})
