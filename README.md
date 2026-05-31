# GDIM33 Vertical Slice
## Milestone 1 Devlog
<img width="2559" height="1368" alt="image" src="https://github.com/user-attachments/assets/c2ad7fbb-f6d7-4736-803d-67a9688933b2" />

1. I created a player movement animation visual state machine in the game as a learning and practice of the Unity Visual Scripting system. The design goal of this state machine is to automatically switch character animations based on the player's movement speed. It retrieves the linear velocity vector of the player's Rigidbody2D component every frame, then calculates the player's main movement direction through vector decomposition, and then controls the Animator component to play the corresponding animation. Although I finally chose the key input-based Animator state machine to implement the actual animation control, this visual script graph helped me understand the basic working principle of state machines.

2. I updated my initial task breakdown to include the development steps of the player animation system. The original task breakdown only included the physical logic of player movement. Now I have added the core step of "implementing the player animation state machine" and split it into three sub-steps:
2.1 Import and slice the character sprite sheet, create four independent animation clips corresponding to up, down, left, and right movement respectively
2.2 Create a state machine in Unity Animator, add an idle state and four movement states, and set transition conditions between states
2.3 Write logic in the player controller C# script to set Animator boolean parameters based on player key input and trigger state transitions.

The player animation state machine I actually use is implemented based on Unity Animator. It has five states: a default idle state and four directional movement states. When the player presses any of the WASD keys, the C# script sets the corresponding boolean parameter to true, and the Animator state machine automatically transitions to the corresponding movement animation state; when the player releases the key, the boolean parameter is reset to false, and the state machine returns to the idle state.

This state machine is closely related to other systems in the game: it receives player input data from the TopDownCharacterController.cs C# script, and then controls the Animator component to play the corresponding animation based on these data, realizing the synchronization of player input, physical movement and visual performance.

## Milestone 2 Devlog
Q1. The complicating gameplay feature I developed for this Milestone is the dual-weapon combat system and finite state machine-based intelligent enemy AI. This feature is newly added on the basis of MS1 and was not written in class. It provides a complete combat loop for the game: players can use two weapons, a ranged pistol and a melee knife, to interact with enemies that have independent patrol, chase, return and attack behaviors.
I split this feature into 2 major steps from the simplest to the most complex, and each major step is split into 4 testable sub-steps:

Major Step 1: Dual-weapon Combat System Development

1.Implement basic pistol shooting mechanism
Sub-step: Create a bullet prefab, add Rigidbody2D and CircleCollider2D components, write logic for bullet linear movement and 3-second auto-destruction
Test method: Run the game, click the left mouse button, observe whether bullets are generated from the muzzle and fly forward, and whether they disappear automatically after 3 seconds
Unknown step: How to avoid null reference errors after bullet prefabs are destroyed, Note: I will ask classmates in class, or consult the teaching assistant during office hours after class.

2.Implement weapon orientation system
Sub-step: Get the mouse world coordinates in the Gun script, calculate the angle between the pistol and the mouse, and set the rotation angle of the pistol
Test method: Run the game, move the mouse, observe whether the pistol always rotates following the mouse direction.

3.Implement melee knife attack mechanism
Sub-step: Modify the original Spell script, add OnTriggerEnter2D collision detection, and call the enemy's TakeDamage method when hitting an enemy
Test method: Run the game, press the spacebar, observe whether the knife is displayed, and whether the enemy loses health when hit.

4.Implement weapon linkage logic
Sub-step: Add a coroutine in the player controller to automatically hide the pistol when pressing the spacebar to use the knife, hide the knife and restore the pistol display after 0.2 seconds
Test method: Run the game, press the spacebar, observe whether the pistol automatically hides and whether it automatically shows after the knife attack ends.

Major Step 2: Enemy AI State Machine Development

1.Implement enemy basic attributes and hit feedback
Sub-step: Create a MeleeEnemy script, add parameters such as health, damage, and movement speed, implement the TakeDamage method and the red flash coroutine
Test method: Run the game, directly modify the enemy's health value in the Inspector, observe whether the enemy flashes red, and whether it is destroyed when health reaches 0.

2.Implement enemy patrol state
Sub-step: Get the enemy's spawn point in the Start method, generate a random patrol target every 2 seconds, and make the enemy move towards the target
Test method: Run the game, observe whether the enemy moves randomly around its spawn point.

3.Implement enemy chase and attack states
Sub-step: Calculate the distance between the enemy and the player. When the distance is less than the chase radius, make the enemy move towards the player; when the distance is less than the attack range, call the player's TakeDamage method
Test method: Run the game, approach the enemy, observe whether the enemy chases you, and whether you lose health when touched.

