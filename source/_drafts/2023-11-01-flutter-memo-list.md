---
title: "[Flutter] 메모 목록 화면 만들기"
date: 2023-10-24
tags: [flutter, memo, list]
categories:
  - Flutter
  - Project
---


<img src="https://i.imgur.com/bhHMRHW.jpg"  width="200" style="display:block;margin:0 auto;"/>

## view 만들기  

```dart
class MemoListPage extends StatelessWidget {
  const MemoListPage({super.key});

  @override
  Widget build(BuildContext context) {
    return const Placeholder();
  }
}
```

## controller 만들기

`zone`을 arguments 로 받는다. 

```dart
class MemoListController extends GetxController {
  late Zone zone;

  @override
  void onInit() async {
    super.onInit();
    zone = Get.arguments[Fields.zone];
  }
}
```

## binding 만들기

```dart
class MemoListBinding implements Bindings {
  @override
  void dependencies() {
    Get.put(MemoListController());
  }
}
```

## route 정의

```dart
static const memoList = '/memo/list';

GetPage(
  name: memoView,
  page: () => const MemoListPage(),
  binding: MemoListBinding(),
  transition: defaultTransition,
),
```

