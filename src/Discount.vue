<template>
  <div class="discount">
    <h4>지금 결제하면 {{ per }}% 할인</h4>
  </div>
</template>

<script>
export default {
    name: 'DiscountInfo',

    props : {
      per: Object,
    },
}
</script>

<style>

</style>

