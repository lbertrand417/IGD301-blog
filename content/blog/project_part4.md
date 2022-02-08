---
title: "Project Part 4"
date: 2022-02-04
draft: false
description: "Locomotion technique: Teleportation ball"
tags: ["Locomotion", "VR", "ball", "teleportation"]
thumbnail: http://lbertrand417.github.io/IGD301-blog/thumbnail_project1.png
---

Today I planned to correct the trigger problem (you can bring the ball back in your hand or change it even when the ball is thrown). 
For that, I added a boolean which is true when the ball is thrown and which blocks the AttachBall() and ChangeBall() functions.  

I also tried to allow the user to change the hand. My idea is that each controller has a MySelect.cs script and that in LocomotionTechnique 
we disable/enable one or the other depending on the user's actions. To change the hand, the principle is the same as to change the ball. 
If the player uses the left thumbstick then the ball will appear in the left hand and vice versa. I have also statically passed the balls array 
and the currentBall variable since the value is shared by both scripts. However, it didn't work. The ball changed hands the first time but then 
I couln't change the ball or change hands again. I thought it might be because I was using the thumbstick to change hands and balls at the same time so 
I tried to change hands by pressing the B button. But the result was the same. Even when I displayed the value of currentBall during the game, 
I couldn't detect when and why the code crashed (because it seems I couldn't do anything else because the code crashed). I found out that the code was crashing
in the AttachBall() function but nothing more. It's possible that the error came from the array, 
I guess it must have been empty for some reason. Finally, I gave up, so my game has only a right hand version. I'm a little disappointed because 
I thought this feature was interesting.

Instead, I focused on a more important problem, the bouncing ball. I didn't really know how to deal with the kind of elasticity of the ball that allows 
it to bounce but after some research I discovered the existence of physics material. So I added a physic material to my bouncing ball to make it bounce. 

