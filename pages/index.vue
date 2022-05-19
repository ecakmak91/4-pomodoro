<template>
  <div class="container" :class="{'resting':restingClass}">
    <header>
    <a class="information-button" @click="informationPopup=true"><svg fill="#fff" width="26" height="26" version="1.1" id="lni_lni-information" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" viewBox="0 0 64 64" style="enable-background:new 0 0 64 64;" xml:space="preserve"><g><circle cx="31.2" cy="6.9" r="3.9"/><path d="M38.7,59.3h-5V20.5c0-2.2-1.8-4-4-4h-4.5c-1,0-1.8,0.8-1.8,1.8s0.8,1.8,1.8,1.8h4.5c0.3,0,0.5,0.2,0.5,0.5v38.7h-5
		c-1,0-1.8,0.8-1.8,1.8s0.8,1.8,1.8,1.8h13.5c1,0,1.8-0.8,1.8-1.8S39.7,59.3,38.7,59.3z"/></g></svg></a>

    </header>
    <div class="timer">
      <div class="clock">
        <h1>Pomodoro Counter</h1>
        {{remainingMinutes}} : {{remainingSecond}}
      </div>
      <button class="startButton" @click="startFunc">{{startText}}</button>
    </div>
    <div class="working-preview">

      <ul class="list">
        <li :key="index" v-for="item,index in history">
          <strong>{{item.text}} :</strong> 
          {{item.time}}
        </li>
      </ul>
    </div>
    <div class="popup information-popup" id="information-popup" :class="{active:informationPopup}" @click="closePopup($event)">
      <div class="overlay">
        <p>Hello! I'm Emre Çakmak from Varulf. I'm front-end developer. I developed this aplication with Vue.js. I'm using Vue.js for three years, still learning so i am developing this application. If you want buy me a coffee for motivation:)</p>
        <a href="https://www.buymeacoffee.com/ecakmak91?new=1" target="_blank" class="buymebutton">Click here</a>
        <h5>Scheduled Updates</h5>
        <ul>
          <li>Notification</li>
          <li>ToDo list</li>
          <li>Add/Edit project</li>
          <li>Reports</li>
          <li>Boss Mode</li>
          <li>Mobile App</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data(){
    return{
      workingTime:25,
      breakTime:5,
      second:0,
      isStart:false,
      interval:null,
      isWorking:true,
      startText:"Start",
      remainingMinutes:"25",
      remainingSecond:"00",
      informationPopup:false,
      history:[]
    }
  },
  methods:{
    minuteToSecond(minute){
      this.second=minute*60
      return this.second
    },
    closePopup(event){
      if(event.target.id=="information-popup")
        this.informationPopup=false
    },
    secondToMinute(second){
      this.remainingMinutes=Math.floor(second/60)
      if(this.remainingMinutes<10){
        this.remainingMinutes=""+"0"+this.remainingMinutes
      }
      this.remainingSecond=second%60
      if(this.remainingSecond<10){
        this.remainingSecond=""+"0"+this.remainingSecond
      }
    },
    finishCounter(text){
      clearInterval(this.interval); 

      this.isStart=false;
      this.startText=text
    },
    startFunc(){
      if(!this.isStart){
        this.interval = setInterval(()=>{      
          this.second--;
          this.secondToMinute(this.second)
          if(this.second==0){
            let date=new Date();
            let item={
              time:"",
              text:""
            };

            item.time=date.getHours()+":"+date.getMinutes();

            if(this.isWorking){
              item.text="Çalışma Bitti:";
              this.isWorking=false
              this.secondToMinute(this.minuteToSecond(this.breakTime))
            }else{
              item.text="Dinlenme Bitti"
              this.isWorking=true
              this.secondToMinute(this.minuteToSecond(this.workingTime))
            }
            this.history.unshift(item)

            this.finishCounter("Start")
          }

        },1000)
        this.isStart=true;
        this.startText="Stop"
      }else{
        
          this.finishCounter("Stop")
      }
      
    }

  },
  created(){
    this.minuteToSecond(this.workingTime)
  },
  computed:{
    restingClass(){
      return !this.isWorking;
    }
  }
}
</script>

<style>
/*

#709EA0
#6DBDC7
#AADBDE
#F05D68
#E05347
*/
h1 {
  font-size: 25px;
  font-weight: 200;
}
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  flex-direction: column;
}
.timer {
  font-size: 75px;
  background: rgba(255,255,255,.8);
  padding: 15px 50px;
}
.startButton{
  background: #AADBDE;
  position: relative;
  border: 1px solid #6DBDC7;
  border-bottom: 3px solid #6DBDC7;
  padding: 10px 30px;
  font-size: 15px;
  font-weight: 600;
  border-radius: 5px;
  transition: .3s;
  cursor: pointer;
}
.startButton:hover{
  background: #fff;
  border-bottom: 3px solid #6DBDC7;
}
.startButton:after{
  content: "";
  background: #6DBDC7;
  position: absolute;
  bottom:0;
  left:0;
  height:0px;
  width:100%;
  transition:.3s;
}
.startButton:hover:after{
  height:4px;

}
.container{
  background-image: url("static/working.jpg");
  background-size: cover;
  transition:.3s;
}
.container.resting{
  background-image: url("static/resting.jpg");
  background-size: cover;
}
.information-button{
  position: fixed;
  top: 15px;
  left: 15px;
  background: #709EA0;
  width: 40px;
  height: 40px;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 10px;
  cursor:pointer;

}
.popup {
  position: fixed;
  width: 100%;
  height: 0;
  overflow: hidden;
  background: rgba(0,0,0,.5);
  display:flex;
  justify-content: center;
  align-items: center;
  transition:.5s;
  top:0;
}
.popup .overlay {
  width: 80%;
  max-width: 500px;
  background: #fff;
  padding: 30px;
  border-radius: 10px;
  box-shadow: 0px 0px 1px #ccc;
}
.popup .buymebutton{
  text-decoration: none;
  color: #fff;
  background: #F05D68;
  padding: 5px 15px;
  display: inline-block;
  margin: 10px 0;
  border-radius: 5px;
}
.popup ul{
  list-style: none;
  padding: 0;
}

.popup ul li{
  padding: 0;
}
.popup.active{
  height:100%;
}
.working-preview {
    background: #E05347;
    height: 100px;
    overflow-y: auto;
}
.working-preview ul {
    padding: 0;
    list-style: none;
}
.working-preview ul li {
    color: #fff;
    padding: 15px;
    border-bottom: 1px solid #AADBDE;
}
</style>
