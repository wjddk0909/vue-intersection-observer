<template>
  <!-- observer가 observe할 요소-->
  <div ref="scrollDiv"></div>
</template>

<script>
export default {
  name: "Observer-com",
  data() {
    return {
      observer: null,
      option: {
        root: null, // default = null -> 브라우저 viewport
        threshold: 1, // observe(관찰)하는 tartget이 root옵션에서 지정한 요소와 얼마나 교차했을때 콜백함수를 실행할지 결정(1은 100%)
      }
    }
  },
  methods: {
    handleIntersect: function (target) {
      if (target.isIntersecting) { // target이 root 영역에 교차되고 있으면
        this.$emit('infiniteScroll'); // infiniteScroll 이벤트 부모컴포넌트로 emit
      }
    },
  },
  mounted() {
    // intersection observer : 사용자가 감시하고자 하는 페이지 요소(자신에게 등록된 요소)가 특정 요소(브라우저 viewport)와 교차되는 정도를 관찰하고, 설정해둔 비율 이상의 교차가 일어났을 때 실행되어야 하는 콜백함수 등록 가능
    // new IntersectionObserver (callback, options);
    // callback : 관찰이 시작되는 시점에서 실행되는 함수, 2개의 파라미터를 가짐
    // ㄴ entries: IntersectionObserverEntry 객체들을 배열로 반환 / observer: IntersectionObserver instance
    this.observer = new IntersectionObserver((entry) => {
      this.handleIntersect(entry[0]);
      // console.log(entry[0])
      // console.log(entry[0].isIntersecting) // isIntersecting : target이 root영역에 교차되고 있는지의 정보를 boolean으로 반환
    }, this.option)
    this.observer.observe(this.$refs.scrollDiv) // scrollDiv 관찰시작
  }
}
</script>

<style scoped>
div {
  opacity: 0;
}
</style>