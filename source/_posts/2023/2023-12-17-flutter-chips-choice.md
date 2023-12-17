---
title: "[flutter] chips_choice 사용하기"
date: 2023-12-04
tags: [flutter, chips, choice]
categories:
  - Flutter
  - Chips_choice
---


- https://pub.dev/packages/chips_choice


### 설치
```shell
flutter pub add chips_choice
```


### `Map` 데이터 생성
```dart
final categoryList = <Map<String, String>>[
  {'value': 'mon', 'title': '냉장/냉동'},
  {'value': 'tue', 'title': '간식'},
  {'value': 'wed', 'title': '음료'},
  {'value': 'thu', 'title': '축산물'},
  {'value': 'fri', 'title': '해산물'},
  {'value': 'sat', 'title': '신선식품'},
  {'value': 'sun', 'title': '가공식품'},
];
```

### single 만들기
```dart
ChipsChoice<String>.single(
  value: 'mon',
  onChanged: (value) {},
  choiceItems: C2Choice.listFrom<String, Map<String, String>>(
    source: categoryList,
    value: (index, item) => item['value']!,
    label: (index, item) => item['title']!,
  ),
),
```


![](https://i.imgur.com/pzpMrAm.png)


### wrapped: true 설정

```dart
ChipsChoice<String>.single(
  wrapped: true,
  value: 'mon',
  onChanged: (value) {},
  choiceItems: C2Choice.listFrom<String, Map<String, String>>(
    source: categoryList,
    value: (index, item) => item['value']!,
    label: (index, item) => item['title']!,
  ),
),
```


![](https://i.imgur.com/mmLkB1j.png)


### style 변경 ➡️ choiceStyle: C2ChipStyle.filled()
```dart
ChipsChoice<String>.single(
  wrapped: true,
  value: 'mon',
  onChanged: (value) {},
  choiceStyle: C2ChipStyle.filled(),
  choiceItems: C2Choice.listFrom<String, Map<String, String>>(
    source: categoryList,
    value: (index, item) => item['value']!,
    label: (index, item) => item['title']!,
  ),
),
```

![](https://i.imgur.com/ydoUHxb.png)


### style 변경 ➡️ choiceStyle: C2ChipStyle.outlined()
```dart
ChipsChoice<String>.single(
  wrapped: true,
  value: 'mon',
  onChanged: (value) {},
  choiceStyle: C2ChipStyle.outlined(),
  choiceItems: C2Choice.listFrom<String, Map<String, String>>(
    source: categoryList,
    value: (index, item) => item['value']!,
    label: (index, item) => item['title']!,
  ),
),
```

![](https://i.imgur.com/Y2Lc1Kw.png)


### 선택 항목만, filled 적용
```dart
ChipsChoice<String>.single(
  wrapped: true,
  value: 'mon',
  onChanged: (value) {},
  choiceStyle: C2ChipStyle.outlined(
    selectedStyle: C2ChipStyle.filled()
  ),
  choiceItems: C2Choice.listFrom<String, Map<String, String>>(
    source: categoryList,
    value: (index, item) => item['value']!,
    label: (index, item) => item['title']!,
  ),
),
```


![](https://i.imgur.com/Ca1oTQi.png)


### Generics 로 만들기


```dart
@jsonSerializable
class Choice {
  String id = '';
  String name = '';
}
```


```dart
class ChoiceSingle<T extends Choice> extends ConsumerWidget {
  const ChoiceSingle({
    super.key,
    required this.label,
    required this.value,
    required this.data,
    required this.onChanged,
  });
  final String label;
  final T? value;
  final List<T> data;
  final void Function(T) onChanged;

  @override
  Widget build(BuildContext context, WidgetRef ref) {
    return Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        Text(label),
        if (data.isEmpty)
          const Center(child: LoadingSmall())
        else
          Center(
            child: ChipsChoice<T>.single(
              wrapped: true,
              value: value,
              onChanged: onChanged,
              choiceCheckmark: true,
              choiceStyle: C2ChipStyle.outlined(
                color: Colors.grey,
                selectedStyle: C2ChipStyle.filled(
                  color: Theme.of(context).primaryColor,
                  foregroundColor: Colors.white,
                  borderRadius: const BorderRadius.all(
                    Radius.circular(25),
                  ),
                ),
              ),
              choiceItems: C2Choice.listFrom<T, T>(
                source: data,
                value: (index, item) => item,
                label: (index, item) => item.name,
              ),
            ),
          ),
      ],
    );
  }
}

```