![Demo bouncy ball](http://lbertrand417.github.io/IGD301-blog/demo_bouncy_ball.gif)


I also changed the assets of the sticky ball by making it slightly transparent and by giving it a bit of a slimey feel. I also set the ball speeds again so that 
the bouncing ball can go very far (but not too far either) and the sticky ball can go less far than the petanque ball. I realize that having a sticky ball that 
can't go as far as the petanque ball makes things a little less physically coherent (because it feels like the sticky ball is heavier than the petanque ball) but 
I didn't want the sticky ball to be "too efficient". Indeed, it is more precise than the petanque ball so if it can go further, the petanque ball is useless.

![Demo sticky ball](http://lbertrand417.github.io/IGD301-blog/demo_sticky_ball.gif)

While testing throughout this session (and a bit in the previous sessions) I realized that waiting for the end of the throw was very long and made 
the locomotion quite boring. So I finally put back the system that allows to "catch" or change the ball in mid-air. 
I find that it makes the process more dynamic and avoids ending up outside the parkour in case of a bad throw. It also makes it easier to catch coins.

![Demo coin trick](http://lbertrand417.github.io/IGD301-blog/demo_coin_trick.gif)

Finally, I removed the FOV reduction which is not useful here because it is a teleportation, and I added a display which informs the player about the ball he has in hand. 
Indeed, it can be complicated to understand what the ball is doing just by looking at it and, moreover, one does not necessarily look at his hand when changing the ball. 

![Switch ball](http://lbertrand417.github.io/IGD301-blog/switch_ball.gif)

In the same way, a small tutorial appears at the beginning of the game to inform the user about the functioning of the locomotion technique. 
I consciously avoided telling the user that he could cancel a throw because I want this to be a property that he finds by himself while playing. 

![Didacticiel](http://lbertrand417.github.io/IGD301-blog/intro_display.png)

Well, now we have a functional locomotion technique! We just have to fix the last bugs and it will be finished!

One bug I noticed concerns the ball change (again). I had said before that I was using Mathf.Abs to avoid having negative hints. 
But in fact I realized that the order of the balls was not right when scrolling to the left. The coefficients were -2, -1, 0, 1, 2 instead of -1, -2, 0, 1, 2. 
That's why I removed the Mathf.Abs and instead used a condition to know which way to go. 

```c++
if (currentBall == 0 && increment == -1)
{
	currentBall = balls.Length - 1;
} else
{
	currentBall = (currentBall + increment) % balls.Length;
}
```


Another bug I noticed is that the bouncing ball 
sometimes stops in the air (because the stop condition is based on the speed and it is possible that it reaches this speed during one of the bounces). 
However, the y-position of the user depends on the y-position of the ball. In these moments, we find ourselves flying. To solve this problem I wanted to count 
the number of collisions between the ball and the ground and take for the y-component the value at the last collision. But in fact, contrary to what I thought, 
the Collider does not count the number of collisions between two same objects but only between an object and all the other objects it meets. 
So my solution was not suitable and I went back to the old version (which is satisfactory in most cases).

Well, this concludes my project! Below you can find my complete code.

MySelect.cs

```c++
using System.Collections;
using System.Collections.Generic;
using TMPro;
using UnityEngine;

public class MySelect : MonoBehaviour
{
    public GameObject[] balls = new GameObject[3];
    public int currentBall = 0;

    public LocomotionTechnique locomotion;

    public OVRInput.Controller controller;
    private float triggerValue;

    private bool isThrowing = false;
    private bool changeBall = true;

    private Vector3 m_anchorOffsetPosition;
    private Quaternion m_anchorOffsetRotation;

    private Vector3 m_grabbedObjectPosOff;


    // Start is called before the first frame update
    void Start()
    {
        // Retrieve balls
        balls[0] = GameObject.Find("petanque");
        balls[1] = GameObject.Find("sticky");
        balls[1].SetActive(false);
        balls[2] = GameObject.Find("bouncy");
        balls[2].SetActive(false);

        // Initialize current ball
        locomotion.currentBall = balls[0];
        currentBall = 0;
    }
    
    protected virtual void Awake()
    {
        m_anchorOffsetPosition = transform.localPosition;
        m_anchorOffsetRotation = transform.localRotation;
    }

    // Update is called once per frame
    void Update()
    {

        // Remove start text
        bool AButton = OVRInput.Get(OVRInput.Button.One);

        if (AButton)
        {
            locomotion.parkourCounter.startTextGO.SetActive(false);
        }

        // Throw ball
        triggerValue = OVRInput.Get(OVRInput.Axis1D.PrimaryIndexTrigger, controller);

        if (triggerValue > 0.95f)
        {
            if (balls[currentBall].transform.parent == null)
            {
                AttachBall();
            }
            isThrowing = true;
        }

        if (isThrowing && triggerValue < 0.95f) { 
            if (OVRInput.GetLocalControllerVelocity(controller).magnitude > 1f)
            {
                balls[currentBall].transform.parent = null;
                Rigidbody rb = balls[currentBall].GetComponent<Rigidbody>();
                rb.useGravity = true;
                rb.isKinematic = false;

                OVRPose localPose = new OVRPose { position = OVRInput.GetLocalControllerPosition(controller), orientation = OVRInput.GetLocalControllerRotation(controller) };
                OVRPose offsetPose = new OVRPose { position = m_anchorOffsetPosition, orientation = m_anchorOffsetRotation };
                localPose = localPose * offsetPose;

                OVRPose trackingSpace = transform.ToOVRPose() * localPose.Inverse();
                Vector3 linearVelocity = trackingSpace.orientation * OVRInput.GetLocalControllerVelocity(controller);
                Vector3 angularVelocity = trackingSpace.orientation * OVRInput.GetLocalControllerAngularVelocity(controller);
                rb.velocity = balls[currentBall].GetComponent<Ball>().speedFactor * linearVelocity;
                rb.angularVelocity = angularVelocity;
            }

            isThrowing = false;
        }

        // Change ball
        Vector2 primaryAxis = OVRInput.Get(OVRInput.Axis2D.PrimaryThumbstick, controller);

        if (primaryAxis.x > 0.95f && !isThrowing && changeBall)
        {
            StartCoroutine(ChangeBall(1));
                        
        }

        if (primaryAxis.x < -0.95f && !isThrowing && changeBall)
        {
            StartCoroutine(ChangeBall(-1));
        }
    }

    private IEnumerator ChangeBall(int increment)
    {
        changeBall = false;
        balls[currentBall].SetActive(false);
        if (currentBall == 0 && increment == -1)
        {
            currentBall = balls.Length - 1;
        } else
        {
            currentBall = (currentBall + increment) % balls.Length;
        }
        balls[currentBall].SetActive(true);
        locomotion.currentBall = balls[currentBall];
        AttachBall();

        locomotion.parkourCounter.ballTextGO.SetActive(true);
        Ball myBall = balls[currentBall].GetComponent<Ball>();
        if (myBall.type == Ball.Type.petanque)
        {
            locomotion.parkourCounter.ballTextGO.GetComponent<TMP_Text>().text = "Petanque ball";
        }
        else if (myBall.type == Ball.Type.sticky)
        {
            locomotion.parkourCounter.ballTextGO.GetComponent<TMP_Text>().text = "Sticky ball";
        }
        else if (myBall.type == Ball.Type.bouncy)
        {
            locomotion.parkourCounter.ballTextGO.GetComponent<TMP_Text>().text = "Bouncy ball";
        }

        yield return new WaitForSeconds(0.5f);
        locomotion.parkourCounter.ballTextGO.SetActive(false);
        changeBall = true;
    }

    public void AttachBall()
    {
        balls[currentBall].transform.parent = this.transform;

        Vector3 relPos = balls[currentBall].transform.position - transform.position;
        relPos = Quaternion.Inverse(transform.rotation) * relPos;
        m_grabbedObjectPosOff = relPos;
        Vector3 grabbablePosition = transform.position + transform.rotation * m_grabbedObjectPosOff;

        balls[currentBall].transform.localPosition = new Vector3(0.0f, 0.0f, 0.0f);
        Rigidbody rb = balls[currentBall].GetComponent<Rigidbody>();
        rb.position = grabbablePosition;
        rb.isKinematic = true;
        rb.useGravity = false;
        rb.velocity = Vector3.zero;
        rb.angularVelocity = Vector3.zero;
    }
}

```

Ball.cs

```c++
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Ball : MonoBehaviour
{
    public enum Type
    {
        petanque,
        sticky,
        bouncy
    };

    public Type type;
    public float speedFactor = 1;
    public float velocityThreshold = 0.5f;

    public ParkourCounter parkourCounter;

    // Start is called before the first frame update
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        Rigidbody rb = this.GetComponent<Rigidbody>();
        if (rb.velocity.magnitude < velocityThreshold)
        {
            rb.velocity = Vector3.zero;
        }
    }

    private void OnCollisionEnter(Collision collision)
    {
        Rigidbody rb = this.GetComponent<Rigidbody>();
        if (type == Type.sticky || (type == Type.bouncy && rb.velocity.magnitude < velocityThreshold))
        {
            rb.velocity = Vector3.zero;
        }
    }

    void OnTriggerEnter(Collider other)
    {
        if (other.CompareTag("banner"))
        {
            this.GetComponent<AudioSource>().Play();
            parkourCounter.locomotionTech.stage = other.gameObject.name;
            parkourCounter.isStageChange = true;
        } else if (other.CompareTag("coin"))
        {
            parkourCounter.coinCount += 1;
            this.GetComponent<AudioSource>().Play();
            other.gameObject.SetActive(false);
        }
        // These are for the game mechanism.

    }
}

```

LocomotionTechnique.cs

```c++
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class LocomotionTechnique : MonoBehaviour
{
    // Please implement your locomotion technique in this script. 
    private MySelect rightHand;

    public GameObject currentBall;

    private float constantHeight;

    private bool changeHand = true;


    /////////////////////////////////////////////////////////
    // These are for the game mechanism.
    public ParkourCounter parkourCounter;
    public string stage;
    
    void Start()
    {
        constantHeight = this.gameObject.transform.position.y;

        rightHand = GameObject.Find("RightHandAnchor").GetComponent<MySelect>();
        //currentBall = rightHand.balls[rightHand.currentBall];
        rightHand.AttachBall();
    }

    void Update()
    {
        ////////////////////////////////////////////////////////////////////////////////////////////////////
        // Please implement your LOCOMOTION TECHNIQUE in this script :D.
        Rigidbody rb = currentBall.GetComponent<Rigidbody>();
        Ball ball = currentBall.GetComponent<Ball>();
        if (currentBall.transform.parent == null && rb.velocity == Vector3.zero)
        {
            this.transform.position = new Vector3(currentBall.transform.position.x, currentBall.transform.position.y + constantHeight, currentBall.transform.position.z);

            rightHand.AttachBall();
        }


        ////////////////////////////////////////////////////////////////////////////////
        // These are for the game mechanism.
        if (OVRInput.Get(OVRInput.Button.Two) || OVRInput.Get(OVRInput.Button.Four))
        {
            if (parkourCounter.parkourStart)
            {
                this.transform.position = parkourCounter.currentRespawnPos;
            }
        }
    }


    void OnTriggerEnter(Collider other)
    {

        // These are for the game mechanism.
        if (other.CompareTag("banner"))
        {
            stage = other.gameObject.name;
            parkourCounter.isStageChange = true;
        }
        else if (other.CompareTag("coin"))
        {
            parkourCounter.coinCount += 1;
            this.GetComponent<AudioSource>().Play();
            other.gameObject.SetActive(false);
        }
        // These are for the game mechanism.

    }
}
```

ParkourCounter.cs

```c++
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using TMPro;

public class ParkourCounter : MonoBehaviour
{
    public LocomotionTechnique locomotionTech;
    public bool isStageChange;
    // banners
    public GameObject startBanner;
    public GameObject firstBanner;
    public GameObject secondBanner;
    public GameObject finalBanner;
    // coins
    public GameObject firstCoins;
    public GameObject secondCoins;
    public GameObject finalCoins;
    // respawn points
    public Transform start2FirstRespawn;
    public Transform first2SecondRespawn;
    public Transform second2FinalRespawn;
    public Vector3 currentRespawnPos;

    public float timeCounter;
    private float part1Time;
    private float part2Time;
    private float part3Time;
    public int coinCount;
    
    private int part1Count; // 17
    private int part2Count; // 33
    private int part3Count; // 24
    public bool parkourStart;

    public TMP_Text timeText;
    public TMP_Text coinText;
    public TMP_Text recordText;
    public GameObject startTextGO;
    public GameObject ballTextGO;
    public GameObject timeTextGO;
    public GameObject coinTextGO;
    public GameObject recordTextGO;
    public GameObject endTextGO;
    public AudioSource backgroundMusic;
    public AudioSource endSoundEffect;

    void Start()
    {
        coinCount = 0;
        timeCounter = 0.0f;
        firstBanner.SetActive(false);
        secondBanner.SetActive(false);
        finalBanner.SetActive(false);
        firstCoins.SetActive(false);
        secondCoins.SetActive(false);
        finalCoins.SetActive(false);
        parkourStart = false;
        ballTextGO.SetActive(false);
        endTextGO.SetActive(false);
    }

    void Update()
    {
        if (isStageChange)
        {
            isStageChange = false;
            if (locomotionTech.stage == startBanner.name)
            {
                parkourStart = true;
                startBanner.SetActive(false);
                firstBanner.SetActive(true);
                firstCoins.SetActive(true);
                currentRespawnPos = start2FirstRespawn.position;
            }
            else if (locomotionTech.stage == firstBanner.name)
            {
                firstBanner.SetActive(false);
                firstCoins.SetActive(false);
                secondBanner.SetActive(true);
                secondCoins.SetActive(true);
                part1Time = timeCounter;
                part1Count = coinCount;
                currentRespawnPos = first2SecondRespawn.position;
                UpdateRecordText(1, part1Time, part1Count, 17);
            }
            else if (locomotionTech.stage == secondBanner.name)
            {
                secondBanner.SetActive(false);
                secondCoins.SetActive(false);
                finalBanner.SetActive(true);
                finalCoins.SetActive(true);
                part2Time = timeCounter - part1Time;
                part2Count = coinCount - part1Count;
                currentRespawnPos = second2FinalRespawn.position;
                UpdateRecordText(2, part2Time, part2Count, 33);
            }
            else if (locomotionTech.stage == finalBanner.name)
            {
                parkourStart = false;
                finalCoins.SetActive(false);
                part3Time = timeCounter - (part1Time + part2Time);
                part3Count = coinCount - (part1Count + part2Count);
                UpdateRecordText(3, part3Time, part3Count, 24);
                timeTextGO.SetActive(false);
                coinTextGO.SetActive(false);
                recordTextGO.SetActive(false);
                endTextGO.SetActive(true);
                endTextGO.GetComponent<TMP_Text>().text = "Parkour Finished!\n" + recordText.text +
                    "\ntotal: " + timeCounter.ToString("F2") + ", " + coinCount.ToString() + "/74";
                Debug.Log(endTextGO.GetComponent<TMP_Text>().text);
                endSoundEffect.Play();
            }
        }

        if (parkourStart)
        {
            timeCounter += Time.deltaTime;
            timeText.text = "time: " + timeCounter.ToString("F2");
            coinText.text = "coins: " + coinCount.ToString();
        }       
    }

    void UpdateRecordText(int part, float time, int coinsCount, int coinsInPart)
    {
        string newRecords = "part" + part.ToString() + ": " + time.ToString("F2") + ", " + coinsCount + "/" + coinsInPart;
        recordText.text = recordText.text + "\n" + newRecords;
    }
}

```

