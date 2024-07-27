<template>
  <!-- 태그 안 속성에 바인딩하려면 속성명 앞에 콜론을 붙임 -->
  <!-- v-on:은 @로 대체 가능 -->
  <!-- v-on:click, v-on:mouseover, v-on:drag... -->
  <!-- 부모가 가진 데이터를 수정하거나 조작하고 싶을 때 : 메세지를 보낸다. $emit()으로 이벤트 발송 -->
  <div>
    <img :src="roomInfo.image" :alt="roomInfo.title" class="room-img">
    <!-- <h4 :style="textColor" @click="$emit('openModal', roomInfo.id)">{{ roomInfo.title }}</h4> -->
    <h4 :style="textColor" @click="send">{{ roomInfo.title }}</h4>
    <p>{{ roomInfo.price }} 만원</p>
    <button v-on:click="addReports">허위매물신고</button>
    <span> 신고수 : {{ roomInfo.reportCnt }}</span>
  </div>
</template>

<script>
export default {
   name: 'ProductsList',
   props : {
      roomInfo: Object, // 넘어온 props 이름:자료형 형식으로 작성
      modalOpen: Boolean,
      clickedId: Number,
      textColor: String,
   },
   methods : {
     send() {
         this.$emit('openModal', this.roomInfo.id);
     },
     addReports() {
        this.$emit('updateReports');
     }
   }
}
</script>
  
<style>

</style>
