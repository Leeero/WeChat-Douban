/*
 * @Author: leezonglin 
 * @Date: 2018-09-21 16:34:37 
 * @Last Modified by: leezonglin
 * @Last Modified time: 2018-09-21 16:56:34
 */

/* eslint-disable */
<template>
  <div class="content">
    <div class="film__card">
      <div class="film__img">
        <image
          v-if="filmDetails.images"
          :src="filmDetails.images.medium"
        ></image>
      </div>
      <div class="film__info">
        <p class="film__title">{{ filmDetails.title }}</p>
        <p class="film__pubdate">{{ filmDetails.pubdate }}</p>
        <p class="film__detail">
          <span
            class="film__genre"
            v-for="(coun,countrie) in filmDetails.countries"
          >
            {{ coun }} /
          </span>
          <span
            class="film__genre"
            v-for="(genre,tag) in filmDetails.genres"
          >
            {{genre}} /
          </span>
          <span v-for="(pub,pubdate) in filmDetails.pubdates">
            {{ pub }} /
          </span>
          <span
            class="film__genre"
            v-for="(dur,duration) in filmDetails.durations"
          >
            {{ dur }}
          </span>
        </p>
      </div>
    </div>
    <div class="film__tags">
      <van-tag type="success">所属频道</van-tag>
      <van-tag
        mark
        v-for="(tag,ta) in filmDetails.tags"
        class="tag"
      >{{ tag }}</van-tag>
    </div>
    <div class="film__Introduction">
      <p class="film__Introduction__title">剧情简介</p>
      <p class="film__Introduction__text">{{filmDetails.summary}}</p>
    </div>
    <div class="film__director">
      <p class="film__Introduction__title">导演</p>
      <div
        class="film__director__avatar"
        v-for="(image,img) in filmDetails.directors"
      >
        <image
          :src="image.avatars.small"
          alt=""
        ></image>
        <p class="film__director__name">{{ image.name }}</p>
      </div>
    </div>
    <div class="film__star">
      <p class="film__Introduction__title">主演</p>
      <div class="film__star__avatar">
        <div
          v-for="(star,sta) in filmDetails.casts"
          class="star"
        >
          <image
            :src="star.avatars.small"
            alt=""
          ></image>
          <p class="film__star__name">{{ star.name }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      id: '',
      filmDetails: '',
      title: '',
    };
  },

  components: {},

  methods: {
    getNewsData() {
      this.$fly.request({
        method: 'get',
        url: `/subject/${this.id}`,
        body: {
          apikey: '0b2bdeda43b5688921839c8ecb20399b',
          city: '深圳',
          start: '0',
          count: '100',
          client: '',
          udid: '',
        },
      }).then((res) => {
        this.filmDetails = res;
        // eslint-disable-next-line
        this.title = res.title;
      });
    },
  },

  mounted() {
    this.getNewsData();
  },
  onLoad(option) {
    this.id = option.id;
  },
  // updateShareMenu() {
  //   return {
  //     title: '自定义转发标题',
  //     path: 'pages/film-details/main',
  //     success() { },
  //   };
  // },
  onShareAppMessage() {
    return {
      title: this.title,
      path: 'pages/film-details/main',
      success() { },
    };
  },
};
</script>

<style scoped>
.content {
  padding: 10px;
  height: 100%;
  background: #e5eff1;
}

.film__card {
  display: flex;
  padding: 10px;
}

.film__img image {
  width: 90px;
  height: 130px;
  border-radius: 5px;
}

.film__info {
  padding: 0px 20px;
}

.film__title {
  font-size: 17px;
  font-weight: 600;
  color: #000000;
}

.film__pubdate {
  font-size: 14px;
  font-weight: 600;
  color: #000000;
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

.film__tags {
  border-top: 1px solid #d9d9d9;
  display: flex;
  width: 100%;
  height: 40px;
  white-space: nowrap;
  align-items: center;
  overflow-x: scroll; /* 1 */
}
.film__tags p {
  font-size: 14px;
  line-height: 40px;
}
.tag {
  margin-left: 5px;
}
.film__Introduction__title {
  font-size: 14px;
  color: #a9a9a9;
  padding-bottom: 5px;
}
.film__Introduction__text {
  font-size: 12px;
  color: #a9a9a9;
}
.film__director,
.film__star {
  padding-top: 5px;
}
.film__director__avatar {
  display: flex;
  flex-direction: column;
}
.film__director__avatar image {
  width: 70px;
  height: 100px;
  border-radius: 5px;
}
.film__director__name,
.film__star__name {
  font-size: 12px;
  color: #a9a9a9;
  width: 70px;
  text-align: center;
}
.film__star__avatar {
  display: flex;
  white-space: nowrap;
  overflow-x: scroll; /* 1 */
}
.film__star__avatar image {
  width: 70px;
  height: 100px;
  border-radius: 5px;
}
.star {
  margin-right: 20px;
}
</style>
