---
title: "[karabiner] capslock 과 spacebar 설정하기"
date: 2023-12-04
tags: [karabiner, capslock, spacebar]
categories:
  - Application
  - Karabiner
---



![](https://i.imgur.com/2amyllE.png)


capslock 키와 spacebar 키를 같이 사용하는 설정을 한다. 


## 목표 
1. capslock 키는 `ctrl+opt` 로 동작한다. 
2. capslock 키만 눌린거라면 `escape`로 동작한다. (`cmd+s` 가 먼저 추가된다.)
3. capslock 키가 눌린 상태에서 spacebar 를 누르면 shift 가 추가된다. `ctrl+opt+shift`
4. capslock + spacebar 눌린 상태에서 다른 키가 눌리지 않는다면 `ctrl+opt+space` 로 동작한다. 

## capslock 설정

먼저 `from`을 설정한다.
키코드는 `caps_lock`, optional 은 `any`로 한다. 

```json
"from": {
  "key_code": "caps_lock",
  "modifiers": {
    "optional": [
      "any"
    ]
  }
},
```

`to`를 설정한다. 
spacebar 와 같이 사용하기 위한 설정을 넣는다. 
그리고, `ctrl+opt` 로 동작하도록 한다.

spacebar 를 shift 로 동작하기 위한 조건을 추가하기 위해서  `set_variable` 를 넣는다.

```json
"to": [
  {
    "set_variable": {
      "name": "space-changed",
      "value": 1
    }
  },
  {
    "set_variable": {
      "name": "space->shift",
      "value": 1
    }
  },
  {
    "key_code": "left_control",
    "modifiers": [
      "left_option"
    ]
  }
],
```


capslock 키나 눌렸을 때 설정을 한다. 
`cmd+s`를 누르고, `escape`가 된다. 
> [!note] `cmd+s` 는 상황에 따라 제외해도 됩니다. 

```json
"to_if_alone": [
  {
    "key_code": "s",
    "modifiers": [
      "left_command"
    ]
  },
  {
    "key_code": "escape"
  }
],
```


### capslock 전체 설정

```json
[
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
        "set_variable": {
          "name": "space-changed",
          "value": 1
        }
      },
      {
        "set_variable": {
          "name": "space->shift",
          "value": 1
        }
      },
      {
        "key_code": "left_control",
        "modifiers": [
          "left_option"
        ]
      }
    ],
    "to_after_key_up": [
      {
        "set_variable": {
          "name": "space-changed",
          "value": 0
        }
      },
      {
        "set_variable": {
          "name": "space->shift",
          "value": 0
        }
      }
    ],
    "to_if_alone": [
      {
        "key_code": "s",
        "modifiers": [
          "left_command"
        ]
      },
      {
        "key_code": "escape"
      }
    ],
    "type": "basic"
  }
]
```


## space 설정

`from`을 설정한다. 
```json
"from": {
  "key_code": "spacebar",
  "modifiers": {
    "optional": [
      "any"
    ]
  }
},
```

조건에 따라 shift 로 동작하도록 설정한다. 
```json
"conditions": [
  {
    "name": "space-changed",
    "type": "variable_if",
    "value": 1
  },
  {
    "name": "space->shift",
    "type": "variable_if",
    "value": 1
  }
],
```


## space 전체 설정

```json
[
  {
    "from": {
      "key_code": "spacebar",
      "modifiers": {
        "optional": [
          "any"
        ]
      }
    },
    "conditions": [
      {
        "name": "space-changed",
        "type": "variable_if",
        "value": 1
      },
      {
        "name": "space->shift",
        "type": "variable_if",
        "value": 1
      }
    ],
    "to": [
      {
        "key_code": "left_shift"
      }
    ],
    "to_if_alone": {
      "key_code": "spacebar",
      "modifiers": [
        "left_control",
        "left_option"
      ]
    },
    "type": "basic"
  },
]
```
