# How to Create a Stickman Running Game with Unity

## Overview
This game was made with [Unity](https://unity.com) using [Visual Studio Code](https://code.visualstudio.com/) to program it. Unity is scripted in [C#](https://unity.com/how-to/programming-unity).

## Unity Setup
Open up Unity Hub, and add Visual Studio Code to your modules.
![image](https://github.com/user-attachments/assets/a229047a-fe57-4441-9aea-0ae6f3f619a7)

Create a New project and select Universal 2D for our 2D Stickman Running Game. 
![image](https://github.com/user-attachments/assets/e354a838-019a-49cf-baf8-d9f2e550e98f)


### Sprite Setup
Sprites are the game's objects and characters which will be an image to give to GameObject. We will be creating our stickman and box sprites for the game, so create your own sprites or download sprites from the internet. Once you have imported your sprites, create two GameObjects and add a component called Sprite Renderer which you will drag your sprites to.
![image](https://github.com/user-attachments/assets/7623cad3-8cb1-4e1f-94e5-af3414592abd)

For both of these GameObjects, Stickman and Box, you will be adding more components, Rigidbody 2d, Collider, and a Script. 
![image](https://github.com/user-attachments/assets/43b2bb07-5005-4a05-8c76-88a13211d7c9)


### Unity Code
When you first create your script for your sprite, there will be two methods loaded in. ```Start()``` is called at the very beginning before the first frame. ```Update``` is called once per frame.
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

### StickManScript
Stickman's only move is to jump up. We store the component Rigidbody2D into myRigidbody with ```public Rigidbody2D myRidgidBody;``` and create a variable for the jump height with ```public float jumpStrength```. The component Rigidbody2D controls the physics behind the GameObject, in this case it will be the jump height and gravity of Stickman. ```public``` allows the variable to be accessed from other scripts and also allows you to edit the value in Unity without modifying the script. We don't need anything in ```Start()``` because jumping is a player controlled action so it belongs in the ```Update()```. In ```Update()``` we create an ```if``` statement to detect when a player presses the keys "space" and "up arrow" to indicate a jump. To create the jump movement we call myRigidbody and give it linear Velocity in the up direction multiplied by jumpStrength to control the height of the jump.
```
using UnityEngine;

public class StickmanScript : MonoBehaviour
{
    public Rigidbody2D myRidgidbody;
    public float jumpStrength;
    void Start()
    {

    }


    void Update()
    {
        if (Input.GetKeyDown(KeyCode.Space) == true)
        {
            myRidgidbody.linearVelocity = Vector2.up * jumpStrength;
        }
        if (Input.GetKeyDown(KeyCode.UpArrow) == true)
        {
            myRidgidbody.linearVelocity = Vector2.up * jumpStrength;
        }
    }
}

```

### BoxMove
```
using UnityEngine;

public class BoxMoveScript : MonoBehaviour
{
    public float moveSpeed = 5;
    void Start()
    {
        
    }

    void Update()
    {
        transform.position = transform.position + (Vector3.left * moveSpeed) * Time.deltaTime;
    }
}

```

### BoxSpawner
```
using System.Threading;
using UnityEngine;

public class FloorBoxSpawner : MonoBehaviour
{
    public GameObject FloorBox;
    public float spawnRate = 100;
    private float timer = 0;
    public float spawnHeight = 10;

    void Start()
    {
        
    }

    void Update()
    {

        if(timer < spawnRate)
        {
            timer += Time.deltaTime;
        }
        else
        {
            spawnFloorBox();
            timer = 0;
        }

    }

    void spawnFloorBox()
    {
        float lowestPoint = transform.position.y - spawnHeight;
        float highestPoint = transform.position.y + spawnHeight;
        Instantiate(FloorBox, new Vector3(transform.position.x, Random.Range(lowestPoint, highestPoint), 0), transform.rotation);
    }
}

```
