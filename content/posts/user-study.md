+++
date = '2026-03-15T12:57:45+01:00'
draft = false
title = 'The Final Experience - Demo and User Study'
+++
## The Demo
{{< youtube K8EPBnRTFdg  >}}

## The User Study
With the implementation complete, I was ready to test the experience with real users. I conducted a small user study with 3 participants to gather feedback on the locomotion and interaction systems. The study consisted of two parts: users testing out the parkour, followed by a (not so) short questionnaire I had generated to get some more structured feedback. 

The questionnaire included the following questions:
1. I quickly understood how to move using the gravity fields. (1-5)
2. The locomotion system was easy to learn. (1-5)
3. The object interaction system was easy to learn. (1-5)
4. I felt in control while moving through the environment. (1-5)
5. I was able to control my movement trajectory accurately. (1-5)
6. I could reliably reach the locations I intended to reach. (1-5)
7. The movement behaved as I expected. (1-5)
8. It was easy to correct my movement if I made a mistake. (1-5)
9. I felt in control while manipulating objects. (1-5)
10. It was easy to position objects precisely. (1-5)
11. Rotating objects using two hands felt natural. (1-5)
12. The locomotion system felt comfortable to use. (1-5)
13. I did not experience motion sickness while using the system. (1-5)
14. The interaction method was enjoyable to use. (1-5)
15. The system felt immersive. (1-5)
16. I would like to use this interaction system in a VR game. (1-5)
17. What did you like most about the interaction system? (Open-ended)
18. What part of the system was most difficult to use? (Open-ended)
19. Do you have suggestions for improving the interaction? (Open-ended)

In case you're thinking "Cosima, that is way too long.": Yes, you're probably right, but because I wanted to get as much feedback as possible, I thought it would be good to have a thorough questionnaire. Plus, because 2/3 participants did it remotely, I thought this was the best way to get as much feedback as possible.

In any case, the feedback was a lot better than I thought, to be honest. I knew that my locomotion system had a strong learning curve and high potential for motion sickness, so I was frankly a bit anxious to see how people would react to it. But the participants were surprisingly positive about the experience.

As the questionnaire is quite long, I won't go through all the results here, but I let Gemini help me summarize the key findings and insights from the user study:

### Locomotion
Overall, the "fun-factor" of the gravity sphere is rated very high. Question items 14-16 were rated by all three participants either a 4 or 5. Furthermore, they stated that "The adjustable force makes it really fun", "Controllability and challange are nicely balanced", and "Flying around and slingshots !" were what they liked most about the interaction. Furthermore, question item 6 and 7 also had high ratings, with all participants giving a rating of 3 or 5, indicating that they could reliably reach their intended locations and that the movement behaved as they expected.

However, the learning curve, controllability, and trajectory control (1, 2, 4, 5, 8) were rated lower, with some participants giving ratings as low as 2 for these items. The open-ended feedback highlighted that "I would overshoot often but got the hang of it later", "I felt totally out of control when using two gravity fields, so I didn’t.", and "Precise controlling of acceleration and placing spheres while moving ." were some of the most difficult aspects of the system as a whole. At the same time, question item 6 had a high rating 

