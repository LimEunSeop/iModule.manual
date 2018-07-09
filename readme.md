- [imodule 메뉴 ](#imodule)
    - [메뉴 종류 ](#)
        - [1차 메뉴 - Menu](#1----menu)
        - [2차 메뉴 - Page](#2----page)
        - [3차 메뉴 - View](#3----view)
    - [메뉴 설정 항목 ](#)
        - [메뉴코드 ](#)
        - [컨텍스트 종류 ](#)
    - [주소체계 ](#)
- [iModule Source Code](#imodule-source-code)
    - [디렉터리 ](#)
    - [주요 코어 ](#)
        - [iModule (루트 코어) ](#imodule)
        - [Module (모듈 코어)](#module)
        - [Event (이벤트 코어)](#event)
    - [iModule 상수 ](#imodule)
    - [Rewrite Rule](#rewrite-rule)
    - [iModule 에서 사용되는 정규식 패턴 ](#imodule)
    - [iModule 루트 코어에서 사용되는 DB 테이블](#imodule----db)
        - [site_table](#site-table)
        - [sitemap_table](#sitemap-table)
        - [article_table](#article-table)
    - [iModule 모듈 코어에서 사용되는 DB 테이블](#imodule----db)
        - [module_table](#module-table)
    - [iModule 코어 생성, 렌더링 과정 ](#imodule)
        - [1. Request 받고 Rewrite 모듈 동작](#1-request--rewrite)
        - [2. index.php 실행](#2-indexphp)
    - [모듈이 불려지는 방식 ](#)
    - [이벤트 ](#)
        - [이벤트 처리방식 ](#)
        - [이벤트 구독방법 ](#)
            - [1. 이벤트 핸들러 등록 ](#1)
            - [2. package.json 파일에 이벤트 동작정보 상세명세](#2-packagejson)
        - [주요 이벤트 ](#)
    - [process (오버뷰. 보완해야함)](#process)
        - [프로세스 등록 ](#)
        - [프로세스 처리 절차 ](#)
    - [Api 요청 처리 방식 (오버뷰. 보완해야함) ](#api)
        - [Api 등록 ](#api)
        - [Api 요청 처리 절차 ](#api)
    - [모듈의 기본 구조(준비중) ](#)
        - [모듈 만드는법.](#)
        - [Method list](#method-list)
        - [ob_start, ob_get_clean vs ob_start, ob_get_contents + ob_end_clean 용법](#ob-start--ob-get-clean-vs-ob-start--ob-get-contents-ob-end-clean)
            - [ob_get_clean 과 ob_get_contents + ob_end_clean 의 차이](#ob-get-clean--ob-get-contents-ob-end-clean)
    - [jQuery 메소드의 종류](#jquery)
    - [jQuery 객체인가 아닌가](#jquery)
    - [jQuery each 메소드 쓰는법](#jquery-each)
    - [foreach ~ as](#foreach-as)
    - [로그인 모달 불러올 때 주의사항](#)
    - [로그인창 뿌리는법 2가지](#2)
        - [에러로 불러오는법](#)
        - [모달로 불러오는법](#)
    - [Feedback](#feedback)
    - [$_CONFIGS](#configs)
    - [echo와 exit의 차이](#echo-exit)
    - [Ext.JS 정리](#extjs)
        - [Ext.getCmp(cmp_id)](#extgetcmpcmp-id)
        - [Ext.TabPanel](#exttabpanel)
            - [속성](#)
                - [id](#id)
                - [border](#border)
                - [tabPosition](#tabposition)
                - [items](#items)
        - [Ext.grid.Panel](#extgridpanel)
            - [속성](#)
                - [id](#id)
                - [title](#title)
                - [border](#border)
                - [tbar](#tbar)
                - [store](#store)
                - [columns](#columns)
                - [selModel](#selmodel)
                - [bbar](#bbar)
                - [listeners](#listeners)
        - [Ext.data.JsonStore](#extdatajsonstore)
            - [속성](#)
                - [proxy](#proxy)
                - [remoteSort](#remotesort)
                - [sorters](#sorters)
                - [autoLoad](#autoload)
                - [pageSize](#pagesize)
                - [fields](#fields)
                - [listeners](#listeners)
        - [columns 속성](#columns)
                - [text (String)](#text-string)
                - [width (Number)](#width-number)
                - [flex (Number)](#flex-number)
                - [sortable (Boolean)](#sortable-boolean)
                - [dataIndex (String)](#dataindex-string)
                - [renderer (function(value, metaData))](#renderer-functionvalue--metadata)
        - [Ext.PagingToolbar](#extpagingtoolbar)
            - [속성](#)
                - [store](#store)
                - [displayInfo (Boolean)](#displayinfo-boolean)
                - [items](#items)
                - [listeners](#listeners)
        - [xtype](#xtype)
    - [Board 모듈의 admin/scripts/script.js 구조](#board--admin-scripts-scriptjs)
        - [list - add window 속성](#list---add-window)
        - [list - view window 속성](#list---view-window)
        - [category - add window 속성](#category---add-window)
    - [Window 속성](#window)
        - [id](#id)
        - [title](#title)
        - [modal](#modal)
        - [width](#width)
        - [height](#height)
        - [border](#border)
        - [~~autoScroll~~](#autoscroll)
        - [items](#items)
        - [buttons](#buttons)
        - [listeners](#listeners)
    - [db insert 메서드 반환값](#db-insert)
    - [>## 잘 모르는 개념](#)
# imodule 메뉴 <!-- ab -->
크게 2차 depth의 메뉴가 있으나, 2차 메뉴에 모듈을 부착할 시, 최대 3차 메뉴까지 존재.

## 메뉴 종류 <!-- a -->

### 1차 메뉴 - Menu
2차 메뉴들을 그룹핑함과 동시에 2차메뉴에 링크를 연결하는 기능을 담당함. 흔히 웹사이트에 가장 맨 위의 메뉴를 생각하면 편함.

### 2차 메뉴 - Page
1차 메뉴의 컨텍스트 종류가 서브페이지 일 때 생기는 메뉴.

### 3차 메뉴 - View
2차 메뉴의 컨텍스트 종류가 모듈 일때 생기는 메뉴.

## 메뉴 설정 항목 <!-- b -->
### 메뉴코드 <!-- c -->
접근주소(Path)를 결정

### 컨텍스트 종류 <!-- d -->
메뉴의 유형을 정의
- 외부페이지 : 정형화할수 X, 하드코딩하여 새로 html 구성. 외부페이지 항목에서 디스플레이 할 php파일을 선택한다.
- **서브페이지** : 2차메뉴를 구성하고, 그 중 하나를 강제로 할당 (1차메뉴 누르면 디폴트로 display되는 메뉴)
- 위젯 : 위젯으로 이루어진 페이지. (거의 안씀)
- 링크 : 외부링크
- 빈페이지 : 아무것도 없는 빈 페이지
- 모듈 : 2차메뉴부터 적용할 수 있음. 특정한 기능(ex. 게시판), CTL을 붙여주고자 할 때.

>메뉴가 트리형식으로 이루어져 있다고 치면, 서브페이지, 모듈 이외의 것들은 전부 말단노드의 역할을 하고, 서브페이지와 모듈은 다음 레벨(메뉴)로 갈 수 있도록 해주는 노드라고 생각하면 된다.

## 주소체계 <!-- e -->
언어/1차메뉴명/2차메뉴명/(3차 모듈명 - View)/(모듈이 자주 사용하는 파라미터 Ex.index)
>뒤에 필요하면 get 파라미터(Querystring)가 올 수도 있음.



# iModule Source Code
이 부분을 학습하면 알게 될 것들은 다음과 같다.
- iModule 기본적인 디렉터리
- 주요 코어 3가지
- 코어에서 사용하는 상수 및 역할
- Rewrite 란 무엇인지, 프로젝트에 어떤 영향을 끼치는지
- 주요 테이블 4가지
- 코어를 생성하고 렌더링하는 과정
- 모듈을 불러오는 방식
- 이벤트 처리방식, 주요 이벤트, 생명주기
- 모듈을 만들기 위해서 필수적으로 알아야 할 기본 골격(미완성))

## 디렉터리 <!-- f -->
| 디렉터리명 | 설명
| - | - |
| addons | |
| admin | 관리자를 위한 폴더 |
| api | 외부 api를 위한 코드 |
| attachments | 첨부파일. 대학에서는 NAS와 연결해서 symbolic link로 연결함. |
| classes | 코어가 존재하는 폴더 |
| configs | 환경설정 파일 |
| modules | 각종 모듈을 저장하는 폴더 |
| process | 모듈의 doProcess() 메서드를 호출하여 해당 모듈의 process를 처리하게 하는 소스가 들어있는곳. |
| templets | header, footer, 페이지 레이아웃, 외부페이지 등의 범용템플릿 & 기타템플릿이 들어있는 폴더 |

## 주요 코어 <!-- g -->
루트 코어인 iModule, 모듈 코어인 Module, 이벤트 코어인 Event 3가지가 있다.

### iModule (루트 코어) <!-- h -->
- 모듈 끼리의 연결과 페이지 렌더링, 이벤트 발생 등을 중재하여, 관리자로서의 역할을 총괄하는 루트 코어이다.
- iModule.class.php 파일에 정의되어있다.
- 도메인(domain), 언어(language), 1차메뉴(menu), 2차메뉴(page), 3차메뉴(view), idx를 코어가 관리하면서 모듈로 던져주고, 모듈은 값을 받아 적절하게 화면을 구성한다.

### Module (모듈 코어)
- 모듈을 불러오고 모듈과 관련된 변수를 관리하는 코어이다.
- Module.class.php 파일에 정의되어있다.
- 루트 코어에서 렌더링 작업 시, 2차메뉴의 종류가 Module일 경우, 이 코어를 이용하여 모듈을 로드하는 작업을 하게 된다.

### Event (이벤트 코어)
- 실제 이벤트를 발생시키는 코어이다.
- Event.class.php 파일에 정의되어있다.
- 루트 코어가 특정 지점에서 이벤트를 발생시키고 싶을 때, 이벤트의 종류와 발생지점을 명세하면 이것에 이벤트코어에 전달되어 이벤트가 발생된다.

> 루트 코어에서 모듈 코어와 이벤트 코어에 관한 소스를 모두 담기에는 양이 방대할 뿐더러 소스코드의 모듈화에 적합하지 않다. 따라서 Module 과 Event 클래스를 새로 정의하며 기능을 분리하였으며, 모듈 코어와 이벤트 코어는 루트코어에서 모듈과 이벤트에 관한 부분을 대신 처리해주는 알바생 정도로 생각하면 좋다.

## iModule 상수 <!-- i -->
iModule 에서 사용되는 상수는 상수명 앞뒤를 언더바 2개 감싼 형태로 정의한다. (\_\_[상수명]\_\_)

- **\_\_IM\_\_** : iModule core 에 의해 PHP 파일이 실행되었는지 여부를 확인하는 상수
- **\_\_IM\_VERSION\_\_** : iModule 버전을 정의하는 상수. 빌드날짜는 포함되지 않음
- **\_\_IM\_DB\_PREFIX\_\_**: iModule에서 생성되는 모든 DB 테이블앞에 붙는 PREFIX를 정의하는 상수 (단, 각 모듈별로 DB_PREFIX를 재정의할 수 있음)
- **\_\_IM\_PATH\_\_** : $_SERVER['DOCUMENT_ROOT'] 를 포함하는 iModule 이 설치되어 있는 서버상의 경로
- **\_\_IM\_DIR\_\_** : $_SERVER['DOCUMENT_ROOT'] 를 포함하지 않는 iModule 이 설치되어 있는 웹브라우저상의 경로

## Rewrite Rule
아파치에서는 기본적으로 Rewrite Module 이 내장돼있는데, 이름 그대로 path를 받으면 이를 새로운 path 로 다시 write 한다. 아파치서버에서 이 모듈을 Load하면, Request 가 왔을 시 Document Root 의 .htaccess 파일을 읽고나서 정해진 rule 대로 웹 서버가 읽을 수 있는 새로운 path 를 반환해준다.
> 여기서는 .htaccess 의 기본적인 몇가지 정규식 패턴 룰만 소개하고 자세한 분석은 .htaccess 에서 하기로 한다.

## iModule 에서 사용되는 정규식 패턴 <!-- ㅓ -->
| 패턴기호 | 설명 | 예시 |
| - | - | - |
| ^ , $ | ^ : 문자열의 첫 문자(열), $ : 문자열의 끝 문자(열) | ^admin : 처음에 admin이 와야함. ?$ : 마지막에 물음표로 끝남 |
| + , * | + : 1번 이상 와야함, * : 0번 이상 와야함 | a+ : a가 1번 이상, a* : a가 0번 이상(없어도됨) |
| {} | {n} : n번 이상 {0, 1} : 0번 혹은 1번 | a{2} : a가 2번 이상 |
| [ ] | 안에 오는 문자열 무엇이든 와도 됨. | [a-z]{2} : a~z문자가 2개이상 와야함
| [^ ] | 안에 오는 문자열은 오면 안됨 | [^/\\.]+ : / 와 . 은 하나라도 포함되면 안됨 |
| ( ) | 패턴에 매치되는 문자열은 원래 전체가 되는데, 이렇게 괄호로 붙여서 그룹화 하여 그룹을 받을 수도 있다. 괄호를 묶은 순서대로 $1, $2... 로 부를수 있다. | https://snu.ac.kr/history?query=test 라는 요청을 보내면 ^([a-z]{2})/([^/\\.]+)/?$ index.php?language=$1&menu=$2&%{QUERY_STRING} 의 rewrite된 결과는, index.php?language=ko&menu=history&query=test 이다. |

## iModule 루트 코어에서 사용되는 DB 테이블
package.json 파일에 site_table, sitemap_table, article_table 이라고 정의돼있는 테이블이 루트 코어에서 사용되는 테이블이고, 각각 DB에 ```im_site_table```, ```im_sitemap_table```, ```im_article_table``` 이라고 저장된다.

### site_table
http://localhost/admin/configs/sites 에서 관리되며, 추가된 사이트가 저장되는 테이블이다.
![site_table](https://user-images.githubusercontent.com/34618693/40643516-5e4ee6b4-635b-11e8-9a4f-9809ef673817.png)

### sitemap_table
http://localhost/admin/configs/sitemap 에서 관리되며, 사이트맵 정보가 저장되는 테이블이다.
![sitemap_table](https://user-images.githubusercontent.com/34618693/40643523-6224fa12-635b-11e8-8a43-e7cbc49d0f29.png)

### article_table
http://localhost/admin/modules/board/ModuleBoardList 에서 관리되며, 게시판 모듈의 게시글 리스트가 저장되는 테이블이다.
![article_table](https://user-images.githubusercontent.com/34618693/40643531-66c4b01c-635b-11e8-9c50-1288e3bb8d40.png)

## iModule 모듈 코어에서 사용되는 DB 테이블
package.json 파일에 module_table 이라고 정의돼있는 테이블이 모듈 코어에서 사용되는 테이블이고, DB에 im_module_table 이라고 저장된다.

### module_table
관리자 페이지에서 모듈을 더블클릭하면 일부 필드를 관리할 수 있다. (나머지 필드등은 아직 제대로 잘 모르니 나중에 학습하도록)
![module_table](https://user-images.githubusercontent.com/34618693/41083883-606a7fc6-6a6d-11e8-9949-54e10fd5ad76.png)

## iModule 코어 생성, 렌더링 과정 <!-- k -->
일반적으로 메뉴접근시, Rewrite에 의해 Document Root의 index.php 가 실행되고, 전달된 파라미터로 iModule을 생성하고 doLayout을 통해 페이지가 렌더링되는 방식으로 이루어진다.

### 1. Request 받고 Rewrite 모듈 동작
1차메뉴, 2차메뉴, 3차메뉴, index 등이 path로 전달되면, Rewrite 모듈에 의해 내부적으로 Document Root 의 index.php 을 get 방식으로 호출한다. 결국은 모든 메뉴를 클릭해도 Document Root 의 index.php 이 실행되는 것이다.

### 2. index.php 실행
get 파라미터를 넘겨받아, $_REQUEST['menu'], $_REQUEST['page'], $_REQUEST['view'], $_REQUEST['idx'] 에 알맞은 값이 체워졌을 것이다. 여기서는 정말 간단하다. $_REQUEST 를 간직한 상태로 다음 두 줄을 실행한다.
```php
$IM = new iModule();
$IM->doLayout();
```
iModule 코어객체 생성 시, $_REQUEST 값을 필드에 세팅하고, 그 값을 토대로 doLayout 메서드를 호출하여 렌더링을 하게 된다.

## 모듈이 불려지는 방식 <!-- l -->
루트코어의 doLayout 메서드 내에서 컨텍스트를 가져올 때 page의 타입이 MODULE일 경우 모듈을 불러오고 모듈의 템플릿을 렌더링 하게 된다.

## 이벤트 <!-- m -->
### 이벤트 처리방식 <!-- n -->
루트 코어가 특정 지점에서 이벤트를 발생시키고 싶을 때, fireEvent메소드에 이벤트의 종류와 발생지점을 명세하여 호출하면 이벤트 코어에 전달되어 이벤트가 발생된다.

### 이벤트 구독방법 <!-- o -->
#### 1. 이벤트 핸들러 등록 <!-- p -->
이벤트 핸들러를 모듈폴더의 events 폴더 안에 ```이벤트명.php``` 라는 이름으로 생성하고 그 안에 이벤트 처리 코드를 담으면 된다.

#### 2. package.json 파일에 이벤트 동작정보 상세명세
모듈 폴더의 package.json 파일의 "targets" 라는 속성에 ```"이벤트발생한모듈": { "이벤트명": "이벤트발생시킨객체" }``` 형식으로 이벤트를 구독하고 싶을때마다 추가하면 프로그램에서 자동적으로 Event 코어객체의 listeners 배열에 ```listeners["이벤트발생한모듈"]["이벤트명"]["이벤트발생시킨객체"]``` 에 ```"module/모듈명"```이라는 값을 추가한다.

### 주요 이벤트 <!-- q -->
| 이벤트명 | 설명 | 위치 |
| - | - | - |
| beforeGetContext | Page Context를 가져오기 전에 발생하는 이벤트 | 루트 코어의 doLayout 메서드 |
| afterGetContext | Page Context를 가져오고 나서 발생하는 이벤트 | 루트 코어의 doLayout 메서드 |
| afterDoLayout | Context Layout과 Footer, Header 를 Page Context에 결합시키고 나서 발생하는 이벤트 | 루트 코어의 doLayout 메서드|
| beforeDoProcess | 클라이언트로부터 받은 요청을 실행하기 전에 발생하는 이벤트 | 모듈클래스의 doProcess 메서드 |
| afterDoProcess | 클라이언트로부터 받은 요청을 실행하고 나서 발생하는 이벤트 | 모듈클래스의 doProcess 메서드 |
| beforeGetApi | api 코드 실행 전 발생하는 이벤트 | 모듈클래스의 getApi 메서드 |
| afterGetApi | api 코드 실행 후 발생하는 이벤트 | 모듈클래스의 getApi 메서드 |
>이벤트의 상세 조건 (ex. 어떤 이름의 변수가 필요한지 등등..)이 필요할 때는 이벤트 발생지점에 가서 어떤 매개변수로 fireEvent메서드를 호출하는지 확인해도 좋고, Event 클래스의 execEvent메서드에서 이벤트명에 따라 어떤이름의 변수를 세팅하는지 관찰하면 된다.

## process (오버뷰. 보완해야함)
유저는 회원가입을 하고 글을 쓰고 수정하고 저장하는 일련의 행동을 서버에게 요구하는데, 그러한 백엔드 처리를 iModule에서는 ```모듈폴더/process/``` 디렉터리에 모두 등록한다.

### 프로세스 등록 <!-- r -->
모듈의 process 폴더에 ```액션명.php``` 파일 생성후, 프로세스 로직을 작성한다. 액션명 앞에 @가 붙은 경우, 이는 관리자권한이 필요한 프로세스이다.

### 프로세스 처리 절차 <!-- s -->
유저가 브라우저 상에서 요청을 하게 되는데 이 때의 요청 Url의 규칙은 ```도메인/process/언어/모듈명/액션명/(index/?Querystring)``` 이다. 서버가 요청을 받으면 RewriteRule에 의해 process/index.php 파일에 GET 파라미터로 언어, 모듈명, 액션명(, 인덱스, Querystring)이 들어간다. @가 붙은 프로세스인 경우, 관리자권한을 확인하고, 통과되면 process/index.php 파일에서 모듈을 불러와 doProcess 메서드를 호출하게 되고 그 안에서 액션명을 읽어 해당 process 의 코드를 불러들인다.


## Api 요청 처리 방식 (오버뷰. 보완해야함) <!-- t -->
iModule은 ```모듈폴더/api``` 에 api 처리 로직을 등록한다.

### Api 등록 <!-- u -->
모듈의 api 폴더에 ```api명.METHOD.php``` 형태로 등록한다.

### Api 요청 처리 절차 <!-- v -->
브라우저 상에서의 요청 Url 규칙은 ```도메인/모듈명/api명/(인덱스명/?Querystring)```이다. 서버가 요청을 받으면 RewriteRule에 의해 마찬가지로 api.index 파일에 GET 파라미터로 모듈명, api명, (인덱스명, Querystring)이 전달된다. api/index.php 파일에서 모듈을 불러와 getApi 메서드를 호출하게 되고 그 안에서 api명과 method를 조합하여 api 코드를 불러들인다.

## 모듈의 기본 구조(준비중) <!-- w -->
### 모듈 만드는법.
1. example1 모듈 복사해서 가져오고, module의 package.json과 template의 package.js를 나한테 맞게 바꾼다.
2. 모든 폴더를 뒤져가며 example1이라 돼있는 것은 내 모듈 이름으로 전부 바꾼다.
### Method list
| 메소드명 | 기능 |
| - | - |
| getModule | 모듈 코어 클래스를 반환한다. |
| db | 모듈 설치시 정의된 DB코드를 사용하여 모듈에서 사용할 전용 DB클래스를 반환한다. |
| getTable | 모듈에서 사용중인 DB테이블 별칭을 이용하여 실제 DB 테이블 명을 반환한다. |
| getUrl | URL 을 가져온다. |
| getView | view 값을 가져온다. |
| setView | view 값을 변경한다. |
| getIdx | idx 값을 가져온다. |
| getApi | API 실행한다. |
| getConfigPanel | [사이트관리자] 모듈 설정패널을 구성한다.(모듈 더블클릭->모듈설정 화면) |
| getAdminPanel | [사이트관리자] 모듈 관리자패널을 구성한다.(왼쪽 탭들 누르면 나오는 화면) |
| getContexts | [사이트관리자] 모듈의 전체 컨텍스트 목록을 반환한다. |
| getContextTitle | 특정 컨텍스트에 대한 제목을 반환한다. |
| getContextConfigs | [사이트관리자] 모듈의 컨텍스트 환경설정을 구성한다. |
| getContextBadge | 사이트맵에 나타날 뱃지데이터를 생성한다. |
| getText | 언어셋파일에 정의된 코드를 이용하여 사이트에 설정된 언어별로 텍스트를 반환한다. |
| getErrorText | 상황에 맞게 에러코드를 반환한다. |
| getTemplet | 템플릿 정보를 가져온다. |
| getContext | 페이지 컨텍스트를 가져온다. |
| getHeader | 컨텍스트 헤더를 가져온다. |
| getFooter | 컨텍스트 푸터를 가져온다. |
| getError | 에러메세지를 반환한다. 내부에서 getErrorText를 호출한다. |
| getXXXXContext | 템플릿에 정의해 놓은 컨텍스트를 불러들여와 반환한다. $this->db() 를 이용하여 db처리도 OK |
| doProcess | 현재 모듈에서 처리해야하는 요청이 들어왔을 경우 처리하여 결과를 반환한다. |

### ob_start, ob_get_clean vs ob_start, ob_get_contents + ob_end_clean 용법

- 컨텍스트 html 소스를 가져오고자 할 때, Templet 코어에서 getContext 메서드 안에서, 해당 템플릿의 php 파일을 INCLUDE 하게 된다. 그러나 이게 그냥 INCLUDE 한다고 될 일인가?? INCLUDE 는 그냥 PHP 소스를 라인에 포함하는 역할밖에 하지 않는다.
- 템플릿의 php 파일은, html + php 소스의 혼합형태로 되어있지만, 이는 내부적으로 html은 echo 부분으로 치환하고 php 소스는 그대로 두는 형태를 띌 것이다. 결국에는 echo 로 response body에 데이터를 태워서 소켓에 보낼것이다.
- 우리는 이것을 바로 소켓에 안 태우고 일단은 변수에 담아두고 싶은 것이다. 그러려면 ```ob_start```로 출력버퍼를 열어둔 후 INCLUDE 작업을 하여 출력버퍼에 데이터가 체워졌으면, ob_get_clean() 함수를 호출하여 출력버퍼를 비움과 동시에 그 리턴값을 변수(주로 $html)에 담으면 될 것이다.

#### ob_get_clean 과 ob_get_contents + ob_end_clean 의 차이

아이모듈 소스를 보면, 버퍼를 끝맺을 때 어떤건 ob_get_clean 만 있고 어떤건 ob_get_contents + ob_end_clean 이 같이 쓰여져 있는것을 볼 수가 있다. 차이 없다. 둘의 의미는 같은것이다!!!!!!!!!!

## jQuery 메소드의 종류
jQuery 메소드의 종류에는 크게 2가지가 있습니다.

$ 네임스페이스의 메소드는 주로 utility 타입메소드로 정적메소드 처럼 쓰입니다. ex) $.each()

$.fn 네임스페이스의 메소드는 selector와 함께 쓰이는 메소드 들입니다. ex)$().each()

확장 또는 플러그인 이라고 하는 것은 결국  selector와 함께 쓰일 수 있도록

$.fn 네임스페이스에 메소드를  추가하는 것이라고 볼 수 있겠네요

## jQuery 객체인가 아닌가
$('selector') 의 리턴값이 마냥 jQuery객체 일 수는 없다. $('li')를 했다면, 여러가지 li DOM이 리턴된다. 이것 각각은 DOM이지 jQuery객체가 아니다. 따라서 루프를 돌려서 다시 $를 해주어 jQuery 객체로 만드는 작업을 해줘야 한다.

## jQuery each 메소드 쓰는법
```javascript
$('.list li').each(function (index, item) {
    // 인덱스는 말 그대로 인덱스
    // item 은 해당 선택자인 객체를 나타냅니다.
    $(item).addClass('li_0' + index);
    // item 과 this는 같아서 일반적으로 this를 많이 사용합니다.
    // $(this).addClass('li_0' + index); });
```

이렇게 하거나

```javascript
$('.list li').each(function (index) {

    $(this).addClass('li_0' + index);
```

이렇게. 아예 index마져 없앨 수도 있음. 필요없을때.

## foreach ~ as
```php
$test = array("key1"=>"value1","key2"=>"value2");
$test2 = (object)$test;

foreach ($test as $key=>$value) {
	echo $key, $value;
}

foreach ($test2 as $key=>$value) {
	echo $key, $value;
}
```
PHP 연관배열과 객체 모두 foreach ~ as 구문에 똑같이 적용될 수 있다.

## 로그인 모달 불러올 때 주의사항
그냥 로긴모달을 HTML으로 뿌리기만 한다면 안되고 스크립트로 이벤트 먹여줘야해  
 modules/member/scripts/script.js

## 로그인창 뿌리는법 2가지
에러로 불러오는법, 모달로 불러오는법 2가지가 있다.
### 에러로 불러오는법
return $this->getError('REQUIRED_LOGIN');

### 모달로 불러오는법
스크립트에서 필요할 때 Member.loginModal(callback); 호출하면 된다.

## Feedback
- 프론트엔드에서 ES6 금지. IE10 까지는 맞춰줘야 하기 때문.
- IF(complete='YES', 'NO', 'YES') 같은 DB 함수는 가급적 안쓰는게 좋음. PHP 선에서 불편하더라도 데이터를 받아 처리하자. DB 가 바뀌면 곤란해질 수도 있기 때문에 
- ```$success = $this->db()->update($this->table->todolist, array('complete'=>'NO'))->execute();``` 와 같이 DB가 살아있다는 100% 가정이 있는 쿼리문은 리턴값을 받을 필요가 없다. 사용자가 입력한 것이 있는것도 아니고.. 사용자 입력한 값이 있다면 사전에 PHP에서 확실히 걸러줘야된다!!!!!
- 주석 이쁘게, 개발자 이름 이쁘게, README.md 이쁘게 만들어서 발표회 가지자!!


## $_CONFIGS
init.config.php 에서 iModule.preset.php 에서 세팅된 값이 $_CONFIGS에 세팅된다. 그러나 iModule.preset.php 가 없을 경우, 맨 처음 서버 설정에서 세팅한 DB 주소, 계정, 비밀번호 등이 세팅된다.

## echo와 exit의 차이
```php
function exitInFunction() {
	echo "1";
	exit("2");
	echo "3";
}

echo "echo 테스트";
echo "1"; echo "2";

echo "exit in function";
exitInFunction();

echo "exit 테스트";
exit("1"); exit("2");
```

```
echo 테스트12exit in function12
```

echo는 response body에 차곡차곡 데이터가 쌓이지만, exit는 전체 스크립트 자체를 종료하겠다는 의미이므로, 데이터를 보냄과 동시에 종료한다.

## Ext.JS 정리

### Ext.getCmp(cmp_id)

해당 id의 컴포넌트를 불러오는 함수. add 메소드 체이닝하여 또 다른 컴포넌트를 추가할 수 있다.
id로 컴포넌트를 불러왔을 시에는 보통 이런식으로 컴포넌트로 추가한다.

### Ext.TabPanel
탭을 구성할 수 있는 패널이다.

#### 속성
id, border, tabPosition, items 속성을 주로 쓴다.

##### id
컴포넌트의 id

##### border
테두리를 지정한다. false 일 경우 지정되지 않으며, true일 경우는 cls 속성으로 클래스 지정한 다음, css 에서 해당 클래스를 스타일링 하면 된다.

##### tabPosition
"top"으로 지정하면 탬이 위애, "bottom"으로 지정하면 탭이 아래에 위치한다.

##### items
하위에 들어갈 아이템들을 정의한다. 배열에다가 new Ext.grid.Panel 이런식으로 추가하면 된다. 주로 Exit.grid.Panel이 들어가는것 같으나, 또 뭐가 들어갈 수 있는지는 나중에 차차 알아보기로 하자.

### Ext.grid.Panel
클라이언트 사이트에서 대량의 테이블 데이터를 출력하기에 아주 좋은 컴포넌트이다.

#### 속성
id, title, border, tvar, store, columns, selModel, bbar, listeners 속성을 주로 쓴다.

##### id
컴포넌트의 id

##### title
Ext.grid.Panel이 Ext.TabPanel 안에 들어가있지 않다면, 맨 위에 타이틀이 보일것이다. 하지만, 지금은 Ext.TabPanel 안에 들어가 있으므로, Tab에 타이틀 이름이 들어가있다.

##### border
테두리를 지정한다.

##### tbar
toolbar의 약자로, 배열에 new Ext.Button 을 추가하여 툴바를 구성한다. Ext.Button 은 속성으로 text, iconCls, handler 가 있으며, iconCls에 fontawesome 등의 아이콘을 지정할 수가 있다.

##### store
데이터의 저장소를 정의한다.

##### columns
테이블의 컬럼을 정의한다.

##### selModel
선택방법을 정의한다. Ext.selection 을 문서에 검색하여 적용하고 싶은 클래스를 적당히 카피한 후, new 로 생성하면 된다.

##### bbar
bottom bar 의 약자이다. tbar와 같이 배열이든 뭐든 넣어서 자유롭게 배치한다. 여기서는 Ext.PagingToolbar를 사용하였군...

##### listeners
이벤트 핸들러를 등록한다.
```javascript
listeners: {
    itemdbclick:function(grid, record) { // 더블클릭 이벤트
        Board.list.view(record.data.bid, record.data.title);
    },
    itemcontextmenu:function(grid,record,item,index,e) { // 오른쪽클릭 이벤트
        var menu = new Ext.menu.Menu();

        menu.add('<div class="x-menu-title">'+record.data.title+'</div>');

        menu.add({
            iconCls:"xi xi-form",
            text:"게시판 수정",
            handler:function() {
                Board.list.add(record.data.bid);
            }
        });

        menu.add({
            iconCls:"mi mi-trash",
            text:"게시판 삭제",
            handler:function() {
                Board.list.delete();
            }
        });

        e.stopEvent();
        menu.showAt(e.getXY());
    }
}
```

### Ext.data.JsonStore
JSON 데이터로부터 Ext.data.Store 를 만들어주는 Helper Class 이다.

#### 속성
proxy, remoteSort, sorters, autoLoad, pageSize, fields, listeners 속성을 주로 쓴다.

##### proxy
Ext.data.Model 데이터를 로드하고 저장하기 위한것.

##### remoteSort
true면 서버사이드 소팅, false면 클라이언트에서 소팅한다.

##### sorters
sort 옵션을 설정하기 위한 객체배열값이다.

##### autoLoad
컴포넌트 생성되자마자 자동으로 load 메서드 호출할 것인지 결정한다.

##### pageSize
한 페이지에 보여질 레코드의 갯수를 정의한다.

##### fields
있어도 되고 없어도 된다. 정확히 무슨 역할 하는지 모르겠다. 나중에 다시 알아보자.

##### listeners
이벤트 핸들러를 정의한다. 주로 load:function(store,records,success,e) 형태의 이벤트 핸들러를 정의한다.

### columns 속성
text, width, flex, sortable, dataIndex, render를 주로 사용한다.

##### text (String)
컬럼명

##### width (Number)
컬럼 넓이

##### flex (Number)
전체 flex의 합 중 어느정도의 비율을 차지할 것인지 계산한다. 다수의 컬럼이 flex 속성을 가지고 있어야 한다.

##### sortable (Boolean)
정렬할 수 있는지. 클릭에 반응하여 오른차순, 내림차순 정렬이 가능하다.

##### dataIndex (String)
Ext.data.Model에서 어느 필드를 가져올지 정의한다.

##### renderer (function(value, metaData))
value에 따라 렌더링 방법을 달리하고 싶을때 정의한다.

### Ext.PagingToolbar
Ext.data.Store에 바인딩하여 자동 페이징 제어하는 것에 특화된 툴바

#### 속성
store, displayInfo, items, listeners 등이 쓰인다.

##### store
바인딩할 store를 정의한다.

##### displayInfo (Boolean)
레코드 정보를 보일것인지 정의한다. true일 경우 전체 레코드 중 현재 보이는 레코드의 인덱스가 몇번인지 맨 오른쪽 밑의 텍스트로 알려준다.

##### items
툴바에 들어갈 것들을 정의한다. xtype가 정의된 객체가 들어가며, "->" 는 오른쪽 정렬을 의미한다.

##### listeners
다음과 같이 페이지 정보를 가져오기 위해 스토어를 바인딩하는 로직이 들어간다.
```javascript
beforerender:function(tool) {
    tool.bindStore(Ext.getCmp("ModuleBoardList").getStore());
}
```


### xtype
xtype를 이용하여 객체 선언을 간소화 할 수 있다.

```javascript
new Ext.Button({
    text:"선택 게시판 삭제",
    iconCls:"mi mi-trash",
    handler:function() {
        Board.list.delete();
    }
})
```
가

```javascript
{
    xtype: "button",
    text:"선택 게시판 삭제",
    iconCls:"mi mi-trash",
    handler:function() {
        Board.list.delete();
    }
}
```
으로 쓰일 수 있다.

## Board 모듈의 admin/scripts/script.js 구조
- list
    - add : function(bid) { new Ext.Window({ ... }).show(); }
        - Ext.form.Panel
            - Ext.form.Hidden
            - Ext.form.FieldSet
                - Ext.Form.TextField
                - Ext.Form.TextField
            - Ext.form.FieldSet
                - Admin.templetField
                - Ext.form.FieldContainer
                    - Ext.form.NumberField
                    - Ext.form.NumberField
                - Ext.form.FieldContainer
                    - Ext.form.ComboBox
                    - Ext.form.NumberField
            - Ext.form.FieldSet
                - Admin.templetField??????? 뭐지
            - Ext.form.FieldSet
                - Ext.form.Checkbox
                - Ext.form.Checkbox
            - Ext.form.FieldSet
                - Ext.form.ComboBox
                - Ext.form.ComboBox
            - Ext.form.Hidden
            - Ext.form.FieldSet
                - Ext.grid.Panel
            - Ext.form.FieldSet
                - Admin.permisioonField  12개??
            - Ext.form.FieldSet
                - Ext.form.FieldContainer
                    - Ext.form.DisplayField
                    - Ext.form.DisplayField
                - Ext.form.FieldContainer
                    - Ext.form.NumberField
                    - Ext.form.NumberField
                - Ext.form.FieldContainer
                    - Ext.form.NumberField
                    - Ext.form.NumberField
                - Ext.form.FieldContainer
                    - Ext.form.NumberField
                    - Ext.form.NumberField
    - view : function(bid,title) { new Ext.Window({ ... }).show(); }
       - Ext.Panel
    - delete ; function() { 삭제로직 및 메세지 출력... }

- category
    - add : function(index) { new Ext.Window({ ... }).show(); }
        - Ext.form.Panel
            - Ext.form.TextField
            - Ext.form.FieldSet
                - Admin.permissionField
                - Admin.permissionField
                - Admin.permissionField
                - Ext.Panel
    - delete : function() { 삭제로직 및 메세지 출력... }

**뭐든 작성하려면 일단 Panel 안에 넣어야 한다. Panel 에도 여러 종류가 있는데 여기서는 form을 만드는 것이 목적이므로 Ext.form.Panel 을 선택한다. Ext.form.Panel 아래에는 여러가지의 FieldSet이 들어간다. FieldSet 이란 FieldContainer의 집합이며, 동일한 역할유형을 가지는 필드들의 모임이라 할 수 있다. FieldContainer는 FieldSet 내부의 한 줄을 의미하며, FieldContainer 안에 Ext.form.TextField, NumberField, ComboBox, Checkbox, DisplayField 등이 들어갈 수 있다. FieldSet 안에 꼭 FieldContainer가 들어가는 것은 아니다. 또다른 패널을 새로 구성할 수도 있다.**
### list - add window 속성
id, title, modal, width, border, autoScroll, items, buttons, listeners

### list - view window 속성
id, title, modal, width, height, border, layout, maximizable, items

### category - add window 속성
id, title, modal, width, border, autoScroll, items, buttons

## Window 속성
id, title, modal, width, height, border, autoScroll, items, buttons, listeners 등이 있다.

### id
Window의 id를 정의한다.

### title
Window의 타이틀을 정의한다.

### modal
true 로 하면 주위가 어두워져 모달이 띄워진 효과를 준다.

### width
넓이를 정의한다.

### height
높이를 정의한다.

### border
테두리의 여부를 true, false로 정의한다. 웬만하면 false 로 하는게 이쁘더라.

### ~~autoScroll~~
deprecated 됐다. scrollable 속성을 대신 사용하자.
true일 경우, 오버플로우 시 스크롤을 생성해주고, false인 경우는 스크롤 없이 오버플로우된 부분을 자르기만 한다.

### items
구성요소가 들어간다. 뭐든 먼저 Panel을 먼저 써준다. 컨테이너 역할을 해주나보다. 어떤 Panel을 써야할 지 보려면, 문서에서 Panel을 검색해서 알아서 찾아보도록 하자.

### buttons

### listeners


## db insert 메서드 반환값
pk나 auto increment 를 반환. pk가 없으면 false 반환. insert 할때 데이터 검증을 php 차원에서 잘 하자. 만약 DB 가 에러가 있을것 같다면 getDBError 였나?? 이것을 호출해서 디버깅 차원에서 확인해보자. 실서버에서는 호출하지 않도록.
3항연산자 사용???? 그게 어느경우였지?????


>## 잘 모르는 개념
>매직상수 의미역할, get_defined_vars() 의 역할, 왜 로그인모듈의 login.post, me.get, signup.post 를 api에 빼놨지?? 이게 바로 그 코스모스모듈과 연동하기 위해서인가?? 동영상 다시 보자.
>/modules/admin/includes/index.php iModuleAdminPanel 부분 완전 모르겠다 더 알게될 때 다시 학습