---
title: '[Unity] Tutorials - Roll-a-Ball'
date: 2017-01-01 19:13:45
tags: [unity]
categories:
- Programming
- Unity
---

# [Unity] Tutorials - Roll-a-Ball

http://unity3d.com/learn/tutorials/modules/ 사이트에 접속합니다.

PROJECTS 들이 여러개 보이는데요. 그중에서 Beginner 난이도인 Roll-a-Ball을 보도록 하겠습니다.

# 기본 마우스 기능

오브젝트를 선택할 경우에는 `Shift + 좌클릭`

화면을 회전할 경우에는 `Alt + 좌클릭`

평행이동을 하고 싶을 경우에는 `마우스 중클릭` 을 해야 합니다.

# Hierarchy

## Scene

| Hierachy | 설명 |
|--|--|
| Ground | 바닥, 배경|
| Player | 움직이는 공|
| Fill light | 아래에서 비추는 빛 |
| Main light | 위에서 비추는 빛 |
| Walls | 동서남북 벽 |
| PickUps | 회전하는 정육면체|
| Win Text | 승리 메시지 |
| Count Text | 점수|

## GUI Text

튜토리얼과 같이 GUI Text를 생성할 수 없습니다. `Create Empty Object` 후에 `Component → Rendering → GUIText` 를 추가해야 합니다

# Assets

## Prefabs
PickUp : pickup들에 공통적으로 스크립트를 적용합니다.

## Scenes
MiniGame : 메인 화면

## Scripts
### CameraController

```c#
using UnityEngine;
using System.Collections;

public class CameraController : MonoBehaviour {

     public GameObject player;
     private Vector3 offset;

     // Use this for initialization
     void Start () {
          offset = transform.position;
     }
     // Update is called once per frame
     void LateUpdate () {
          transform.position = player.transform.position + offset;
     }
}
```

### PlayerController

```c#
using UnityEngine;
using System.Collections;

public class PlayerController : MonoBehaviour {

     public float speed;
     public GUIText countText;
     public GUIText winText;
     private int count;

     void Start(){
          count = 0;
          SetCountText();
          winText.text = "";
     }

     void FixedUpdate(){
          float moveHorizontal = Input.GetAxis ("Horizontal");
          float moveVertical = Input.GetAxis ("Vertical");

          Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);

          rigidbody.AddForce (movement * speed * Time.deltaTime);
     }

     void OnTriggerEnter(Collider other) {
          if (other.gameObject.tag == "PickUp") {
               other.gameObject.SetActive(false);
               count++;
               SetCountText();
          }
     }

     void SetCountText(){
          countText.text = "Count : " + count;

          if (count >= 12) {
               winText.text = "You WIN!";
          }
     }

}
```

### Rotate

```c#
using UnityEngine;
using System.Collections;

public class Rotate : MonoBehaviour {

     // Use this for initialization
     void Start () {
     }
     // Update is called once per frame
     void Update () {

          transform.Rotate (new Vector3 (15, 30, 45) * Time.deltaTime);
     }
}
```

# Build

## Android

`File → Build Settings → Player Settings'' 에서 ''Bundle Indentifier`를 변경해야 합니다.

`SDK` 경로 변경은 `Edit → Preferences → External Tools` 에서 할 수 있습니다.
