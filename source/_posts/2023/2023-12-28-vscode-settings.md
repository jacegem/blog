---
title: "[vscode] settings.json 파일 나눠서 관리하기"
date: 2023-12-28
tags: [vscode, clojure, vim]
categories:
  - Application
  - Vscode
---


### 문제 
vscode 의 설정들은 settings.json 파일을 통해서 관리된다. 
> ~/Library/Application Support/Code/User/settings.json

![](https://i.imgur.com/NBGmyQJ.png)

vim extension을 설정하다보니, settings.json 파일이 2000천 줄을 넘게되었다.    
수정이 쉽지 않은 상황이 되어서, 이를 관리하기 위해서는 파일 분리가 필요했다. 

![](https://i.imgur.com/NnxV9Jy.png)

### 해결

clojure로 설정내용을 나눠서 관리한다. 
모두 관리하지는 않고, vim 관련 설정만 분리해서 관리한다. 

- "vim.insertModeKeyBindingsNonRecursive"
- "vim.normalModeKeyBindingsNonRecursive"
- "vim.visualModeKeyBindingsNonRecursive" 

### deps.edn

```clojure
{:paths ["src" "resources"]
 :deps    {org.clojure/clojure {:mvn/version "1.11.1"}
           org.clojure/data.json {:mvn/version "2.4.0"}}}
```

json 파일을 읽고 쓰기 위해서 `org.clojure/dats.json` 을 사용한다. 

### main.clj

```clojure
(ns settings.vim.main
  (:require [clojure.data.json :as json]
            [settings.vim.mode.insert :refer [vim-insert]]
            [settings.vim.mode.normal :refer [vim-normal]]
            [settings.vim.mode.visual :refer [vim-visual]]))

(defn write-settings []
  (let [file-name "settings.json"
        settings (json/read-str (slurp file-name))
        config (merge settings
                      (vim-insert)
                      (vim-normal)
                      (vim-visual))
        json-str (json/write-str config {:escape-slash false
                                         :escape-unicode true})]
    (spit file-name json-str)))
```

기존 settings.json 파일을 읽고, 설정내용을 `merge` 한 후에 다시 쓰는 코드이다. 

### insert.clj

```clojure
(ns settings.vim.mode.insert
  (:require [settings.vim.mode.util :refer [convert-after]]))

(def before-after
  {[:j :j] :esc
   ["ㅓ" "ㅓ"] :esc})

(defn vim-insert []
  {"vim.insertModeKeyBindingsNonRecursive"
   (convert-after before-after)})
```
insert 모드에서는 jj 입력시에 normal 모드로 변경되도록한다.  


### util.clj

설정을 간단하게 작성하기 위해 중복되는 부분을 제거하려고 만든 함수들이다. 

```clojure
(ns settings.vim.mode.util)

(defn convert-key [list]
  (map (fn [k]
         (let [s (name k)]
           (if (> (count s) 1)
             (str "<" s ">")
             s))) list))

(defn convert-after [list]
  (map (fn [[k v]]
         {:before (-> (if (vector? k)
                        k
                        [k])
                      convert-key)
          :after (-> (if (vector? v)
                       v
                       [v])
                     convert-key)}) list))

(defn convert-commands [list]
  (map (fn [[k v]]
         {:before (-> (if (vector? k)
                        k
                        [k])
                      convert-key)
          :commands (if (vector? v)
                      v
                      [v])}) list))
```


### normal.clj

> 여기서 모두 확인할 수 있습니다. https://github.com/jacegem/vscode-user 

방향키를 변경해서 사용하다보니, 설정내용이 길어졌다. 

```clojure
(ns settings.vim.mode.normal
  (:require [settings.vim.mode.util :refer [convert-after convert-commands]]))

(def before-commands
  {"." "editor.action.quickFix"
   ":" "workbench.action.showCommands"
   "[" "workbench.action.navigateBackInNavigationLocations"
   "]" 	"workbench.action.navigateForwardInNavigationLocations"
   [:b] "editor.action.revealDefinition"
   [:c :n] "clojureLsp.refactor.cleanNs"
   [:c :o] "workbench.action.closeEditorsInOtherGroups"
   [:g :m] "magit.status"
   [:leader :f :a] "clojureLsp.refactor.threadFirstAll"
   [:leader :f :f] "actions.find"
   })

(def before-after
  {"=" :i
   ";" :S-a ;; insert end cursor
   "," :C-d ;; page half down
   "m" :C-u  ;; page half up
   "D-m" :m
   :a :o
   [:c :i "\""] ["\"" "_" :c :i "\""]
   [:d :d] ["\"" "_" :d :d]
   :f [:leader :leader :leader :b :d :w]
   :F [:leader :leader :leader :j]
   :h "^" ;; beginning of line
   :i :k  ;; up
   "ㅑ" :k
   :I :i  ;; insert 
   :j :h  ;; left
   :k :j  ;; down
   :o :i ;; insert
   "ㅐ" :i
   :O :o  ;; insert new line
   :p :S-p
   :U :C-r ;; redo
   :t "%"
   :q :b
   :x  ["\"" "_" :x]
   })

(defn vim-normal []
  {"vim.normalModeKeyBindingsNonRecursive"
   (concat (convert-after before-after)
           (convert-commands before-commands))})
```
 
### visual.clj

visual모드에서 필요한 설정들을 넣는다. 

```clojure
(ns settings.vim.mode.visual
  (:require [settings.vim.mode.util :refer [convert-after convert-commands]]))

(def before-commands
  {">" "editor.action.indentLines"
   "<" "editor.action.indentLines"
   :w "paredit.sexpRangeExpansion"
   })

(def before-after
  {";" "$"
   "," :C-d
   :h "^"
   :i :k ;; beginning of line
   "ㅑ" :k  ;; up
   :j :h
   :k :j  ;; left
   :m :C-u   ;; down
   [:p] [:p :g :v :y] ;; page half down
   :s :S  ;; page half up
   :t "%"
   })

(defn vim-visual []
  {"vim.visualModeKeyBindingsNonRecursive"
   (concat (convert-after before-after)
           (convert-commands before-commands))})
```

### 결론

클로저로 설정관리를 하면서 중복된 설정들을 제거할 수 있게 되었다. 
다른 설정들도 분리해서 관리할 수는 있으나, 아직 거기까지를 필요없어서, `vim`만 진행하였다. 