Surprisingly, question item 13 was rated evenly between 3 and 5 (I just hope they didn't miss the "not"). This is surprsingly low, but might also be because of the participants being less prone to cybersickness.

### Interaction
All participants thought that the interaction system was easy to learn (3), and they felt in control while manipulating objects (9). Morever, it was overwhelmingly easy to position objects precisely (10). However, question item 11 had a more ambiguous rating, with 2/3 rating a 3 and 1/3 rating a 4, indicating that it felt somewhat natural to rotate objects using two hands, but there is room for improvement.

### Times
As part of the parkour mechanics, the completion time for the sections and the interaction tasks were also recorded. I forgot to write down the times for one of the participants, but I do have the times for the other two.

| Parkour        | Participant 2       | Participant 3       | Me |
| -------------- | ------------------- | ------------------- | ------------------- | 
| Section 1      | 105.3s, 14/16 coins | 128.5s, 15/16 coins | 144.9s, 4/16 coins  |
| Task 1         | 2.5s, 0.01 error    | 4.7s, 0.00 error    | 10.8s, 0.00 error   |
| Section 2      | 96.0s, 30/30 coins  | 116.3s, 30/30 coins | 101.2s, 16/30 coins |
| Task 2         | 2.5s, 0.01 error    | 3.5s, 0.00 error    | 7.9s, 0.00 error    |
| Section 3      | 77.2, 21/23  coins  | ⁠131.1s, 23/23 coins | 83.6s, 18/23 coins  |
| Task 3         | 2.1s, 0.01 error    | 8.0s, 0.00 error    | 7.4s, 0.00 error    |
| **Total Sections** | **278.6, 65/69 coins**  | **375.9s, 68/69 coins** | 329.7s, 38/69 coins |

### Suggestions and Future Improvements
While one person liked the system the way it is, another noted "Prevent the object interaction from being exploited as a simple grab. And maybe decrease defaul force field size.". When I personally tested the system, I frankly didn't notice the interaction system could be exploited like that. I was fully aware of the lcomotion system's potential exploitations, but this came as a surprise. The participant later texted me that I could implement a distance limit for the ray interaction, so that you can only pick up the object if you are far enough away from it. This would prevent the "grabbing" exploit, where you could do a simple grab. 

The third person suggested "Maybe instant replacing of old sphere instead using two clicks for picking up and placing". While I really like this idea and agree with it, I also think that it would make the system a lot more difficult to use, especially for beginners, as I currently see no way of implementing this while still having the "kill-switch" functionality.

However, I have personally also thought that a different setting, like a true zero-gravity environment, could be a better fit for the gravity spheres, as it would allow for more freedom of movement. Working out how to avoid overshooting and uncontrollable trajectories in such a setting would be a fun challenge, and it would also fit the sci-fi theme of the ray guns and black hole spheres better.

Another idea suggested by one of the participants is to make this more of a puzzle game, where you can first explore the parkour and then think of how you could place as little spheres as possible to get to the end of the course. This would be a fun way to make the most out of the unique mechanics of the gravity spheres, and it would also give players a clear goal to work towards while learning how to use the system.

In a similar vein, I have also thought about implementing portal inspired puzzle rooms, where you have to strategically place the spheres to get through the room and also have small puzzles you have to solve with the interaction system. This would be a fun way to make the most out of the unique mechanics of the gravity spheres, and it would also give players a clear goal to work towards while learning how to use the system. Furthermore, just like in portal, you can guide the player through the learning curve by making the challanges progressively more difficult, starting with simple movement and then gradually introducing more complex mechanics like using multiple spheres and the interaction system.

## Final Reflections
Overall, the class IVAR was by far one of the best I have had during the course of my studies thus far. Not only did I have a lot of fun working on this project, I also thoroughly enjoyed being part of the class. I have not had the pleasure to experience a class with this much discussion and active participants from the students, something that I wager to say is a bit of a rare sight in CS.

While we were alone in our projects, we were never alone in our struggles. The class was a great place to share ideas, get feedback, and learn from each other. I think this collaborative environment was one of the key factors that made this class so enjoyable and rewarding. Furthermore, I don't think my project would be the way it is without the feedback and suggestions I got from my classmates and professor, so I am very grateful for that.

While I had a strong interest in HCI prior to taking this class, my interest has only been further fueled by this project. It makes me want to dive deeper into the field and explore, as well as research, more about this field of studies. Having a bachelor's degree in Cognitive Science, I believe that this area is one that can neatly combine my interests in both technology and human behavior, and I am excited to see where this interest will take me in the future.

In any case, I am very happy with the final result of my project, and I am grateful for the opportunity to work on such a fun and creative project. While I didn't necessarily improve my non-existant programming skills, I believe I have learned a lot in both the class and my project. This wasn't my first rodea with VR and Unity, but I do believe that I have gained a deeper understanding of both technologies and hope to use this knowledge in future projects.

Thank you to everyone who participated in the user study, and thank you to my professor and classmates for all the feedback and support throughout the project. It has been a great experience.