4.Implement enemy return state
Sub-step: When the player runs out of the enemy's chase range, make the enemy move towards its spawn point, and continue patrolling after returning to the spawn point
Test method: Run the game, lead the enemy far away from its spawn point, then run away, observe whether the enemy walks back to its patrol area.

Q2. The Week 5 task breakdown activity was extremely helpful for my development process. It allowed me to break down a seemingly complex feature into individual, testable small modules before I even started writing any code. This incremental development approach enabled me to test after completing each small step, avoiding the situation where I write hundreds of lines of code all at once only to end up with a bunch of hard-to-locate bugs. For example, I first tested the enemy's patrol behavior independently and only added the chase logic after confirming it worked correctly, which significantly reduced the difficulty of debugging.
However, I still encountered an unexpected issue that was not covered in the task breakdown: when I turned the enemy into a prefab and duplicated multiple instances into the scene, all the duplicated enemies would get stuck and not move at all. I later discovered that this was because I had initialized the enemy's spawn point in the Awake method, and Unity prefabs execute the Awake method even in edit mode, causing all enemies' spawn points to be incorrectly set to (0,0,0).
If I were to do the task breakdown again, I would add a dedicated "prefab compatibility testing" step. I would immediately test turning each feature into a prefab and duplicating multiple instances right after its development is completed, to catch bugs related to Unity's prefab mechanism early on.

Q3. I implemented a bridge between C# code and Visual Scripting state machine in the game, where C# code calls the Visual Scripting graph. The involved C# script is TopDownCharacterController.cs (the player controller script). In the Update method of this script, I call the PlayerAnimationStateMachine visual state machine graph I created through the CustomEvent.Trigger API provided by Unity Visual Scripting. When the player presses or releases the WASD keys, the C# code triggers a custom event named "OnPlayerInput" and passes the current input direction and input state as parameters to the visual state machine.This bridge plays a key role in separating input from presentation in my game architecture. I use C# code to handle core game mechanics such as player input, physical movement and combat logic, which are the strengths of code; at the same time, I use visual state machines to handle player animation state transitions, which are the strengths of visual scripting. This combination makes my code clearer and easier to maintain, and also makes the adjustment of animation logic more intuitive.
<img width="2559" height="1368" alt="image" src="https://github.com/user-attachments/assets/27ed09af-943f-4494-9c27-e6ca1f5dcdf3" />

Q4. The Unity system I want you to grade is the Prefab system. I used the Prefab system to create a reusable melee enemy template that includes complete enemy AI logic, basic attributes, colliders, physics components and hit visual feedback. With the Prefab system, I can quickly place multiple enemies with the same behavior and attributes in the scene, greatly improving the efficiency of level design. You can find the blue prefab file named MeleeEnemy under the Assets folder in the Project window.

## Milestone 3 Devlog
<img width="2559" height="1220" alt="d859ed3a788a72d5dfe68725e8f9b4cd" src="https://github.com/user-attachments/assets/d0c333a6-675e-438e-a361-24c6802ab53f" />
Q1. I initially tried to create a breathing glow effect for coins using Shader Graph, and built the node graph according to the teaching content of Week 8 of the course. This Shader Graph mainly uses several core nodes taught in the course: the Time node to get the game running time, the Sine node to generate smooth oscillating values, the Add node to implement the glow effect of color overlay, and the Sample Texture 2D node to sample the base texture of the coin. The overall logic is to drive the sine wave through time to produce brightness changes, and then overlay the glow color on the base texture of the coin to achieve the breathing flickering effect.

However, in practical application, I encountered an unsolvable material synchronization problem: although the node connections and property settings of Shader Graph were completed according to the teaching requirements, the material could never correctly apply the effect of Shader Graph, and there was no glow performance when running the game. After many failed attempts to fix it, I finally switched to handwritten HLSL code to implement exactly the same logic, and successfully made the coins generated after defeating enemies produce a purple breathing flickering glow.

At the same time, I also added a similar glow effect to the fired bullets, but because the bullets are small and move very fast, this effect is not very obvious visually, just a very tiny trailing light effect.

Q2. Based on the feedback from other students during playtesting, I mainly made three improvements to the coin system. First, I fixed the bug where some enemies would not drop coins after death; second, I enlarged the size of the coins by 66% to make them more obvious in the scene and easier for players to find and pick up; finally, I adjusted the rendering layer priority of the coins and set it to the highest level in the scene, ensuring that the coins are always displayed above the map terrain and decorative items and will not be blocked by any objects.

