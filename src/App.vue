<template>
  
  <!-- 애니메이션 속성 주기 : transiton 태그 사용 -->
  <!-- <div class="start" :class="{ end: modalOpen }"> -->
  <transition name="fade">
    <!-- <Modal v-bind="roomInfos" /> 자식 컴포넌트로 데이터 전송할 때 props 사용(데이터 바인딩)--> 
    <!-- <Modal v-bind="숫자, 문자열 등 바로 데이터를 넣어도 됨" /> --> 
    <Modal @closeModal="modalOpen = false" :roomInfos="roomInfos" :modalOpen="modalOpen" :clickedId="clickedId" />
  </transition>
  <!-- </div> -->
  
  <div class="menu"> 
    <!-- <a v-for="menu in menus" :key="menu">{{ menu }}</a> -->
    <a v-for="(menu, i) in menus" :key="i">{{ menu }}</a> <!-- 반복문 인자 2개 넣으면 앞 인자는 변수, 뒤 인자는 0부터 인덱스 증가-->
  </div>
 
  <!-- 오브젝트를 분해해서 보내거나 한꺼번에 보내기-->
  <!-- <Discount :name="testObj.name" :age="testObj.age"/> -->
  <!-- <Discount v-bind="testObj"/> -->
  <Discount v-if="showDiscount == true" :per="per"/>
  
  <div class="order-buttons">
    <button @click="sortPrice">저가순정렬</button>
    <button @click="sortReversePrice">고가순정렬</button>
    <button @click="sortSpell">가나다순정렬</button>
    <button @click="sortBack">되돌리기</button>
  </div>

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
      per: 20,
      showDiscount: true,
      testObj: { name:'kim', age:20 },
      modalOpen: false,
      textColor: "color: darkslateblue",
      products : ['역삼동원룸', '천호동원룸', '마곡동원룸'],
      price: [650, 7700, 7700],
      roomInfos : roomDatas,
      originRoomInfos : [...roomDatas], // 스프레드 오퍼레이터로 별개의 사본을 생성. 스프레드 안쓰면 그냥 같은 배열을 공유하는 셈이 되어버리므로 한쪽에서 값을 바꾸면 다른쪽도 바뀐 값을 참조한다.
      clickedId: 0,
      menus: ['Home', 'Products', 'About'],
    }
  },
  // 함수는 methods 안에 모두 선언 및 정의
  methods: {
    increase() {
      this.reportCnt++; // data() 안에 있는 변수를 쓰려면 앞에 this를 붙여야 참조 가능
    },

    // 가격순정렬 버튼 함수
    sortPrice() {
      // 숫자 정렬 sort
      // array.sort((a, b)=>{
      //   return a - b;
      //})
      // sort는 원본 배열이 변형 되어버리므로 사용 지양
      this.roomInfos.sort((a, b) => {
         return a.price - b.price;
      }) 
    },

    // 가격역순정렬 버튼 함수
    sortReversePrice() {
      this.roomInfos.sort((a, b) => {
         return b.price - a.price;
      })
    },

    // 가나다순 정렬 버튼 함수
    sortSpell() {
      this.roomInfos.sort((a, b) => {
         if(a.title < b.title) {
           return -1;
         } else if(a.title > b.title) {
           return 1;
         } else {
           return 0;
         }
      })
    },

    // 되돌리기 버튼 함수
    // 스프레드 오퍼레이터 : 데이터를 복사할 때 사용 / 괄호를 벗기는 의미. 
    // array를 해체했다가 다시 array로 만들면 서로 값을 공유하지 않는 별개의 사본을 만들수 있다.
    sortBack() {
      this.roomInfos = [...this.originRoomInfos];
    },
  },
 
  // life cylcle hook
  // beforeCreate(), created(), beforeMount(), mounted(), beforeUpdate(), updated(), beforeUnmount(), unmounted() 등
  // 라이프사이클 훅 사용 방법(App.vue가 마운트 되고 나서 - mounted())
  mounted() {
    let interval = setInterval(()=>{
      this.per--;
      if(this.per === 0) {
        this.showDiscount = false;
        clearInterval(interval);
        return;
      }
    }, 1000);
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

/** transition : 모든 속성이 변할 때 걸리는 시간을 임의로 주는 속성 */
.start {
  opacity: 0;
  transition: all 1s;
}

.end {
  opacity: 1;
}

/** transition 애니메이션 효과를 주기 위한 css 3가지*/
/** 등장할 때 {클래스명}-enter-from,... */
.fade-enter-from {
  /** 시작 할 때 스타일 */
  opacity: 0;
}
.fade-enter-active { 
  /** 애니메이션 효과 */
  transition: all 0.5s;
}
.fade-enter-to {
  /** 끝날 때 스타일 */
  opacity: 1;
}

/** 사라질 때 {클래스명}-leave-from,... */
.fade-leave-from {
  /** 시작 할 때 스타일 */
  opacity: 1;
}
.fade-leave-active { 
  /** 애니메이션 효과 */
  transition: all 1s;
}
.fade-leave-to {
  /** 끝날 때 스타일 */
  opacity: 0;
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

.discount {
  background: #eee;
  padding: 10px;
  margin: 10px;
  border-radius: 5px;  
}

.order-buttons {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
}

.order-buttons > button{
  margin: 5px;
}
</style>
