# How to Create a Stickman Running Game with Unity

## Introduction
This project is a **2D Stickman Running Game** built using [Unity](https://unity.com) and [Visual Studio Code](https://code.visualstudio.com/) to program it. Players control a stickman as they jump over obstacles and survive as long as possible. This project is designed for beginners to learn the basics of Unity game development.

## Overview
- Developed with **Unity and C#**
- Features **infinite runner mechanics**
- Implements **jumping, obstacles, and a scoring system**
- Includes a **game over system** when hitting obstacles

## Prerequisites
- Unity (latest version)
- Basic knowledge of C#
- A computer with Windows/Mac

## Unity Setup
**1.** Open **Unity Hub**, go to **Installs**, select your Unity version, and **add Visual Studio Code as a module**.  
![image](https://github.com/user-attachments/assets/a229047a-fe57-4441-9aea-0ae6f3f619a7)

**2.** Create a New Project
- Open Unity Hub → Click **"New Project"**
- Select Universal 2D for your 2D Stickman Running Game. 
![image](https://github.com/user-attachments/assets/e354a838-019a-49cf-baf8-d9f2e550e98f)


### Sprite Setup
Sprites are the **visual representations of game objects**. For this game, we need:
- **Stickman character**
- **Ground (floor)**
- **Box (obstacle)**
**1.** Import Sprites
 - **Download or create your own** sprites.
 - **Import them into Unity**:  
   - Go to **Assets → Import New Asset**  
   - Select the sprite images.
**2.** Create GameObjects
   - Create **three GameObjects**:
     - **Stickman**
     - **Ground**
     - **Box (obstacle)**
   - Attach a **Sprite Renderer** component to each.
     - Drag the correct sprite into the **Sprite Renderer**.
**3.** Add Physics Components
For each GameObject:
   - Add **Rigidbody2D** (for physics-based movement)
   - Add **BoxCollider2D** (for collision detection)
![image](https://github.com/user-attachments/assets/7623cad3-8cb1-4e1f-94e5-af3414592abd) ![image](https://github.com/user-attachments/assets/048843a5-54af-4140-99ad-c2645ceea18f)


![image](https://github.com/user-attachments/assets/43b2bb07-5005-4a05-8c76-88a13211d7c9)


### Unity Scripting (C#)
When you first create your script for your sprite, there will be two methods loaded in. ```Start()``` is called at the very beginning before the first frame. ```Update()``` is called once per frame.
```
using UnityEngine;

public class StickmanScript : MonoBehaviour
{
    void Start()
    {

    }

    void Update()
    {

    }
}
```
## Visual Studio Code
There are 5 classes needed for this game to function properly, stickman's functionality, box movements, box spawning, box deletion, and a logic script.
### StickManScript.cs
The **StickmanScript.cs** controls the **player's movement**, specifically jumping and falling, while also handling **collisions** with the ground and obstacles. The script ensures that:
- The **Stickman can jump** when touching the ground.
- The **Stickman falls faster** when pressing the **Down Arrow**.
- The **game ends** when the Stickman **collides with the left side of an obstacle**.
#### Step 1) Declare the Variables
```
using UnityEngine;

public class StickmanScript : MonoBehaviour
{
    public Rigidbody2D myRidgidbody;
    public float jumpStrength;
    public float downStrength;
    private bool isGrounded;
    private LogicScript logic;
```
- ```Rigidbody2D myRidgidbody;``` → Stores the Rigidbody2D component, allowing Unity's physics engine to handle movement.
- ```float jumpStrength;``` → Controls how high the Stickman jumps.
- ```float downStrength;``` → Controls how fast the Stickman falls when the Down Arrow is pressed.
- ```bool isGrounded;``` → Keeps track of whether the Stickman is on the ground (prevents double jumps).
- ```LogicScript logic;``` → Reference to the LogicScript for triggering the Game Over screen.
#### Step 2) Initialize Variables (```Start()```Method)
```
    void Start()
    {
        isGrounded = false;

        // Find Logic Manager
        GameObject logicManager = GameObject.FindGameObjectWithTag("Logic");
        if (logicManager != null)
        {
            logic = logicManager.GetComponent<LogicScript>();
        }
        else
        {
            Debug.LogError("❌ ERROR: Logic Manager not found! Make sure it has the tag 'Logic'.");
        }
    }
```
- The ```sGrounded``` variable is initialized as false, meaning the Stickman starts in the air.
- ```GameObject.FindGameObjectWithTag("Logic")``` searches for a GameObject tagged as "Logic" in the scene.
- If found, it gets the ```LogicScript``` component so we can later call ```logic.gameOver()``` to end the game.
- If not found, an error message is displayed in the Unity Console.

#### Step 3) Handling Player Input (```Update()``` Method)
```
    void Update()
    {
        if (isGrounded && (Input.GetKeyDown(KeyCode.Space) || Input.GetKeyDown(KeyCode.UpArrow)))
        {
            myRidgidbody.linearVelocity = Vector2.up * jumpStrength;
            isGrounded = false; // Prevent double jumps
        }

        if (Input.GetKeyDown(KeyCode.DownArrow))
        {
            myRidgidbody.linearVelocity = Vector2.down * downStrength;
        }
    }
```
- The Stickman can only jump when ```isGrounded == true```.
- Pressing ```Space``` or ```Up Arrow``` applies an upward force (```Vector2.up * jumpStrength```), making the Stickman jump.
- After jumping, ```isGrounded``` is set to ```false``` to prevent multiple jumps in mid-air.
- Pressing ```Down Arrow``` applies a downward force (```Vector2.down * downStrength```), making the Stickman fall faster.

#### Step 4) Collision Handling (```OnCollisionEnter2D()``` Method)
```
    void OnCollisionEnter2D(Collision2D collision)
    {
        if (collision.gameObject.CompareTag("Ground") || collision.gameObject.CompareTag("Box"))
        {
            isGrounded = true;
        }

        if (collision.gameObject.CompareTag("Box")) // Ensure it's a Box
        {
            Debug.Log("✅ Stickman collided with a Box!");

            ContactPoint2D[] contactPoints = new ContactPoint2D[collision.contactCount];
            collision.GetContacts(contactPoints);

            foreach (ContactPoint2D contact in contactPoints)
            {
                Debug.Log("👉 Contact Normal: " + contact.normal);

                if (contact.normal.x < -0.5f) // Left side hit detected
                {
                    Debug.Log("❌ Game Over! Stickman hit the left side of the Box.");
                    logic.gameOver(); // Trigger Game Over
                    Time.timeScale = 0f; // Pause game
                    break;
                }
            }
        }
    }
```
- If the Stickman lands on the ground (```"Ground"```) or a Box (```"Box"```), it is considered grounded (```isGrounded = true```).
- If the Stickman collides with a Box, it checks where the collision occurred.
- ```collision.GetContacts(contactPoints);``` retrieves all contact points of the collision.
- ```contact.normal.x < -0.5f```
  - This means the Stickman hit the left side of the Box.
  - If ```true```, it triggers Game Over (```logic.gameOver();```) and pauses the game (```Time.timeScale = 0f;```).

#### Step 5) Handling Exit Collisions (```OnCollisionExit2D()``` Method)
```
    void OnCollisionExit2D(Collision2D collision)
    {
        if (collision.gameObject.CompareTag("Ground"))
        {
            isGrounded = false;
        }
    }
}
```
- If the Stickman leaves the ground (```"Ground"```), isGrounded is set to ```false```.
- This prevents the player from double jumping.

### FloatBoxMoveScript.cs
```
using UnityEngine;

public class FloatBoxMoveScript : MonoBehaviour
{
    public float moveSpeed = 5f;
    public float speedIncrease = 0.2f;
    public float maxSpeed = 10f;
    private LogicScript logicScript;

    void Start()
    {
        Debug.Log("✅ FloatBoxMoveScript has started!");
        logicScript = GameObject.FindFirstObjectByType<LogicScript>();

        if (logicScript == null)
        {
            Debug.LogError("❌ ERROR: LogicScript NOT found! Make sure 'Logic Manager' is in the scene.");
        }
    }

    void Update()
    {
        moveSpeed = Mathf.Min(moveSpeed + speedIncrease * Time.deltaTime, maxSpeed);
        transform.position += Vector3.left * moveSpeed * Time.deltaTime;

        if (transform.position.x < -10f)
        {
            Debug.Log("✅ Box moved off-screen!");
            if (logicScript != null)
            {
                logicScript.addScore(); // Increase score
            }
            Destroy(gameObject);
        }
    }
}

```

### BoxSpawner.cs
```
using UnityEngine;

public class BoxSpawner : MonoBehaviour
{
    [Header("Box Prefabs")]
    public GameObject FloorBox;
    public GameObject FloatBox;

    [Header("Spawn Settings")]
    public float spawnRate = 2f;
    private float timer = 0f;

    [Header("Adjustable Spawn Positions")]
    public float floorBoxX = 0f;
    public float floorBoxY = -3f;

    public float floatBoxX = 0f;
    public float minSpawnHeight = 2f;
    public float maxSpawnHeight = 5f;

    void Update()
    {
        if (timer < spawnRate)
        {
            timer += Time.deltaTime;
        }
        else
        {
            SpawnBox();
            timer = 0f;
        }
    }

    void SpawnBox()
    {
        GameObject boxToSpawn;
        float spawnX, spawnY;

        if (Random.value < 0.5f)
        {
            boxToSpawn = FloorBox;
            spawnX = floorBoxX;
            spawnY = floorBoxY;
        }
        else
        {
            boxToSpawn = FloatBox;
            spawnX = floatBoxX;
            spawnY = Random.Range(minSpawnHeight, maxSpawnHeight);
        }

        Instantiate(boxToSpawn, new Vector3(spawnX, spawnY, 0), Quaternion.identity);
    }
}
```

### BoxDestroyer.cs
```
using UnityEngine;

public class BoxDestroyer : MonoBehaviour
{
    private LogicScript logicScript;

    void Start()
    {
        logicScript = GameObject.FindFirstObjectByType<LogicScript>();

        if (logicScript == null)
        {
            Debug.LogError("❌ ERROR: LogicScript NOT found! Make sure 'Logic Manager' is in the scene.");
        }
    }

    void OnTriggerEnter2D(Collider2D other)
    {
        if (logicScript != null)
        {
            logicScript.addScore(); // Increase score
            Debug.Log("✅ Box entered destroy zone → Score should increase.");
        }
        Destroy(other.gameObject);
    }
}
```

### LogicScript.cs
```
using UnityEngine;
using UnityEngine.UI;
using UnityEngine.SceneManagement;

public class LogicScript : MonoBehaviour
{
    public int playerScore;
    public Text scoreText;
    public GameObject gameOverScreen;

    void Start()
    {
        Debug.Log("✅ LogicScript has started!");
    }

    [ContextMenu("Increase Score")]
    public void addScore()
    {
        playerScore += 1;
        scoreText.text = playerScore.ToString();
        Debug.Log("✅ Score Increased: " + playerScore);
    }

    public void restartGame()
    {
        Debug.Log("🔄 Restarting Game...");
        Time.timeScale = 1f; // ✅ Ensure game resumes before restarting
        SceneManager.LoadScene(SceneManager.GetActiveScene().buildIndex);
    }

    public void gameOver()
    {
        Debug.Log("💀 Game Over Screen Activated!");
        if (gameOverScreen != null)
        {
            gameOverScreen.SetActive(true);
        }
        else
        {
            Debug.LogError("❌ ERROR: GameOverScreen not assigned in LogicScript!");
        }

        Time.timeScale = 0f; // Pause the game
    }
}
```

### Features to Add Next
- Sound effects (jumping, colliding, etc.)
- Background Music
- UI Improvements
- High Score System