Q3. Since the last milestone submission, I have added three main contents to the game: first, implemented the purple breathing glow Shader effect for coins; second, added a very tiny trailing glow Shader effect for bullets; third, added more enemy spawn points in the scene to enrich the combat content of the game. The core combat loop of the game is now basically complete. In the future, I plan to set the final clearance goal of the game as: the player can successfully clear the level by collecting a certain number of coins by defeating the monsters in the scene.

## Milestone 4 Devlog
Milestone 4 Devlog goes here.

## Final Devlog
Q1. This is a 2D top-down pixel shooter where the player controls an adventurer fighting through a dungeon filled with wild boar enemies. The core gameplay loop is: defeat enemies using either ranged bullets or a melee knife, collect coins dropped by defeated enemies, and survive against waves of foes.
This vertical slice demonstrates what the full game would look like by implementing all core mechanics: player movement, two different attack types, enemy AI with pathfinding, a collectible system, and player health and death. It shows the core combat and progression loop that would be expanded in the full game, where players would explore larger dungeons, fight more varied enemies, and use collected coins to purchase upgrades.

This implementation partially aligns with the vertical slice plan I proposed in the Week 1 brainstorm. My original inspiration came from games like Soul Knight, Dead Cells, and Neon Abyss, and I planned to create a pixel-style top-down shooter centered around a complete "combat-collect-upgrade" loop. The current vertical slice has implemented the first two core parts of this loop: players can fight using two different weapons and defeat enemies to earn coins. The system for using coins to purchase weapons and skill upgrades will be implemented in future development. This version already demonstrates the core combat experience I originally envisioned and proves the feasibility of this gameplay concept.

Q2. I implemented a screen flash effect that activates when the player takes damage. This effect is triggered directly from gameplay logic in the player's TakeDamage() function. When the player is hit by an enemy, the function calls a coroutine that temporarily changes the color of a full-screen UI Image to a semi-transparent red, then fades it back to transparent over 0.15 seconds.

This is a rendering effect that modifies the final output of the game to provide immediate visual feedback to the player. The effect is only active for a short duration when the specific gameplay event of taking damage occurs, and it is implemented using C# code to control the UI element's color property.
Figure 1: Logic for triggering the rendering effect on hit event
<img width="2559" height="1368" alt="image" src="https://github.com/user-attachments/assets/7775aa3d-a9ad-4c94-aeab-2df5672cc008" />

Figure 2: Specific implementation of the red screen effect
<img width="2559" height="1368" alt="image" src="https://github.com/user-attachments/assets/66424fce-b887-4730-ad21-acc5d6056f1e" />

Additionally, I implemented a purple breathing glow effect for coins. I initially tried to implement this effect using Shader Graph, but encountered a known compilation bug in Unity 6.3 where the material would never correctly apply the effect despite correct node connections and property settings. After many failed attempts to fix it, I switched to handwritten HLSL code to implement exactly the same logic, and successfully made the coins produce a purple breathing flickering glow. This experience gave me a deeper understanding of how shaders work at a lower level.
<img width="2559" height="1368" alt="image" src="https://github.com/user-attachments/assets/e9f1695d-7d73-4b38-a56b-b9ec49387acd" />

Q3.
I broke down this freshman-year project into multiple independent, manageable systems, including the player controller, enemy AI and combat system, player weapon and bullet system, collectible system, and UI feedback system. I implemented one system at a time, thoroughly testing each component before moving on to the next to ensure stable functionality.

I plan to continue using the task step breakdown method we practiced this quarter in my future planning process. This approach has been extremely effective for me. As someone who wasn't familiar with the Unity engine at first, creating a 2D pixel shooter similar to Soul Knight seemed simple at first glance, but it was actually full of challenges. However, once I broke the entire project down into small, independent modules, the complex task became clear and manageable. It helped me track my progress more effectively, and seeing the game advance with each completed small module gave me great motivation.

Breaking down the project into small steps also helped me truly understand its scope. Initially, I thought completing this vertical slice would be very difficult. But as I completed each small task step by step, I found myself more clearly aware of what was missing from the project and what needed to be added to make the game more complete.

The process I described is exactly the workflow I followed when creating this vertical slice. I would first list all the small functional modules needed in the game, implement each one, test it in the game, and only start developing the next module after confirming it runs stably. This method helped me identify and fix bugs early before they became more complex, avoiding extensive rework later. In the future, I will continue to use this task-based breakdown approach, and I will also reserve more time at the end of the project for polishing details and fixing bugs to make the game experience even better.


## Open-source assets
[- Cite any external assets used here!]
(https://yqqs.huijiwiki.com/wiki/%E5%B0%8F%E6%80%AA#%E6%A3%80%E7%B4%A2)
