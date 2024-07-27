<template>
  
  <!-- <Modal v-bind="roomInfos" /> 자식 컴포넌트로 데이터 전송할 때 props 사용(데이터 바인딩)--> 
  <!-- <Modal v-bind="숫자, 문자열 등 바로 데이터를 넣어도 됨" /> --> 
  <Modal @closeModal="modalOpen = false" :roomInfos="roomInfos" :modalOpen="modalOpen" :clickedId="clickedId" />

  <div class="menu"> 
    <!-- <a v-for="menu in menus" :key="menu">{{ menu }}</a> -->
    <a v-for="(menu, i) in menus" :key="i">{{ menu }}</a> <!-- 반복문 인자 2개 넣으면 앞 인자는 변수, 뒤 인자는 0부터 인덱스 증가-->
  </div>
 
  <!-- 오브젝트를 분해해서 보내거나 한꺼번에 보내기-->
  <!-- <Discount :name="testObj.name" :age="testObj.age"/> -->
  <!-- <Discount v-bind="testObj"/> -->
  <Discount />
  
  <!-- 자식이 보낸 $emit()에 담긴 데이터를 꺼내는 방법 : $event-->
  <Card @openModal="modalOpen = true; clickedId = $event" @updateReports="roomInfo.reportCnt++"
  v-for="roomInfo in roomInfos" :key="roomInfo.id"  :roomInfo="roomInfo" :modalOpen="modalOpen" :clickedId="clickedId" :textColor="textColor"/>

</template>

<script>
import roomDatas from './data.js';
import Discount from './Discount.vue';
import Modal from './Modal.vue';
import Card from './Card.vue';

roomDatas.map(item => item.reportCnt = 0);

export default {
  name: 'App',
  // 데이터 저장소(Object로 작성)
  // 하위 컴포넌트에도 만들수는 있지만, 쓰는 곳이 여러 곳이면 최상위 컴포넌트(최고 부모 컴포넌트)에 데이터를 만드는게 좋음
  data() {
    return {
      testObj: { name:'kim', age:20 },
      modalOpen: false,
      textColor: "color: darkslateblue",
      products : ['역삼동원룸', '천호동원룸', '마곡동원룸'],
      price: [650, 7700, 7700],
      roomInfos : roomDatas,
      clickedId: 0,
      menus: ['Home', 'Products', 'About'],
    }
  },
  // 함수는 methods 안에 모두 선언 및 정의
  methods: {
    increase() {
      this.reportCnt++; // data() 안에 있는 변수를 쓰려면 앞에 this를 붙여야 참조 가능
    }
  },
  // 컴포넌트 등록 공간(외부에 만든 컴포넌트를 기술해야 함)
  components: {
      Discount: Discount,
      Modal: Modal,
      Card: Card,
  }
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
