<template>
  <div class="content">
    <div class="film__list" v-for="(item,index) in contentNewsList">
      <navigator :url="'../film-details/main?id=' + item.id">
        <div class="film__card">
          <div class="film__img">
            <image v-if="item.images.medium" :src="item.images.medium"></image>
          </div>
          <div class="film__info">
            <p class="film__title">{{ item.title }}</p>
            <p class="film__average">评分：{{ item.rating.average }}</p>
            <p class="film__detail">
              <span class="film__genre" v-for="(genre,tag) in item.genres">{{genre}}</span> /
              <span v-for="(dir,director) in item.directors">
                {{ dir.name }}
              </span> /
              <span class="film__genre" v-for="(cas,cast) in item.casts">{{ cas.name }}</span>
            </p>
            <p class="film__collect">
              {{ item.collect_count }}评价
            </p>
          </div>
        </div>
      </navigator>
    </div>
  </div>
</template>

<script>

export default {
  data() {
    return {
      contentNewsList: [],
    };
  },

  components: {
  },

  methods: {
    getNewsData() {
      this.$fly.request({
        method: 'get',
        url: '/in_theaters',
        body: {
          apikey: '0b2bdeda43b5688921839c8ecb20399b',
          city: '深圳',
          start: '0',
          count: '100',
          client: '',
          udid: '',
        },
      }).then((res) => {
        this.contentNewsList = res.subjects;
        // eslint-disable-next-line
        console.log(res);
      });
    },
    getDetails() {
      wx.navigateTo({
        url: 'film-details/main',
      });
      // eslint-disable-next-line
      console.log('go to details');
    },
  },

  async onPullDownRefresh() { // 下拉刷新
    await this.getNewsData();
    wx.showNavigationBarLoading();
    wx.hideNavigationBarLoading(); // 完成停止加载
    wx.stopPullDownRefresh();
  },

  created() {
    this.getNewsData();
  },

  onReachBottom() { // 上拉加载
    this.getNewsData();
  },
};
</script>

<style scoped>
.film__list {
  padding: 10px;
}
.film__card {
  display: flex;
  padding: 10px;
  border-bottom: 1px solid #d9d9d9;
}
.film__img image{
  width: 90px;
  height: 130px;
  border-radius: 5px;
}
.film__info {
  padding: 0px 20px;
}
.film__title {
  font-size: 16px;
  font-weight: 600;
}
.film__average {
  font-size: 12px;
  color: coral;
}
.film__detail {
  font-size: 14px;
  margin-top: 10px;
  color: #a9a9a9;
}
.film__genre {
  margin-right: 4px;
}
.film__collect {
  font-size: 13px;
  color: #a9a9a9;
  background: #f9f9f9;
  width: 100%;
  height: 30px;
  line-height: 30px;
  border-radius: 5px;
}
</style>
