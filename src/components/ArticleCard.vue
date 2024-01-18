<template>
    <div class="article" @click="handleClick">
      <div class="top">
        <h3 class="title">{{ title }}</h3>
        <div :style="{ backgroundImage: `url(${media})` }" class="img"></div>
        <p class="desc">{{ summary }}</p>
      </div>
      <div class="bottom">
        <p class="src">Source: {{ rights }}</p>
        <svg-icon type="mdi" :path="path"></svg-icon>
      </div>
    </div>
  </template>
  
<script>
  import SvgIcon from '@jamescoyle/vue-icon';
  import { mdiOpenInNew } from '@mdi/js';

  export default {
    name: 'ArticleCard',
    components: {
      SvgIcon,   
    },
    data() {
      return {
        path: mdiOpenInNew,
      }
    },
    props: {
      title: {
        type: String,
        default: 'No title',
      },
      media: {
        type: String,
        default: 'https://media.istockphoto.com/id/1309699912/vector/vector-illustration-daily-news-paper-template-with-text-and-picture-placeholder.jpg?s=1024x1024&w=is&k=20&c=H-sG8enS-3H7cFcJaLY883g4UbUBN0zNxrQkl2OzjYM=',
      },
      summary: {
        type: String,
        default: 'No description',
      },
      rights: {
        type: String,
        default: 'Uknown',
      },
      link: {
        type: String,
        default: '',
      },
    },
    methods: {
      handleClick() {
        console.log('Link:', this.link);
        this.$emit('click', this.link);
      },
    },
  };
  </script>

<style lang="scss">
@import "../styles/_variables.scss";
    .article {
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        margin: 8px;
        padding: 2px 0;
        background-color: $gray;
        color: $white;
        border-radius: 12px;
        box-shadow: rgba(0, 0, 0, 0.16) 0px 1px 4px;
        transition: 0.2s ease-in-out;
        &:hover {
          cursor: pointer;
          transform:scale(101%);
          box-shadow: rgba(0, 0, 0, 0.30) 0px 1px 4px;
        }
        .title {
            text-align: start;
            font-size: 18px;
            padding: 0 16px;
        }
        .img {
            width: 100%;
            height: 200px;
            background-size: cover;
            background-position: center;
        }
        .desc {
            text-align: start;
            font-size: 14px;
            padding: 0 16px;
        }
        .bottom {
          padding: 0 16px;
          display: flex;
          justify-content: space-between;
          align-items: center;
          .src {
              text-align: left;
              font-size: 12px;
          }
          svg {
            height: 16px;
          }
        }
    }
</style>
