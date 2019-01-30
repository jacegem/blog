---
title: 'Android 사진 동영상 불러오기'
date: 2017-01-07 19:13:45
tags: [android]
categories:
- Programming
- Android
---

# Android 사진 동영상 불러오기

## 웹앱 사용

```java
import android.webkit.JavascriptInterface;
mWebView.addJavascriptInterface(new AndroidBridge(this), "androidJS");
```

## 클래스 생성

```java
//웹뷰 서버 자바스크립트 연동
private class AndroidBridge {
    Context context = null;

    public AndroidBridge(Context context) {
        this.context = context;
    }
}
```

## 갤러리 이미지 호출

```java
//갤러리 이미지 호출
Uri uri = Uri.parse("content://media/external/images/media");
Intent intent = new Intent(Intent.ACTION_VIEW, uri);
intent.setAction(Intent.ACTION_GET_CONTENT);
intent.setType("image/*");
startActivityForResult(intent, IMAGEFILE_REQUEST);
```

### 썸네일 Base64 생성

```java
// 이미지, 동영상의 썸네일의 base64를 반환한다.
private String getBase64(String file_path) {
    // http://lookintoandroid.blogspot.kr/2015/11/getting-camera-captured-image-in.html
    int width = 400;
    int height = 300;
    String ext = FilenameUtils.getExtension(file_path);

    Bitmap resized = null;
    if (ext.equals("jpg")){
        resized = ThumbnailUtils.extractThumbnail(BitmapFactory.decodeFile(file_path), width, height);
    } else if (ext.equals("mp4")){
        // http://stackoverflow.com/questions/20208007/difference-between-micro-kind-and-mini-kind-in-mediastore-in-android
        resized = ThumbnailUtils.createVideoThumbnail(file_path, MediaStore.Images.Thumbnails.MINI_KIND);
    }

    ByteArrayOutputStream baos = new ByteArrayOutputStream();
    resized.compress(Bitmap.CompressFormat.PNG, 100, baos); //bm is the bitmap object
    byte[] byteArrayImage = baos.toByteArray();
    return Base64.encodeToString(byteArrayImage, Base64.DEFAULT);
}
```

## 갤러리 동영상 호출

```java
//갤러리 동영상 호출
Uri uri = Uri.parse("content://media/external/images/media");
Intent intent = new Intent(Intent.ACTION_VIEW, uri);
intent.setAction(Intent.ACTION_GET_CONTENT);
intent.setType("video/*");
startActivityForResult(intent, VIDEOFILE_REQUEST);
```

## 사진 촬영

```java
Intent intent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
startActivityForResult(intent, CAMERA_IMAGE_REQUEST);
```

###  onActivityResult

사진 촬영의 경우 intent.getData() 를 통해 데이터를 얻지 못하므로, intent.getExtras().get("data") 를 사용합니다.

데이터를 받아서, 저장한 후 해당파일을 사용하여 처리합니다.

```java
if (resultCode == Activity.RESULT_OK) {
    final Bitmap bitmap = (Bitmap) intent.getExtras().get("data");
    String root = Environment.getExternalStorageDirectory().getAbsolutePath();
    File myDir = new File(root + "/saved_images");
    myDir.mkdirs();
    String time = String.valueOf(System.currentTimeMillis());
    String saved_file_name = "Image-"+ time +".jpg";
    File file = new File (myDir, saved_file_name);
    FileOutputStream out = new FileOutputStream(file);
    bitmap.compress(Bitmap.CompressFormat.JPEG, 100, out);
    out.flush();
    out.close();
}
```

## 동영상 촬영

```java
Intent intent = new Intent(MediaStore.ACTION_VIDEO_CAPTURE);
startActivityForResult(intent, CAMERA_VIDEO_REQUEST);
```
