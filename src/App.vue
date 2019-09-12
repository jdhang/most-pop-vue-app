<template>
  <div id="app">
    <div class="container">
      <div class="row">
        <div class="col-md-12">
          <Header
            title="Most Popular Articles on New York Times"
            v-bind:period="period"
          />
          <ul class="nav nav-tabs justify-content-center">
            <Tab
              v-for="tab in tabs"
              v-bind:key="tab.id"
              v-bind:tab="tab"
              v-bind:select="setPeriod"
              v-bind:selectedPeriod="period"
            />
          </ul>
          <div class="row">
            <Article
              class="col-md-4"
              v-for="article in articles"
              v-bind:key="article.id"
              v-bind:article="article"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Article from './components/Article.vue'
import Header from './components/Header.vue'
import Tab from './components/Tab.vue'
import { getCookie, setCookie } from './utils/cookieHelper'

const tabs = [
  {
    id: 1,
    text: '1 Day',
    period: 1
  },
  {
    id: 2,
    text: '7 Days',
    period: 7
  },
  {
    id: 3,
    text: '30 Days',
    period: 30
  }
]

const json = (res) => res.json()
const byViews = (a, b) => {
  return b.views - a.views;
}

export default {
  name: 'app',
  components: {
    Header,
    Article,
    Tab
  },
  data: () => (
    {
      period: 7,
      articles: [],
      tabs: tabs
    }
  ),
  mounted() {
    this.getLastSelectedPeriod()
    this.getArticlesForPeriod(this.period)
  },
  methods: {
    getLastSelectedPeriod: function() {
      const lastPeriod = getCookie('last_period')
      if (lastPeriod) {
        this.period = +lastPeriod
      }
    },
    getArticlesForPeriod: function(period) {
      fetch(`https://api.nytimes.com/svc/mostpopular/v2/viewed/${period}.json?api-key=fscln9NHTk6OXewIHOTfNiG5GyngV9lA`)
        .then(json)
        .then(data => {
          this.articles = data.results.filter(el => el.type == 'Article').sort(byViews);
        })
    },
    setPeriod: function (period) {
      this.period = period
      setCookie('last_period', period)
      this.getArticlesForPeriod(this.period)
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
  margin-bottom: 60px;
}
</style>
