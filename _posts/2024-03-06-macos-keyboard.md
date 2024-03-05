---
title: 카라비너로 키보드 더 편하게 쓰기
author: 김준회
date: 2024-03-06 00:00:00 +0900
categories: [MacOS]
tags: [Karabiner, TouchCursor]
pin: true
math: true
mermaid: true
image:
  path: /commons/today-i-learned.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: TIL image
---

# 키맵핑이 없으면 너무 불편해
## 윈도우에선..
윈도우에서는 처음엔 오토핫키로 직접 키맵핑을 해서 썼다. 그런데 가끔 관련 주제로 찾고 찾다보니 아주 좋은 소프트웨어를 찾았다. TouchCursor가 그것이다.
## 맥에선..
### 한영전환
맥에서는 한영전환을 더 편하게 하고 싶어서 카라비너를 썼다. 사실상 필수나 다름없었다. 기본 설정인 캡스락 전환은 뭔가 애매하게 딜레이가 있었다. 정확히는 딜레이보다 다른 키랑 중복해서 입력이 되지 않았던 것 같다. 이건 매우 답답해서 맥을 쓰면서 디테일에 실망했던 요소 중 하나였다.

캡스락이 한영전환 키라는 건 나쁘지 않은데, 숏컷에서 `select previous input source`은 키가 무시되는 문제가 아주 자주 발생하고, `select next input source` 숏컷으로 설정에서 직접 바꿔줘야 이런 문제를 해결할 수 있었다.

### TouchCursor가 너무 편해서 맥도...
캡스락은 아주아주 좋은 위치를 가지고 있다. 컨트롤이나 알트, 윈도우 혹은 커맨드키 보다도 더 누르기 편한 위치에 있다. 전체 손을 하나도 움직이지 않아도 되는 위치다.

이 캡스락을 레이어를 바꾸는 키로 사용하면 아주아주아주 편하다. 이 짧은 글의 결론도 그것이다... 잘 쓰지도 않는 캡스락을 제대로 활용해야 해보시라, 츄라이 츄라이~ 카라비너가 정말 좋은 이유 중 하나는 [이런 공유](https://ke-complex-modifications.pqrs.org/)가 있기 떄문이다. 해당 링크에서 caps-lock 을 검색해보시길...

다만 TouchCursor는 설정하는 것이 쉬운데 카라바이너는 그렇지가 않다. 다행히 TouchCursor 기본 설정과 똑같게 룰을 작성해서 공유해주신 분이 있었다. 레이어 변경 키가 스페이스바여서 그것을 스크롤락으로 변경했다. 아래에 변경한 룰을 사용하려면 사전 작업이 필요하다.

1. 맥 설정에서 화면 밝기 줄이기 숏컷을 해제해야 한다. 키보드 숏컷에서 디스플레이에 f14에 해당하는 숏컷이다. (맥 키보드로 밝기 조정하는 것에는 아무런 영향이 없다.)
2. 카라비너에서 단순 키매핑(Simple Modification)을 걸어줘야 한다. (caps_lock -> scroll_lock) 왜 그렇냐면, 원치 않게 캡스락이 눌릴 때마다 불편하기 싫기 때문이다. 
> f14 등 안 쓰는 펑션키를 써도 될 것 같은데, 윈도에서 가장 스무스하게 동작했던 키가 scroll_lock 이었기에 그렇게 했다. 참고로 카라비너에서 scroll_lock 은 keys in pc keyboard 항목 아래에 있다.
3. 아래 json 내용대로 Complex Modifications를 넣어주면 된다.
```json
{
  "description": "TouchCursor Mode [Space as Trigger Key] (rev 2)",
  "manipulators": [
      {
          "from": {
              "key_code": "scroll_lock",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "set_variable": {
                      "name": "touchcursor_mode",
                      "value": 1
                  }
              }
          ],
          "to_after_key_up": [
              {
                  "set_variable": {
                      "name": "touchcursor_mode",
                      "value": 0
                  }
              }
          ],
          "to_if_alone": [
              {
                  "key_code": "scroll_lock"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "j",
              "modifiers": {
                  "optional": [
                      "any"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "left_arrow"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "k",
              "modifiers": {
                  "optional": [
                      "any"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "down_arrow"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "i",
              "modifiers": {
                  "optional": [
                      "any"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "up_arrow"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "l",
              "modifiers": {
                  "optional": [
                      "any"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "right_arrow"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "u",
              "modifiers": {
                  "optional": [
                      "any"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "left_arrow",
                  "modifiers": [
                      "command"
                  ]
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "o",
              "modifiers": {
                  "optional": [
                      "any"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "right_arrow",
                  "modifiers": [
                      "command"
                  ]
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "y",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "insert"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "m",
              "modifiers": {
                  "optional": [
                      "any"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "delete_forward"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "p",
              "modifiers": {
                  "optional": [
                      "any"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "delete_or_backspace"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "h",
              "modifiers": {
                  "optional": [
                      "any"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "page_up"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "n",
              "modifiers": {
                  "optional": [
                      "any"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "page_down"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "return_or_enter",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "return_or_enter"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "tab",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "tab"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "hyphen",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "hyphen"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "equal_sign",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "equal_sign"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "open_bracket",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "open_bracket"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "close_bracket",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "close_bracket"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "backslash",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "backslash"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "non_us_pound",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "non_us_pound"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "semicolon",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "semicolon"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "quote",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "quote"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "grave_accent_and_tilde",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "grave_accent_and_tilde"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "comma",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "comma"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "period",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "period"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "slash",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "slash"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "non_us_backslash",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "non_us_backslash"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "1",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "1"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "2",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "2"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "3",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "3"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "4",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "4"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "5",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "5"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "6",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "6"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "7",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "7"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "8",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "8"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "9",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "9"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "0",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "0"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "a",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "a"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "b",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "b"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "c",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "c"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "d",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "d"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "e",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "e"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "f",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "f"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "g",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "g"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "q",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "q"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "r",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "r"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "s",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "s"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "t",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "t"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "v",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "v"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "w",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "w"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "x",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "x"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      },
      {
          "conditions": [
              {
                  "name": "touchcursor_mode",
                  "type": "variable_if",
                  "value": 1
              }
          ],
          "from": {
              "key_code": "z",
              "modifiers": {
                  "optional": [
                      "caps_lock"
                  ]
              }
          },
          "to": [
              {
                  "key_code": "scroll_lock"
              },
              {
                  "key_code": "z"
              },
              {
                  "key_code": "vk_none"
              }
          ],
          "type": "basic"
      }
  ]
}
```

