<template>
  <div>
    <form ref="form" @submit.prevent="sendEmail">
      <label>Email</label>
      <input type="email" v-model="email" name="user_email">
      <input type="submit" value="Send">
    </form>

    {{TimerStr}}
    {{TimeCounter}}

    <form>
      <label>인증번호</label>
      <input v-model="userCode" maxlength="6">
      <button v-on:click="checkCode" type="button">확인</button>
    </form>
  </div>
</template>


<script>
import emailjs from 'emailjs-com';

export default {
  name: "TimerName",
  data: function(){
    return {
      email: "",
      sysCode: "",
      userCode: "",
      TimerName: null,
      TimeCounter: 10,
      TimerStr: "00:10" }
  },
  methods: {
    sendEmail: function() {
      this.sysCode = Math.floor(Math.random() * 900001) + 100000;
			let templateParams  = { 
				user_email: this.email,
				sys_code: this.sysCode,
			}
      
      console.log(this.sysCode);
      emailjs.init('REMuhzEQAisDSZ2hk');
			emailjs.send('email', 'template_q0r3oy4', templateParams)
      .then((result) => {
          console.log('SUCCESS!', result.text);
      }, (error) => {
          console.log('FAILED...', error.text);
      });

      if(this.TimerName != null){
        this.timerStop(this.TimerName);
        this.TimerName = null;
      }
      this.TimerName = this.timerStart();
    },
    timerStart: function(){ // 1초에 한번씩 start 호출
      this.TimeCounter = 10;
      var interval = setInterval(() => {
        this.TimeCounter--; //1초씩 감소
        this.TimerStr = this.prettyTime();
        if (this.TimeCounter <= 0)
          this.timerStop(interval);
      }, 1000);
      return interval;
    },
    timerStop: function(TimerName) {
      clearInterval(TimerName);
      this.sysCode="abcdefghijklmnop";
      this.TimeCounter = 0;
      console.log("타임 아웃!!", this.sysCode);
    },
    prettyTime: function() { // 시간 형식으로 변환 리턴
      let time = this.TimeCounter / 60;
      let minutes = parseInt(time);
      let secondes = Math.round((time - minutes) * 60);
      return ( minutes.toString().padStart(2, "0") + ":" + secondes.toString().padStart(2, "0") );
    },
    checkCode: function() {
      if(this.sysCode == parseInt(this.userCode)){
        alert("확인!");
        this.timerStop();
      }
      else {
        alert("실패");
      }
    }
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
