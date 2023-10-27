---
title: "[Flutter] GetX 라우트 사용하기"
date: 2023-10-27
tags: [flutter, getx, route]
categories:
  - Flutter
  - GetX
---

![](https://raw.githubusercontent.com/jonataslaw/getx-community/master/get.png)


## getx 설치

```dart
dependencies:
  get: ^4.6.6
```

## routes 생성

url 을 정의하고, pages 에서 binding 까지 설정한다.

```dart
class Routes {
  static const main = '/';  
  static const defaultTransition = Transition.downToUp;
  static final pages = <GetPage>[
    GetPage(
      name: main,
      page: () => const MainPage(),
      binding: MainBinding(),
      transition: defaultTransition,
    ),
  ];
}
```

## routes 정의

`GetMaterialApp`에서 `initialRoute`와 `getPages`를 설정한다. 

```dart
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return GetMaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'YOUR APP NAME',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
        useMaterial3: true,
      ),
      initialRoute: Routes.main,
      getPages: Routes.pages,
    );
  }
}
```

## route 이동

`toNamed`를 사용하여 이동한다. 
main 으로 이동하는 함수이며, 이때 전달인자로, "key" 키에, "val" 값을 전달한다. 

```dart
Get.toNamed(
  Routes.main,
  arguments: {
    "key": "val",    
  },
);
```

## arguments 사용

전달받은 arguments를 사용할 수 있다. 

```dart
var val = Get.arguments["key"];
```


## 출처

- https://pub.dev/packages/get
- https://github.com/jonataslaw/getx/blob/master/documentation/en_US/route_management.md#navigation-with-named-routes
