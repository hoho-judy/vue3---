<template>
  <!-- 조건식 v-if 에 따라 상세정보 모달창이 보이고 안보이고 -->
  <div class="black-bg" v-if="modalOpen == true">
    <div class="white-bg">
       <h4>{{ roomInfos[clickedId].title }}</h4>
       <img :src="roomInfos[clickedId].image" :alt="roomInfos[clickedId].title" class="view-img">
       <p>{{ roomInfos[clickedId].content }}</p>
       <!-- input, textarea, select 등 모든 인풋 종류에 붙일 수 있는 v-model -->
       <!-- 입력값을 v-model에 바로 저장가능, 초기값을 숫자로 지정하면 들어오는 것도 숫자여야함 -->
       <!-- <input type="text" @input="month = $event.target.value"> -->
       <input v-model.number="month">
       <!-- 입력할 때 숫자만 입력해야 하는데 문자를 입력해버리는... 이상한게 들어오는지 검사할 때 watcher 사용-->
       <p>{{month}}개월 선택함 : {{ roomInfos[clickedId].price * month}} 만원</p>
       <button @click="$emit('closeModal')">닫기</button>
    </div>
  </div>
</template>

<script>

export default {
   name: 'ModalDetail',
   data() {
       return {
         month: 1,
       }
   },
  // 데이터 감시용
  //  watch: {
  //    // 감시할 데이터명으로 함수를 만들면됨
  //    // month 데이터가 변할때마다 감지함
  //    // a: 변경 후 데이터, b: 변경 전 데이터
  //    month(a) {
  //      if(isNaN(a)) {
  //         alert('숫자만 입력 가능합니다.');
  //         this.month = 1;
  //      } else {
  //         if(a > 12) {
  //           alert('13개월 이상은 입력할 수 없습니다.');
  //           this.month = 1;
  //         }
  //      }
  //    },
  //  },

  // 생명주기를 활용한 데이터 감시
   updated() {
     console.log(` updated 호출 및 인풋값: ${this.month}`);
     if(isNaN(this.month)) {
      alert('숫자만 입력할 수 있습니다.');
      this.month = 1;
     } else {
      if(this.month > 12) {
        alert('12개월 이상은 입력할 수 없습니다.');
        this.month = 1;
      }
     }
   },
   // 부모창에서 받은 데이터  props 등록
   // props는 전달받은 후 값을 재할당(수정)하면 안됨! read-only임
   props: {
      roomInfos: Object, // 넘어온 props 이름:자료형 형식으로 작성
      modalOpen: Boolean,
      clickedId: Number,
   },
}

</script>

<style>

</style>