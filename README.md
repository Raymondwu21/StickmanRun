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

### Coding the Game
```

```

### StickManScript
```

```

### BoxMove
```

```

### BoxSpawner
```

```
