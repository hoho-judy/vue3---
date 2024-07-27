<template>
  <!-- 조건식 v-if 에 따라 상세정보 모달창이 보이고 안보이고 -->
  <div class="black-bg" v-if="modalOpen == true">
    <div class="white-bg">
       <h4>{{ roomInfos[clickedId].title }}</h4>
       <img :src="roomInfos[clickedId].image" :alt="roomInfos[clickedId].title" class="view-img">
       <p>{{ roomInfos[clickedId].content }}</p>
       <p>{{ roomInfos[clickedId].price }} 만원</p>
       <button @click="$emit('closeModal')">닫기</button>
    </div>
  </div>
</template>

<script>
export default {
   name: 'ModalDetail',
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