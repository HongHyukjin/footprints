
카테고리별 로직은 지금 문제 없이 잘 돌아가는 상황  
문제점은 지금 post를 가져오는 개념이 post.member.area_name 이라 사용자의 지역 리스트뷰를 보임  
성북구 주민 -> 구로구 음식 배달 불가  
우리가 회의 해서 결국 본인 이외의 지역도 볼 수 있도록 구현 하기로 했는데  
현재 entity로는 타 지역의 리스트뷰 보는 로직 짜기 힘듦  
따라서 post에 post_area 컬럼 추가 해서 그 지역만을 위한 리스트를 짤 수 있도록 코드를 수정하고 싶음  
<br>
백에서는 POST 도메인에 컬럼 하나 추가하면 로직 바꾸는게 어렵지 않을 것 같고,
<br>
프론트는 할 게 좀 많음
create 과정에서 핀 꽂으면 위도 경도 -> 서울 성북구 정릉동 으로 찍는 코드 필요    
찾아보니 카카오 API 관련해서 아래 링크에 자세히 설명 되어 있음    
https://apis.map.kakao.com/web/sample/coord2addr/       
<br>
근데 서비스 라이브러리를 불러오지 못해서 Geocoder()이 안 먹음  
찾아보니 아래 링크에 자세히 설명되어 있음  
https://devtalk.kakao.com/t/geocoder/36665  
<br>
서비스 라이브러리 불러오는 코드는 찾아보니 다음과 같음  
https://apis.map.kakao.com/web/guide/#whatlibrary  
<script type="text/javascript" src="//dapi.kakao.com/v2/maps/sdk.js?appkey=APIKEY&libraries=services"></script>
<br>
근데 Vue.js는 싱글 파일 컴포넌트라 그래서인지 잘 모르겠지만 여러개의 스크립트 파일이 안 넣어짐   
Webpack에 넣으려는 시도를 해봤으나 아직 좀 버거운 상태  
<br>
이거만 해결하면 일단 POST를 우리가 원하는 엔티티로 넘길 수 있음
<br>
<br>
권한에 따른 공지사항 작성 위해 관리자 회원가입 시 role 부여 필요<br>        
마이페이지 작업 필요 지역 추가 개념 넣고 필요시 토큰에 지역1,2,3 추가<br>                
Security 관련 정확한 개념 정립 필요<br>           

## Member 영역 
⭐ **개발 및 수정**
- [x]  회원가입 성공 로그 찍는게 회원가입 시점 이전에 이루어짐  
- [x]  회원가입 지역 설정 API 문제 수정안되면 주소 찍고 뒤에 삭제하는 방향으로 일단 해놔야할듯
- [x]  회원가입 실패 시의 에러처리 필요
- [x]  로그인 시 콘솔창에 비밀번호를 찍고 있음
- [x]  아이디 찾기 과정에서 닉네임이 아닌 이메일을 찍고 있음 
- [x]  존재하지 않는 아이디를 찾을 경우 blabla를 찍고 있음
- [x]  비밀번호 찾기 과정에서 store에 있는 id값을 통해 비밀번호를 전송하는 로직으로 잘못 구현되어있음
- [x]  비밀번호 변경 후 기존 비밀번호로만 로그인 되는 문제 발견(비밀번호 변경 시 기존 데이터 삭제하면 해결 가능)
- [x]  비밀번호 변경시 존재하지 않는 이메일에 대한 예외 처리 안되있음
- [x]  아이디 찾기 과정에서 전에 찾았던 결과를 찍고 있음, 비밀번호도 마찬가지   
- [x]  회원가입, 로그인 엔터버튼 기능 구현  
- [x]  회원가입 닉네임 중복확인 에러 발견  
- [x]  회원가입 닉네임 입력창에 이미 있는 닉네임 입력 시 바로 적용이 아닌 한번 더 클릭시 적용     
- [x]  회원가입 과정에서 이메일 직접입력 창 넣기  
- [x]  이메일 직접 입력 시 중복 체크 안되는 에러   

📍**To Do**
- [ ]  비밀번호 변경 시 디비의 기존 데이터 삭제 안되는 오류 발견 
- [ ]  로그아웃 -> WhitePageError 이슈 발견
<br>

## Post 영역 
⭐ **개발 및 수정**
- [x]  리스트뷰 불러오는 API 백 쪽에서 지역 이름 파라미터로 받아오도록 코드 수정 필요
- [x]  로그인 하지 않아도 글을 등록할 수 있는 에러 - 로그인 상태 유지 토큰으로 해결이 가능한지?
- [x]  마감기한 UI 한글 추가하는 것은 어떤지? 
- [x]  상세페이지에서 리스트뷰로 돌아가는 고정버튼? 구현   
- [x]  지역별 첫번째 포스트가 보이지 않는 에러 발견  
- [x]  상세 페이지에서 보이는 이미지가 리스트뷰에서는 공백으로 보이는 이슈  
- [x]  마감기한 지났을때 글 삭제되도록 설정할 것인지? 안 하기로~   
-  리스트뷰의 게시물 정렬 버튼 2개(카테고리별, 시간별) 로직 구현    

📍**To Do** 
- [ ]  배달리스트뷰 상태에서 새로고침 하는 경우 JSON 형태로만 출력되는 에러 발견 - 에러페이지로 가는 로직 추가해야함
- [ ]  글 내용이 긴 경우 리스트뷰에서 보일 때 보이는 최대치 설정 필요할듯
- [ ]  글 작성 페이지에서 로그 아웃하면 권한 없으니 에러 페이지로 가야함
<br>

## 전체 영역 
⭐ **개발 및 수정**
- [x]  @Id로 PK 지정, @GeneratedValue로 키 생성 전략 명시
- [x]  @Builder의 정확한 역할이 무엇인지?  
- [x]  글 작성할때 제목이 동일해도 괜찮은지? 아니라면 중복체크 해줄것인지  
- [x]  service 폴더 합체    
- [x]  시간이 어느정도 지나면 로그인이 풀리는 에러 발견
- [x]  HTTP 상태코드가 지금 뒤죽박죽인거 같아서 회의 후 통일하는 방식으로 가는게 맞을듯
- [x]  에러페이지 로직 추가     
  
📍**To Do**
- [ ]  어느 페이지에서든 새로고침 시 로그인 풀리는 에러 발견
  
<br>
