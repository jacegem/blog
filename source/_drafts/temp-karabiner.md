






Karabiner 를 사용해서, control, option, command, shift 키를 추가해보자.

> 아래와 같이 표기합니다. 
> control ➡️ `ctrl`
> option ➡️ `opt`
> command ➡️ `cmd`
> shift ➡️ `sft` 
> capslock ➡️ `caps`

## 설정 파일

`/.config/karabiner/karabiner.json`에 rule 을 추가한다.

### 기본 구조


```json
{
    "profiles": [
        {
            "name": "Default profile",
            "complex-modifications": {
                "rules": [
                    <여기에 추가한다>
                ]
            }
        }
    ] 
}
```

profiles 중에서 원하는 프로파일을 이름으로 찾아서, rules 부분에 추가한다.

## 키 변경

사용빈도가 낮은 키를 변경한다.
`capslock` 키를 `ctrl+opt` 로 변경한다.

```json
{
  "description": "caps_lock ➡️ ctrl+opt",
  "manipulators": [
    {
      "from": {
        "key_code": "caps_lock",
        "modifiers": {
          "optional": [
            "any"
          ]
        }
      },
      "to": [
        {
          "key_code": "left_control",
          "modifiers": [
            "left_option"
          ]
        }
      ],
      "type": "basic"
    }
  ]
},
```

`from`부분에 입력 키값을 넣고, `to`부분에 변경할 키값을 넣었다.


## 동시에 누르기

`s`+`d` 키를 동시에 누르면, `ctrl+opt` 로 변경한다.

```json
{
  "description": "s+d ➡️ ctrl+opt",
  "copy-flip": true,
  "manipulators": [
    {
      "from": {
        "simultaneous": [
          {
            "key_code": "s"
          },
          {
            "key_code": "d"
          }
        ],
        "modifiers": {
          "optional": [
            "any"
          ]
        },
        "simultaneous_options": {
          "detect_key_down_uninterruptedly": true,
          "key_down_order": "insensitive",
          "key_up_order": "insensitive",
          "key_up_when": "all",
          "to_after_key_up": []
        }
      },
      "to": [
        {
          "key_code": "left_control",
          "modifiers": [
            "left_option"
          ]
        }
      ],
      "type": "basic"
    }
  ]
},
```

## 조합 

위와 같은 방법으로 조합가능한 키들을 모두 추가한다. 
> 설정 파일은 [이곳](https://github.com/jacegem/karabiner/blob/main/karabiner.json)에서 볼수 있습니다.


![](https://i.imgur.com/9zXT1h4.png)
![](https://i.imgur.com/JnbzDvQ.png)
![](https://i.imgur.com/tByC2Ng.png)
![](https://i.imgur.com/V7yDy9S.png)








- https://karabiner-elements.pqrs.org/docs/json/typical-complex-modifications-examples/#change-right_shift-x2-to-mission_control

여기 예제를 보고 작성한다. 

`right-shift`를 두번 누르면 ``ctrl+opt+` ``로 변경되도록 한다.

manipulators 에 두개의 설정을 넣는다. 

하나는 variable 값을 변경하는 것이고, 하나는 변경된 variable 값을 확인하는 것이다. 

```json
{
  "description": "double-right-shift to ...",
  "manipulators": [
    {}
    {}
  ]
}
```


```json
{
  "set_variable": {
    "name": "right-shift",
    "value": 1
  }
}
```



### 설정



```json
{
  "description": "double-right-shift to ...",
  "manipulators": [
    {
      "from": {
        "key_code": "right_shift",
        "modifiers": {
          "optional": [
            "any"
          ]
        }
      },
      "conditions": [
        {
          "name": "right-shift",
          "type": "variable_if",
          "value": 1
        }
      ],
      "to": [
        {
          "key_code": "grave_accent_and_tilde",
          "modifiers": [
            "left_control",
            "left_option"
          ]
        }
      ],
      "type": "basic"
    },
    {
      "from": {
        "key_code": "right_shift",
        "modifiers": {
          "optional": [
            "any"
          ]
        }
      },
      "to": [
        {
          "set_variable": {
            "name": "right-shift",
            "value": 1
          }
        },
        {
          "key_code": "right_shift"
        }
      ],
      "to_delayed_action": {
        "to_if_invoked": [
          {
            "set_variable": {
              "name": "right-shift",
              "value": 0
            }
          }
        ],
        "to_if_canceled": [
          {
            "set_variable": {
              "name": "right-shift",
              "value": 0
            }
          }
        ]
      },
      "type": "basic"
    }
  ]
},
```


## link

- https://github.com/pqrs-org/Karabiner-Elements/issues/925
