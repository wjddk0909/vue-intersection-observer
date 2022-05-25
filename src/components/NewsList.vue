<template>
  <div>
    <ul class="news-list">
      <li v-for="item in items" :key="item.index" class="post">
        <div class="points">
          {{ item.points || 0 }}
        </div>
        <div>
          <!-- 타이틀 영역 -->
          <p class="news-title">
            <template v-if="item.domain">
                {{ item.title }}
            </template>
          </p>
          <small class="link-text">
            {{ item.time_ago }} by
            {{ item.domain }}
          </small>
        </div>
      </li>
    </ul>
    <!-- 하위컴포넌트에서 infiniteScroll 이벤트 emit 하면 intersected 실행-->
    <Observer @infiniteScroll="intersected"/>
  </div>
</template>

<script>
import axios from 'axios';
import Observer from '@/components/Observer'

export default {
  name: "NewsList",
  components: {
    Observer,
  },
  data() {
    return {
      page: 1,
      items: [],
    }
  },
  methods: {
    async intersected() {
      const res = await axios.get(`https://api.hnpwa.com/v0/news/${this.page}.json`) // get 요청으로 데이터를 받아와서 변수 res에 넣고
      console.log(this.page, res.data);
      this.page++;
      const addItems = res.data; // 변수 addItems에 intersection observer에서 intersected 함수를 실행 시켜서 호출한 다음 page의 데이터를 담음
      this.items = [...this.items, ...addItems]
      // console.log(this.items)
      // console.log(items)
    }
  }
}
</script>

<style lang="scss" scoped>
.news-list {
  margin: 0;
  padding: 0;
  text-align: left;
  .post {
    list-style: none;
    display: flex;
    align-items: center;
    border-bottom: 1px solid #eee;
    .points {
      width: 80px;
      height: 60px;
      display: flex;
      align-items: center; // (flex)세로 중앙 정렬
      justify-content: center; // (flex)가로 중앙 정렬
      color: #42b883
    }
    .news-title {
      margin: 0;
    }
    .link-text {
      color: #828282
    }
  }
}
</style>