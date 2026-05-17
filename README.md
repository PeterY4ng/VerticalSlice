# GDIM33 Vertical Slice
## Milestone 1 Devlog
<img width="2559" height="1368" alt="image" src="https://github.com/user-attachments/assets/c2ad7fbb-f6d7-4736-803d-67a9688933b2" />
<img width="2559" height="1368" alt="image" src="https://github.com/user-attachments/assets/27ed09af-943f-4494-9c27-e6ca1f5dcdf3" />

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



## Milestone 3 Devlog
Milestone 3 Devlog goes here.
## Milestone 4 Devlog
Milestone 4 Devlog goes here.
## Final Devlog
Final Devlog goes here.
## Open-source assets
- Cite any external assets used here!
