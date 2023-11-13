---
title: "[Clojure] backblaze 이미지 업로드"
date: 2023-10-24
tags: [clojure, backblaze, image, upload]
categories:
  - Clojure
  - Image
---


<img src="https://secure.backblaze.com/bzapp_web_assets/public/pics/header/logo-backblaze-flame-header.4851ea2289eaf4242079c6dcd0acb1be.png"  width="200" style="display:block;margin:0 auto;"/>

## 준비

1. https://www.backblaze.com/ 에서 backblaze 회원가입

2. 버킷 생성

![](https://i.imgur.com/CoGvPdY.png)
Create a Bucket 를 선택한다. 

![](https://i.imgur.com/8AHHOb4.png)
적당한 이름을 입력하고, Files in Bucket are 를 `Public`으로 변경한다.

`Buckets`에서 생성한 버킷의 내용을 보면, `Bucket ID`를 확인할 수 있다. 

3. 앱키 생성

![](https://i.imgur.com/FsxX0sE.png)

Account > Application Keys 에서 `Add a New Application Key`를 눌러서 앱키를 생성한다. 

![](https://i.imgur.com/cYrWF3w.png)

![](https://i.imgur.com/JyzoWGD.png)

생성후에 나오는, `keyId`, `applicationKey`를 확인한다.
> 한번만 볼 수 있으므로 잘 보관하도록 한다. 

이렇게 얻은 3가지 값을 이용해서 이미지 업로드를 진행한다. 

- `Bucket ID`
- `keyId`
- `applicationKey` 

## 1단계 :  b2_authorize_account 요청

https://www.backblaze.com/apidocs/b2-authorize-account 에 GET 요청으로 `authorization-token`을 얻는다.

요청할때 헤더에 위에서 확인한 값을 넣는다.
`<keyId>:<applicationKey>` 를 base64로 인코딩한 값을 `Authorization`으로 전달한다. 

### b2_authorize_account 요청 코드

```clojure
(:require [camel-snake-kebab.core :as csk]
          [clojure.data.codec.base64 :as base64]
          [clojure.data.json :as json]
          [hato.client :as hato])

(defn str->base64 [s]
  (String. (base64/encode (.getBytes s)) "UTF-8"))

(defn b2-authorize-account [key-id app-key]
  (let [api-url  "https://api.backblazeb2.com/b2api/v3/b2_authorize_account"
        encoded  (str->base64 (str key-id ":" app-key))
        req-opts {:content-type :json
                  :headers      {"Authorization" (str "Basic " encoded)}}]
    (-> (hato/get api-url req-opts)
        :body
        (json/read-str :key-fn csk/->kebab-case-keyword))))
```

요청이 성공하면 다음의 결과를 확인할 수 있다. 
키값은 케밥케이스로 변환하였다.

```clojure
{:account-id "<account-id>",
 :api-info
 {:storage-api
  {:capabilities
   ["deleteKeys"
    "bypassGovernance"
    "readFileRetentions"
    "readBucketReplications"
    "writeFileRetentions"
    "writeKeys"
    "readFiles"
    "writeBucketEncryption"
    "writeBucketReplications"
    "listBuckets"
    "listFiles"
    "readBucketRetentions"
    "writeFiles"
    "deleteFiles"
    "writeBucketRetentions"
    "listKeys"
    "readBuckets"
    "readFileLegalHolds"
    "shareFiles"
    "readBucketEncryption"
    "writeBuckets"
    "deleteBuckets"
    "writeFileLegalHolds"],
   :info-type "storageApi",
   :download-url "https://fNNN.backblazeb2.com",
   :s-3-api-url "https://s3.us-east-NNN.backblazeb2.com",
   :api-url "https://apiNNN.backblazeb2.com",
   :recommended-part-size 100000000,
   :absolute-minimum-part-size 5000000,
   :name-prefix nil,
   :bucket-id nil,
   :bucket-name nil}},
 :application-key-expiration-timestamp nil,
 :authorization-token "<authorization-token>"}
```

## 2단계 : b2-get-upload-url 요청

위에서 얻은 `authorization-token`, `api-url`를 사용하여 upload-url 을 요청한다. 
추가적으로 버킷 생성후에 얻는 `Bucket ID`를 사용한다. 

```clojure
(defn b2-get-upload-url [bucket-id {:keys [token api-url]}]
  (let [api-url  (str api-url "/b2api/v2/b2_get_upload_url")
        req-opts {:content-type :json
                  :headers      {"Authorization" token}
                  :query-params {:bucketId bucket-id}}]
    (-> (hato/get api-url req-opts)
        :body
        (json/read-str :key-fn csk/->kebab-case-keyword))))
```

결과

```clojure
{:authorization-token "<authorization-token>",
 :bucket-id "<bucket-id>",
 :upload-url
 "https://pod-NNN.backblaze.com/b2api/v2/b2_upload_file/f-/c-_v-_t-"}
```

## 3단계 : b2_upload_file 요청

위에서 받은 결과값에서, `authorization-token`, `upload-url` 를 사용한다.

```clojure
(defn sha1-str [bytes]
  (->> (-> "sha1"
           java.security.MessageDigest/getInstance
           (.digest bytes))
       (map #(.substring
              (Integer/toString
               (+ (bit-and % 0xff) 0x100) 16) 1))
       (apply str)))

(defn upload-file [base-64 {:keys [token api-url file-name]
                            :or   {file-name "image.png"}
                            :as   _opts}]
  (let [file-bytes (base64/decode (.getBytes base-64))
        req-opts   {:headers {"Authorization"     token
                              "Content-Type"      "b2/x-auto"
                              "X-Bz-File-Name"    file-name                            
                              "X-Bz-Content-Sha1" (sha1-str file-bytes)}
                    :body    file-bytes}]
    (-> (hato/post api-url req-opts)
        :body
        (json/read-str :key-fn csk/->kebab-case-keyword))))
```

정상적으로 이미지가 업로드가 되면 다음 결과를 볼 수 있다. 

```clojure
{:legal-hold {:is-client-authorized-to-read true, :value nil},
 :file-id "<file-id>",
 :server-side-encryption {:algorithm nil, :mode nil},
 :file-info {},
 :account-id "<account-id>",
 :content-length 941,
 :file-name "<file-name.png>",
 :content-sha-1 "1d114e4981aaaaaaed692c9ab312b2bcacea0",
 :content-type "image/png",
 :action "upload",
 :bucket-id "<bucket-id>",
 :content-md-5 "7ecdcc00cd50ecb2fffff43077287878",
 :file-retention {:is-client-authorized-to-read true, :value {:mode nil, :retain-until-timestamp nil}},
 :upload-timestamp 1688775435757}
```

모두 완료되었으면, 웹 페이지로 돌아가서 이미지 파일이 있는지 확인하자.


## 링크

-  https://www.backblaze.com/
- https://www.backblaze.com/apidocs/b2-authorize-account
- https://www.backblaze.com/apidocs/b2-upload-file
