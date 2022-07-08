<template>
  <div>
    <tool-bar></tool-bar>
    <div id="wrap">
      <div id="wrap2">
        <div style="float:left; width:25%; height:100%;">
          <div id="chat_list_header">
            <img src="../assets/people.jpg">
            <h5>밥프렌즈</h5>
          </div>
          <ul class="chat_list">
            <li v-for="(data, index) in chatData" :key=data v-bind:id="[`${index}`]" v-on:mousedown.left="clickLeft"
              v-on:mousedown.right="clickRight" v-on:contextmenu.prevent class="chat_list_link">
              <div class="info_area">
                <img class="info_pic" :src="chatData[index]['src']">
                <h3 class="info_title">{{ chatData[index]['title'] }}</h3>
                <p class="info_message">{{ chatData[index]['lastMessage'] }}</p>
              </div>
            </li>
            <!-- <li v-bind:class=chekcedArr[0]> -->
          </ul>
        </div>
        <div id="wrapRight">
          <div id="content_list_header">
            <img :src="chatData[checkedNum]['src']">
            <h3>{{ chatData[checkedNum]['title'] }}</h3>
            <p v-for="name in chatData[checkedNum]['userNames']" :key=name>{{ name }}</p>
          </div>
          <div id="content_message">
            <!-- <p id="chatname" v-for="chatname in chatData[checkedNum]['chatNames']" :key=chatname>{{ chatname }}</p> -->
            <p id="chatlog" v-for="chatlog in chatData[checkedNum]['chatLogs']" :key=chatlog>{{ chatlog }}</p>
          </div>
          <div id="content_input">
            <input type="text" placeholder="메세지를 입력해주세요." v-on:keyup.enter="sendMessage" v-model="inputText">
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ToolBar from '../components/ToolBar.vue'


export default {
  components: {
    ToolBar,
  },
  data() {
    return {
      dataNum: 0,
      chatData: [
        {
          src: require('../assets/pizza.png'),
          title: '미스터피자',
          userNames: ['홍길동', '김갑동', '김영희', '박철수'],
          chatNames: ['김영희', '박철수', '김갑동', '김영희', '김영희'],
          chatLogs: ['무슨 피자 시킬까요?', '전 불고기 피자가 좋아요.', '하와이안 피자는 어때요?', '음..', 'ㅋㅋㅋㅋ', '그건 쫌..'],
          lastMessage: '',
        },
        {
          src: require('../assets/chicken.png'),
          title: '교촌치킨',
          userNames: ['홍길동', '김지원', '이지원', '박지원'],
          chatNames: ['이지원', '김지원'],
          chatLogs: ['ㅎㅇ', '허니콤보 레드콤보'],
          lastMessage: '',
        },
        {
          src: require('../assets/shavedIce.png'),
          title: '설빙',
          userNames: ['홍길동', '어피치', '네오', '라이언'],
          chatNames: ['어피치', '네오'],
          chatLogs: ['치즈빙수 먹고싶다', 'ㅋㅋㅋ'],
          lastMessage: '',
        },
      ],
      chekcedArr: [],
      checkedNum: 0,
      inputText: "",
    }
  },
  created() {
    this.dataNum = this.chatData.length;

    this.chekcedArr[this.checkedNum] = "on"
    for (var i = 0; i < this.dataNum; i++) {
      if (i != 0) {
        this.chekcedArr[i] = "";
      }
      this.chatData[i]['lastMessage'] = this.chatData[i]['chatLogs'][this.chatData[i]['chatLogs'].length - 1];
    }
  },
  methods: {
    clickLeft(li) {
      for (var i = 0; i < this.dataNum; i++)
        this.chekcedArr[i] = "";
      this.checkedNum = li.currentTarget.id;
      this.chekcedArr[this.checkedNum] = "on";
    },
    clickRight() {
      console.log("clickRight");
    },
    sendMessage() {
      this.chatData[this.checkedNum]['chatNames'].push("홍길동");
      this.chatData[this.checkedNum]['chatLogs'].push(this.inputText);
      this.chatData[this.checkedNum]['lastMessage'] = this.chatData[this.checkedNum]['chatLogs'][this.chatData[this.checkedNum]['chatLogs'].length - 1];
      this.inputText = "";
    }
  }
}
</script>

<style scoped>

#wrap {
  height: 1000px;
  padding: 60px 0;
}
#wrap2 {
  border-radius: 30px;
  background-color: #f9f9f9;
  width: 1300px;
  height: 800px;
  margin: 0px auto;
  text-align: left;
  padding-bottom: 20px;
}
#wrapRight {
  float: right;
  height: 100%;
  width: 74.9%;
  position: relative;
  border-left: 1px solid rgb(220, 220, 220);
}

#chat_list_header {
  height: 80px;
  padding: 0px 0px 0px 80px;
  border-bottom: 1px solid #ddd;
}

#chat_list_header>* {
  float: left;
  margin-bottom: 0px;
}

#chat_list_header>img {
  width: 40px;
  height: 40px;
  margin-top: 20px;
}

#chat_list_header>h5 {
  margin-top: 25px;
  margin-left: 10px;
  font-size: 20px;
}

.chat_list {
  list-style: none;
  margin: 0px 0px;
  padding: 0px 0px;
  height: 100px;
  padding-top: 0px;
}

.chat_list>* {
  border-bottom: 1px solid #ddd;
  cursor: pointer;
}

.on {
  background-color: rgb(234, 234, 234);
}

.info_area {
  height: 100px;
  padding-top: 0px;
}

.info_area>* {
  float: left;
  margin-left: 25px;
  margin-bottom: 0px;
}

.info_pic {
  width: 50px;
  height: 50px;
  margin-top: 25px;
}

.info_title {
  margin-top: 25px;
  width: 200px;
}

.info_message {
  font-size: 13px;
  color: #999;
}


#content_list_header {
  position: absolute;
  top: 0;
  right: 0;
  left: 0;
  height: 80px;
  padding: 0 0;
  border-bottom: 1px solid #ddd;
}

#content_list_header>* {
  float: left;
  margin-bottom: 0px;
  margin-left: 20px;
}

#content_list_header>img {
  margin-top: 20px;
  width: 40px;
  height: 40px;
}

#content_list_header>h3 {
  width: 850px;
}

#content_list_header>p {
  margin-top: 5px;
  font-size: 15px;
}

#content_message {
  margin-top: 80px;
  padding: 5px 15px;
}
#content_message>#chatname{
  margin: 0 2px ;
}
#content_message>#chatlog {
  font-size: 15px;
  border-radius: 20px;
  background-color: rgba(255, 176, 176, 0.61);
  width: 250px;
  padding: 15px 20px;
  margin: 15px 0px;
}

#content_input {
  position: absolute;
  bottom: 0;
  width: 95%;
  height: 120px;
  margin-left: 15px;
  border: 3px solid rgb(189, 189, 189);
  border-radius: 20px;
}

#content_input>input {
  margin: 7px 0 0 5px;
  width: 98%;
  height: 100px;
  vertical-align: top;
  border: none;
  outline: none;
  font-size: 15px;
  background-color: #f9f9f9;
}
</style>