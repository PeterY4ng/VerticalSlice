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
1. The complicating gameplay feature I developed for this Milestone is the dual-weapon combat system and finite state machine-based intelligent enemy AI. This feature is newly added on the basis of MS1 and was not written in class. It provides a complete combat loop for the game: players can use two weapons, a ranged pistol and a melee knife, to interact with enemies that have independent patrol, chase, return and attack behaviors.
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
## Milestone 3 Devlog
Milestone 3 Devlog goes here.
## Milestone 4 Devlog
Milestone 4 Devlog goes here.
## Final Devlog
Final Devlog goes here.
## Open-source assets
- Cite any external assets used here!
