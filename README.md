# Unity Cheat Sheet

## Table of Contents

- [Basics](#basics)
  - [MonoBehaviour](#monobehaviour)
  - [Transform](#transform)
  - [Vector3](#vector3)
  - [Quaternion](#quaternion)
  - [Euler Angles](#euler-angles)
- [Movement & Rotation](#movement--rotation)
  - [Move Object](#move-object)
    - [Transform.Translate()](#transformtranslate)
    - [Vector3.MoveTowards()](#vector3movetowards)
    - [Vector3.Lerp()](#vector3lerp)
    - [Vector3.SmoothDamp()](#vector3smoothdamp)
  - [Rotate Object](#rotate-object)
    - [Transform.rotation](#transformrotation)
    - [Transform.eulerAngles](#transformeulerangles)
    - [Transform.Rotate()](#transformrotate)
    - [Transform.LookAt()](#transformlookat)
    - [Quaternion.LookRotation()](#quaternionlookrotation)
    - [Quaternion.FromToRotation()](#quaternionfromtorotation)
    - [Quaternion.ToAngleAxis()](#quaterniontoangleaxis)
- [Physics](#physics)
  - [Raycast](#raycast)
- [Input](#input)
  - [Keyboard](#keyboard)
  - [Mouse](#mouse)
  - [Touch](#touch)
- [Audio](#audio)
  - [Basic Audio Play](#basic-audio-play)
- [Design Patterns](#design-patterns)
  - [Singleton](#singleton)
- [Practical Use Cases](#practical-use-cases)
  - [Check if object is on the ground](#check-if-object-is-on-the-ground)
  - [Get the transform of a Body Bone](#get-the-transform-of-a-body-bone)

## Basics

### [MonoBehaviour](https://docs.unity3d.com/ScriptReference/MonoBehaviour.html)
[MonoBehaviour Life Cycle Flow Chart](https://docs.unity3d.com/uploads/Main/monobehaviour_flowchart.svg)
```csharp
// MonoBehaviour is the base class from which every Unity script derives.
// Offers some life cycle functions that are easier for you to develop your game.

// Some of the most frequently used ones are as follows;
// These will only run if the GameObject is enabled
![image](https://user-images.githubusercontent.com/9862287/130880435-b3db1c23-bc9f-4894-a945-30bb2155dfe8.png)

// Gets called once when the gameobject is enabled and never again. Good for setting variables.
Awake()

// Gets called once when the script is first enabled and never agai.
Start()

// Gets called in every frame (Limit using these)
Update()

// Gets called multiple times during a frame. Used with physics (Adding force to rigidbody etc)
FixedUpdate()

// Gets called in every frame but after Update() and before the camera renders.
LateUpdate()

// Gets called every frame for rendering and handling GUI events
OnGUI()

// Gets called everytime you use SetActive(false) on the script
OnDisable()

// Gets called everytime you use SetActive(true) on the script and before Start()
OnEnable()


// These are in Events > lifecycles;
```
![image](https://user-images.githubusercontent.com/9862287/123566962-2b760600-d815-11eb-806f-c7ab27bbeeb9.png)


### [Transform](https://docs.unity3d.com/ScriptReference/Transform.html)
```csharp
// Every object in a Scene has a Transform.
// It's used to store and manipulate the position, rotation and scale of the object.

transform.position.x = 5;
```
![image](https://user-images.githubusercontent.com/9862287/123567547-852b0000-d816-11eb-8cc0-8b7ff337038f.png)


### [Vector3](https://docs.unity3d.com/ScriptReference/Vector3.html)
```csharp
// Vector3 is representation of 3D vectors and points
// It's used to represent 3D positions,considering x,y & z axis.

Vector3 v = new Vector3(0f, 0f, 0f);
```
![image](https://user-images.githubusercontent.com/9862287/123869479-29cb5000-d985-11eb-9079-4c82ce3008b0.png)


### [Quaternion](https://docs.unity3d.com/ScriptReference/Vector3.html)
```csharp
// A Quaternion stores the rotation of the Transform in world space.
// Quaternions are based on complex numbers and don't suffer from gimbal lock.
// Unity internally uses Quaternions to represent all rotations.
// You almost never access or modify individual Quaternion components (x,y,z,w); 

// A rotation 30 degrees around the y-axis
Quaternion rotation = Quaternion.Euler(0, 30, 0);
```
![image](https://user-images.githubusercontent.com/9862287/123870016-da395400-d985-11eb-829a-17121cabda84.png)



### Euler Angles
```csharp
// Euler angles are "degree angles" like 90, 180, 45, 30 degrees.
// Quaternions differ from Euler angles in that they represent a point on a Unit Sphere (the radius is 1 unit).

// Create a quaternion that represents 30 degrees about X, 10 degrees about Y
Quaternion rotation = Quaternion.Euler(30, 10, 0);

// Using a Vector
Vector3 EulerRotation = new Vector3(30, 10, 0);
Quaternion rotation = Quaternion.Euler(EulerRotation);

// Convert a transform's Quaternion angles to Euler angles
Quaternion quaternionAngles = transform.rotation;
Vector3 eulerAngles = quaternionAngles.eulerAngles;
```
![image](https://user-images.githubusercontent.com/9862287/123875721-46b85100-d98e-11eb-81d2-5e3344e7cab7.png)


## Movement & Rotation

### Move Object
#### Transform.Translate()
```csharp
// Moves the transform in the direction and distance of translation.
public void Translate(Vector3 translation);
public void Translate(Vector3 translation, Space relativeTo = Space.Self);

transform.Translate(Vector3.right * movementSpeed);
```
![image](https://user-images.githubusercontent.com/9862287/123877495-6309bd00-d991-11eb-9a15-4df81aa4fd68.png)
![translate0](https://user-images.githubusercontent.com/9862287/123880569-0e694080-d997-11eb-9e4d-f843bf0a60fb.gif)




#### Vector3.MoveTowards()
```csharp
// Calculate a position between the points specified by current and target
// Moving no farther than the distance specified by maxDistanceDelta
public static Vector3 MoveTowards(Vector3 current, Vector3 target, float maxDistanceDelta);

Vector3 targetPosition;
transform.position = Vector3.MoveTowards(transform.position, targetPosition, Time.deltaTime);
```
![image](https://user-images.githubusercontent.com/9862287/123880976-dc0c1300-d997-11eb-98e8-d5e0404f39a6.png)
![translate2](https://user-images.githubusercontent.com/9862287/123879278-c517f180-d994-11eb-9eb6-5c123c7789c5.gif)

#### Vector3.Lerp()
```csharp
// Linearly interpolates between two points. Results in a smooth transition.
public static Vector3 Lerp(Vector3 startValue, Vector3 endValue, float interpolationRatio);

Vector3 targetPosition;
float t = 0;
t += Time.deltaTime * speed;
transform.position = Vector3.Lerp(transform.position, targetPosition, t);
```
![image](https://user-images.githubusercontent.com/9862287/123886982-bd604900-d9a4-11eb-9f7a-e30d00d08df1.png)
![lerp](https://user-images.githubusercontent.com/9862287/123885254-2b0a7600-d9a1-11eb-92fe-e2229ae117ba.gif)



#### Vector3.SmoothDamp()
```csharp
// Gradually changes a vector towards a desired goal over time.
// The vector is smoothed by some spring-damper like function, which will never overshoot.
// The most common use is for smoothing a follow camera.
public static Vector3 SmoothDamp(Vector3 current, Vector3 target, ref Vector3 currentVelocity, float smoothTime, float maxSpeed = Mathf.Infinity, float deltaTime = Time.deltaTime);
```
![image](https://user-images.githubusercontent.com/9862287/123891341-e258ba00-d9ac-11eb-93a4-18d572b33c8e.png)

```csharp
float smoothTime = 1f;
Vector3 velocity;
Vector3 targetPosition = target.TransformPoint(new Vector3(0, 5, -10));
// Smoothly move the camera towards that target position
transform.position = Vector3.SmoothDamp(transform.position, targetPosition, ref velocity, smoothTime);
```
![image](https://user-images.githubusercontent.com/9862287/123891290-d1a84400-d9ac-11eb-8316-812bb644ab11.png)

![smoothdamp](https://user-images.githubusercontent.com/9862287/123891102-855d0400-d9ac-11eb-90ac-a2656125cf3c.gif)


### Rotate Object
#### Transform.rotation
```csharp
// A Quaternion stores the rotation of the Transform in world space.
// Quaternions are based on complex numbers and don't suffer from gimbal lock.
// Unity internally uses Quaternions to represent all rotations.

transform.rotation = new Quaternion(rotx, roty, rotz, rotw);
```
![image](https://user-images.githubusercontent.com/9862287/123898391-5d27d200-d9b9-11eb-9ec1-742f866e0598.png)


#### Transform.eulerAngles
```csharp
// Transform.eulerAngles represents rotation in world space. 
// It is important to understand that although you are providing X, Y, and Z rotation values to describe your rotation
// those values are not stored in the rotation. Instead, the X, Y & Z values are converted to the Quaternion's internal format.

transform.eulerAngles = Vector3(rotx, roty, rotz);
```
![image](https://user-images.githubusercontent.com/9862287/123898600-b7289780-d9b9-11eb-830b-bf7f26290d87.png)


#### Transform.Rotate()
```csharp
// Applies rotation around all the given axes.
public void Rotate(Vector3 eulers, Space relativeTo = Space.Self);
public void Rotate(float xAngle, float yAngle, float zAngle, Space relativeTo = Space.Self);

transform.Rotate(rotx, roty, rotz);
```

#### Transform.LookAt()
```csharp
// Points the positive 'Z' (forward) side of an object at a position in world space.
public void LookAt(Transform target);
public void LookAt(Transform target, Vector3 worldUp = Vector3.up);

// Rotate the object's forward vector to point at the target Transform.
Transform target;
transform.LookAt(target);

// Same as above, but setting the worldUp parameter to Vector3.left in this example turns the object on its side.
transform.LookAt(target, Vector3.left);
```

#### Quaternion.LookRotation()
```csharp
// Creates a rotation with the specified forward and upwards directions.
public static Quaternion LookRotation(Vector3 forward, Vector3 upwards = Vector3.up);

// The following code rotates the object towards a target object.
Vector3 direction = target.position - transform.position;
Quaternion rotation = Quaternion.LookRotation(direction);
transform.rotation = rotation;
```

#### Quaternion.FromToRotation()
```csharp
// Creates a rotation (a Quaternion) which rotates from fromDirection to toDirection.
public static Quaternion FromToRotation(Vector3 fromDirection, Vector3 toDirection);

// Sets the rotation so that the transform's y-axis goes along the z-axis.
transform.rotation = Quaternion.FromToRotation(Vector3.up, transform.forward);
```

#### Quaternion.ToAngleAxis()
```csharp
// Converts a rotation to angle-axis representation (angles in degrees).
// In other words, extracts the angle as well as the axis that this quaternion represents.
public void ToAngleAxis(out float angle, out Vector3 axis);

// Extracts the angle - axis rotation from the transform rotation
float angle = 0.0f;
Vector3 axis = Vector3.zero;
transform.rotation.ToAngleAxis(out angle, out axis);
```

## Physics
### Raycast

```csharp
void FixedUpdate() {
    // Bit shift the index of the layer (8) to get a bit mask
    int layerMask = 1 << 8;

    // This would cast rays only against colliders in layer 8.
    // But instead we want to collide against everything except layer 8. The ~ operator does this, it inverts a bitmask.
    layerMask = ~layerMask;

    RaycastHit hit;
    // Does the ray intersect any objects excluding the player layer
    if (Physics.Raycast(transform.position, transform.TransformDirection(Vector3.forward), out hit, Mathf.Infinity, layerMask)) {
        Debug.DrawRay(transform.position, transform.TransformDirection(Vector3.forward) * hit.distance, Color.yellow);
        Debug.Log("Did Hit");
    }
}
```

## Input

### Keyboard

```csharp
// Returns true during the frame the user starts pressing down the key
if (Input.GetKeyDown(KeyCode.Space)) {
    Debug.Log("Space key was pressed");
}

// Jump is also set to space in Input Manager
if (Input.GetButtonDown("Jump")) {
    Debug.Log("Do something");
}
```

### Mouse

```csharp
if (Input.GetAxis("Mouse X") < 0) {
    Debug.Log("Mouse moved left");
}

if (Input.GetAxis("Mouse Y") > 0) {
    Debug.Log("Mouse moved up");
}

if (Input.GetMouseButtonDown(0)) {
    Debug.Log("Pressed primary button.");
}

if (Input.GetMouseButtonDown(1)) {
    Debug.Log("Pressed secondary button.");
}

if (Input.GetMouseButtonDown(2)) {
    Debug.Log("Pressed middle click.");
}
```

### Touch
```csharp
if (Input.touchCount > 0) {
    touch = Input.GetTouch(0);

    if (touch.phase == TouchPhase.Began) {
        Debug.Log("Touch began");
    }

    if (touch.phase == TouchPhase.Moved) {
        Debug.Log("Touch moves");
    }

    if (touch.phase == TouchPhase.Ended) {
        Debug.Log("Touch ended");
    }
}
```

## Audio

### Basic Audio Play

```csharp
public class PlayAudio : MonoBehaviour {
    public AudioSource audioSource;

    void Start() {
        // Calling Play on an Audio Source that is already playing will make it start from the beginning
        audioSource.Play();
    }
}
```


## Design Patterns

### Singleton

```csharp
// Define singleton class
public class SingletonClass: MonoBehaviour {
    private static SomeClass instance;

    public static SomeClass Instance { get { return instance; } }

    private void Awake() {
        if (instance != null && instance != this) {
            Destroy(this.gameObject);
        } else {
            instance = this;
        }
    }
}

// Use it in another class
public class AnotherClass: MonoBehaviour {
    public Singleton instance;

    private void Awake() {
       instance = Singleton.Instance;
    }
}
```

## Practical Use Cases

### Check if object is on the ground

```csharp
RaycastHit hit;

// Unlike this example, most of the time you should pass a layerMask as the last option to hit only to the ground
if (Physics.Raycast(transform.position, -Vector3.up, out hit, 0.5f)) {
   Debug.log("Hit something below!");
}
```

### Get the transform of a Body Bone

```csharp
Animator animator;

Transform transform = animator.GetBoneTransform(HumanBodyBones.Head);
```

### Change Location of object

![image](https://user-images.githubusercontent.com/9862287/128634772-d3fd1fe0-5606-4e29-8b81-9f6cb519b2a3.png)


