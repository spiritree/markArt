<template>
  <div class="date">
    <p class="title">
      <span class="title-name"> {{ date }} </span>
      <span class="line"></span>
    </p>

    <div class="article">
      <articleView
        :articleList = "list"
        :haveMoreArt="haveMoreArt"
        @loadMore="loadMore"></articleView>
    </div>

  </div>
</template>

<script>
import articleView from '~/components/common/article.vue';

export default {
  name: 'date',

  transition: 'fade',

  scrollToTop: true,

  fetch({ store, params }) {
    return store.dispatch('getArtList', params);
  },

  head() {
    return { title: `${this.date} | date` };
  },

  data() {
    return {};
  },

  components: {
    articleView
  },

  computed: {
    mobileLayout() {
      return this.$store.state.options.mobileLayout;
    },

    date() {
      return this.$route.params.date;
    },

    list() {
      return this.$store.state.article.art.list;
    },

    haveMoreArt() {
      return (
        this.$store.state.article.art.pagination.current_page !==
        this.$store.state.article.art.pagination.total_page
      );
    }
  },

  methods: {
    loadMore() {
      this.$store.dispatch('getArtList', {
        current_page: this.$store.state.article.art.pagination.current_page + 1,
        date: this.date
      });
    }
  },

  updated() {
    const thumbNodes = document.getElementsByClassName('onelist-item-thumb');
      Array.from(thumbNodes).map(item => {
        let randomNumber = parseInt(Math.random() * 10);
        let prevNumber = randomNumber;
        if (prevNumber === randomNumber) {
          randomNumber = parseInt(Math.random() * 10);
        }
        item.style = `background-image:url(/images/${randomNumber}.jpg)`;
      });
  },

  mounted() {
    this.$nextTick(() => {
      const thumbNodes = document.getElementsByClassName('onelist-item-thumb');
      Array.from(thumbNodes).map(item => {
        let randomNumber = parseInt(Math.random() * 10);
        let prevNumber = randomNumber;
        if (prevNumber === randomNumber) {
          randomNumber = parseInt(Math.random() * 10);
        }
        item.style = `background-image:url(/images/${randomNumber}.jpg)`;
      });
    });
  }
};
</script>

<style scoped lang="scss">
@import '~assets/scss/variable.scss';

.date > .title {
  position: relative;
  display: flex;
  align-items: center;
  padding: 0.5rem 0rem;
  line-height: 1.5rem;
  font-size: 1rem;
  font-weight: normal;

  i {
    margin-right: 0.5rem;
  }

  > .title-name {
    position: relative;
    padding-right: $lg-pad;
    background: $white;
    z-index: 99;
  }

  > .line {
    top: 50%;
  }
}
</style>
