---
title: "[Spring] PostgreSQL에 파일 저장하고 불러오기"
date: 2018.09.21
tags: [spring, mybatis, postgresql, psql, file, bytea]
categories:
- Programming
- Spring
---

# [Spring] PostgreSQL에 파일 저장하고 불러오기

### bytea 컬럼 생성

파일(바이너리)을 저장하기 위한 컬럼을 생성합니다. 

```sql
CREATE TABLE public.attach_file_info
(
  file_size integer NOT NULL, -- 파일크기
  file_type character varying(4), -- 파일유형
  thumbnail text, -- 썸네일
  file bytea,
)
```

마지막 file 컬럼을 bytea 타입으로 생서합니다. 

### 파일 전송

이부분은 자바로 전송하는 코드를 사용하였습니다. 

```java
private void fileUploadWithThumbnail(String attach_file_seq, String file_path, String user_id, String thumbnail) {
    File uploadFile = new File(file_path);
    MultipartEntityBuilder builder = MultipartEntityBuilder.create() //객체 생성
            .setCharset(Charset.forName("UTF-8")) //인코딩을 UTF-8로
            .setMode(HttpMultipartMode.BROWSER_COMPATIBLE);
    builder.addPart("file", new FileBody(uploadFile)); //빌더에 FileBody 객체에 인자로 File 객체를 넣어준다.
    builder.addTextBody("attach_file_seq", attach_file_seq, ContentType.create("Multipart/related", "UTF-8"));
    builder.addTextBody("user_id", user_id, ContentType.create("Multipart/related", "UTF-8"));
    builder.addTextBody("thumbnail", thumbnail, ContentType.create("Multipart/related", "UTF-8"));

    try {
        InputStream inputStream = null;
        HttpClient httpClient = new DefaultHttpClient();
        HttpPost post = new HttpPost(server_domain + "/file/fileUploadDisk.json"); //전송할 URL
        HttpEntity multipart = builder.build();
        post.setEntity(multipart);
        HttpResponse httpResponse = httpClient.execute(post);
        HttpEntity httpEntity = httpResponse.getEntity();
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

`MultipartEntityBuilder` 를 사용하여 파일을 전송합니다. 

- `file` 에 전송할 파일을 담습니다. 
- 추가적으로 필요한 정보들을 넣습니다. 
- Post 로 해당 내용을 전송합니다. 

### 서버에서 파일 저장


```java
@RequestMapping("/fileUploadDisk.json")
public void fileUploadDisk(RMap rmap, ModelMap model, @RequestParam("file") MultipartFile file) throws 
IOException {
    // 파일정보가 없는 경우가 리턴합니다.
    if (file.isEmpty())
        return;
        
    rmap.put("file", file.getBytes());
    commonFileService.insertAttachFileInfo(rmap, model);
}   
```

`getBytes`로 파일을 byte array 로 받습니다. 그리고 mybatis 로 전달합니다. 


### insert Query

```sql
<!-- 파일 등록 -->
<INSERT id="insertAttachFileInfo">
	/* file.insertAttachFileInfo */
	INSERT INTO ATTACH_FILE_INFO
	(
		thumbnail, 
		file
	)
	VALUES
		(
		
			#{thumbnail},
			#{file}
		)
</INSERT>
```

byte array 를 쿼리를 통해 그대로 저장합니다. 

### 이미지 불러오기

```javascript
@RequestMapping("/attachImage.do")
public void attachImage(RMap rmap, ModelMap model, HttpServletRequest request, HttpServletResponse response) throws IOException {
    // 파일 조회
    UMap umap = commonService.selectAttachFile(rmap, model);

    byte[] data = (byte[]) umap.get("file");
    String file_type = (String)umap.get("file_type");
    
    if (data == null || !file_type.equals("0001")){
        String thumbnail = (String) umap.get("thumbnail");
        data = Base64.decodeBase64(thumbnail.split("base64,")[1]);
    }

    // 이미지를 전송한다.
    response.setContentType("image/jpeg");
    response.getOutputStream().write(data);
}   
```

### 다운로드 하기


```java
@RequestMapping("/fileDownload.do")
public void fileDownload(RMap rmap, ModelMap model, HttpServletRequest request, HttpServletResponse response) throws IOException {
    UMap umap = commonService.selectAttachFile(rmap, model);

    String filename = (String) umap.get("original_file_name");
    String fileNm = new String(filename.getBytes("UTF-8"), "ISO-8859-1");

    response.setContentType("application/octet-stream");
    response.setHeader("Content-Disposition", "attachment;filename=" + fileNm);

    byte[] data = (byte[]) umap.get("file");
    response.getOutputStream().write(data);
}
```