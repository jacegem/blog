---
title: '[jQuery] file upload'
date: 2017-02-02 19:13:45
tags: [jquery, file, upload, javascript]
categories:
- Programming
- Javascript
---

# [jQuery] file upload

file upload 를 구현한다.

DB 에 입력되는 것을 디스크에 저장되도록 수정한다.

글로벌 프로퍼티 설정

```xml
# for JFile properties
system.uploadpath = /upload
```

자바에서 해당 값을 주입합니다.

```java
@Value("#{global['system.uploadpath']}")
private String fileBasePath;
```

Value 어노테이션 사용을 위해 임포트를 추가합니다.

```java
import org.springframework.beans.factory.annotation.Value;
```

리퀘스트 맵핑을 등록하고 함수를 구현합니다.

```java
@RequestMapping("/fileUploadDisk.json")
public void fileUploadDisk(RMap rmap, ModelMap model, @RequestParam("file") MultipartFile file) throws IOException {
    //
}
```

@RequestParam\("file"\)를 통해서 멀티파트파일 정보는 file 변수에 담기게 됩니다.

업로드 대상 URL 정보를 위에서 설정한 주소로 변경합니다.

```javascript
url : "/web/common/file/fileUploadDisk.json"
```

jQuery.uploadfile 플러그인을 사용중이므로 아래와 같이 설정합니다.

```javascript
// 파일 업로드
upload = $("#spreadRegistFile").uploadFile({
    url : "/file/fileUploadDisk.json",
    fileName : "file",
    autoSubmit : false,
    dragDropStr : '',
    uploadStr : '등록',
    showQueueDiv : 'spreadRegistFileQueue',
    dragdropWidth : 150,
    statusBarWidth : 140,
    maxFileCount : 5,
    showError : false,
    showProgress : false,
    onSuccess : function(files, data, xhr, pd) {
        toast.push(Object.toJSON(files));
        upload.reset();
    },
    onError : function(files, status, errMsg, pd) {
        toast.push(errMsg);
    }
});
```

![](https://goo.gl/cbOJ36)

## controller 구현

## service 구현

```java
public int insertFileInfo(RMap rmap, ModelMap model) {
    rmap.put("user_id", rmap.getSession().getAttribute("user_id"));
    return dao.insert("web.common.insertFileInfo", rmap);
}
```

## mybatis 구현

insert 태그 추가

```xml
<insert id="insertFileInfo">
        /* web.common.insertFileInfo */
       ...
</insert>
```

저장할 대상

```
- #{file_seq}
- #{file_size}
- #{file_type}
- #{original_file_name}
- #{file_name}
- #{file_path}
```

```xml
<!-- 파일 등록 -->
<insert id="insertFileInfo">
    /* web.common.insertFileInfo */
    INSERT INTO FILE_INFO
    (
        FILE_SEQ
        , FILE_NO
        , FILE_SIZE
        , FILE_TYPE
        , ORIGINAL_FILE_NAME
        , FILE_NAME
        , FILE_PATH            
        , REG_USER
        , REG_DATE
        , LAST_DATE            
    )
    VALUES
    (
        #{file_seq}
        , (SELECT COALESCE(MAX(FILE_NO),0)+1 FROM FILE_INFO WHERE FILE_SEQ = #{file_seq})
        , #{file_size}
        , #{file_type}
        , #{original_file_name}
        , #{file_name}
        , #{file_path}
        , #{user_id}
        , TO_CHAR(NOW(), 'YYYYMMDDHH24MISS')
        , TO_CHAR(NOW(), 'YYYYMMDDHH24MISS')            
    )        
</insert>
```

저장된 것을 확인한다.
![](https://goo.gl/RCWPyw)

## 파일 불러오기

조회시에, `original_file_name`을 가져오도록 수정.
화면 출력시에도 이름 변경해야 함.

파일을 선택했을 때, 요청하는 부분을 수정하자.

이미지의 경우 팝업 화면을 보여주고, 동영상의 경우 다운로드 되도록 처리한다.

### 이미지

```html
onclick="wutil.popup(seq, no)"
```

```javascript
url : "/file/attachPop.do",
```

JSP에서 이미지를 요청한다.

```html
<img src="<c:url value='/web/common/file/attachImage.do?seq=${seq}&no=${no}'/>" width="550" />
```

```java
UMap umap = commonService.selectAttachFile(rmap, model);
```

```java
@RequestMapping("/attachImage.do")
public void attachImage(RMap rmap, ModelMap model, HttpServletRequest request, HttpServletResponse response) throws IOException {
    // 파일 경로를 조회한다.
    UMap umap = commonService.selectAttachFile(rmap, model);

    String attach_file_path = (String) umap.get("attach_file_path");        
    System.out.println(attach_file_path);        

    // 바이너리를 전송한다.
    try{
        Path path = Paths.get(attach_file_path);
        byte[] data = Files.readAllBytes(path);
        response.setContentType("image/jpeg");
        response.getOutputStream().write(data);
    } catch(Exception e) {
        e.printStackTrace();
        logger.debug("파일 생성중 오류 발생",e);
    }
}
```

### 동영상

```html
onclick="wutil.download(seq, no)"
```

```javascript
window.location = "/file/fileDownload.do?seq=" + seq + "&no=" + no;
```
