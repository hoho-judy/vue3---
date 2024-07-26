<template>
  <!-- 조건식 v-if 에 따라 상세정보 모달창이 보이고 안보이고 -->
  <div class="black-bg" v-if="modalOpen == true">
    <div class="white-bg">
       <h4>{{ roomInfos[clickedId].title }}</h4>
       <img :src="roomInfos[clickedId].image" :alt="roomInfos[clickedId].title" class="view-img">
       <p>{{ roomInfos[clickedId].content }}</p>
       <p>{{ roomInfos[clickedId].price }} 만원</p>
       <button @click="modalOpen = false">닫기</button>
    </div>
  </div>
  
  <div class="menu"> 
    <!-- <a v-for="menu in menus" :key="menu">{{ menu }}</a> -->
    <a v-for="(menu, i) in menus" :key="i">{{ menu }}</a> <!-- 반복문 인자 2개 넣으면 앞 인자는 변수, 뒤 인자는 0부터 인덱스 증가-->
  </div>

  <!-- 태그 안 속성에 바인딩하려면 속성명 앞에 콜론을 붙임 -->
  <!-- v-on:은 @로 대체 가능 -->
  <!-- v-on:click, v-on:mouseover, v-on:drag... -->
  <div v-for="roomInfo in roomInfos" :key="roomInfo.id">
    <img :src="roomInfo.image" :alt="roomInfo.title" class="room-img">
    <h4 :style="textColor" @click="modalOpen = true; clickedId = roomInfo.id">{{ roomInfo.title }}</h4>
    <p>{{ roomInfo.price }} 만원</p>
    <button v-on:click="roomInfo.reportCnt++">허위매물신고</button><span> 신고수 : {{ roomInfo.reportCnt }}</span>
  </div>

</template>

<script>
import roomDatas from './data.js';

roomDatas.map(item => item.reportCnt = 0);

export default {
  name: 'App',
  // 데이터 저장소(Object로 작성)
  data() {
    return {
      modalOpen: false,
      textColor: "color: darkslateblue",
      products : ['역삼동원룸', '천호동원룸', '마곡동원룸'],
      price: [650, 7700, 7700],
      roomInfos : roomDatas,
      clickedId: 0,
      // roomInfos: [
      //   {
      //      name:'역삼동원룸',
      //      price: 650,
      //      reportCnt : 0,
      //      imgSrc: require('./assets/room0.jpg'),
      //   },
      //   {
      //      name:'천호동원룸',
      //      price: 7700,
      //      reportCnt : 0,
      //      imgSrc: require('./assets/room1.jpg'),
      //   },
      //   {
      //      name:'마곡동원룸',
      //      price: 1220,
      //      reportCnt : 0,
      //      imgSrc: require('./assets/room2.jpg'),
      //   },
      // ],
      menus: ['Home', 'Products', 'About'],
      reportCnt : 0,
    }
  },
  // 함수는 methods 안에 모두 선언 및 정의
  methods: {
    increase() {
      this.reportCnt++; // data() 안에 있는 변수를 쓰려면 앞에 this를 붙여야 참조 가능
    }
  },
  components: {}
}
</script>

<style>
body {
  margin: 0;
}

div{
  box-sizing: border-box;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.menu {
  background: darkslateblue;
  padding: 15px;
  border-radius: 5px;
}

.menu > a {
  color: white;
  padding: 10px;
}

.room-img {
  width: 100%;
  margin-top: 40px;
}

.black-bg {
  width: 100%;
  height: 100%;
  background: rgba(0,0,0,0.5);
  position: fixed;
  padding: 20px;
}

.white-bg {
  width: 100%;
  background: white;
  border-radius: 8px;
  padding: 20px;
}
 
.view-img{
  width: 50%;
}

</style>
