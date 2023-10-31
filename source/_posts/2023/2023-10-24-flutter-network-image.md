---
title: "[Flutter] 이미지 보여주기"
date: 2023-10-24
tags: [flutter, image]
categories:
  - Flutter
  - Image
---

![](https://picsum.photos/250?image=9)


## 네트워크 이미지 보여주기

```dart
Image.network('https://picsum.photos/250?image=9')
```

## 모서리 둥글게 만들기

```dart
ClipRRect(
    borderRadius: BorderRadius.circular(8.0),
    child: Image.network(
        'https://picsum.photos/250?image=9',
        height: 150.0,
        width: 100.0,
    ),
)
```

## 원형 이미지 만들기

```dart
CircleAvatar(
  radius: 48, // Image radius
  backgroundImage: NetworkImage('imageUrl'),
)
```

```dart
ClipOval(
  child: SizedBox.fromSize(
    size: Size.fromRadius(48), // Image radius
    child: Image.network('imageUrl', fit: BoxFit.cover),
  ),
)
```

## cached_network_image 사용하기

- https://pub.dev/packages/cached_network_image

### dependecies 추가

pubspec.yaml 파일에 아래 내용을 추가한다. 

```dart
dependencies:
  cached_network_image: ^3.3.0
```

CachedNetworkImage 를 사용하여 이미지를 표출한다. 

```dart
CachedNetworkImage(
        imageUrl: "http://via.placeholder.com/350x150",
        placeholder: (context, url) => CircularProgressIndicator(),
        errorWidget: (context, url, error) => Icon(Icons.error),
     ),
```


## 출처

- https://pub.dev/packages/cached_network_image
- https://stackoverflow.com/questions/51513429/how-to-do-rounded-corners-image-in-flutter
