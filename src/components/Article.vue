<template>
  <div class="wrapper">
    <div class="article" v-on:click="goToURL">
      <div class="section">{{ article.section }}</div>
      <div class="views">{{ article.views }} views</div>
      <div class="row">
        <div class="col-md-12 text-center">
          <img
            class="image"
            v-bind:src="imageData.url"
            v-bind:height="imageData.height"
            v-bind:width="imageData.width"
            v-bind:alt="imageData.caption"
          />
          <div class="copyright">{{ imageData.copyright }}</div>
        </div>
        <div class="col-md-12">
          <div class="title">{{ article.title }}</div>
          <div class="abstract">{{ article.abstract }}</div>
          <div class="footer">
            <div class="byline">{{ article.byline }}</div>
            <div class="published">{{ formattedPublishedDate }}</div>
            <div class="clearfix"></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import moment from 'moment'
export default {
  name: 'Article',
  props: {
    article: Object
  },
  computed: {
    formattedPublishedDate() {
      return moment(this.article.published_date).format('MMM Do YYYY')
    },
    imageData() {
      let imageData = {
        height: 75,
        width: 75,
        url: 'http://placehold.it/75x75',
        caption: 'placeholder',
        copyright: ''
      }
      this.article.media.forEach(media => {
        if (media.type === 'image') {
          media["media-metadata"].forEach(metadata => {
            const formatToLower = metadata.format.toLowerCase()
            if (formatToLower.indexOf('thumbnail') > -1) {
              imageData = metadata
            } else if (formatToLower.indexOf('mediumthreebytwo') > -1) {
              imageData = metadata
            }
          })
          imageData.caption = media.caption || 'placeholder'
          imageData.copyright = media.copyright || ''
        }
      });
      return imageData
    }
  },
  methods: {
    goToURL: function() {
      window.open(this.article.url, '_blank')
    }
  }
}
</script>

<style>
.wrapper {
  padding: 10px;
}
.article {
  padding: 10px 15px;
  box-shadow: 0 3px 6px rgba(0,0,0,0.16), 0 3px 6px rgba(0,0,0,0.23);
  cursor: pointer;
  height: 100%;
}
.article:hover {
  box-shadow: 0 14px 28px rgba(0,0,0,0.25), 0 10px 10px rgba(0,0,0,0.22);
}
.section,
.views {
  position: absolute;
  top: 15px;
  font-weight: bold;
  background-color: #000;
  color: #FFF;
  z-index: 1000;
}
.section {
  left: 3px;
  font-size: 16px;
  font-weight: bold;
  width: 110px;
  padding: 3px 5px 3px 10px;
}
.views {
  right: 3px;
  font-size: 12px;
  text-align: center;
  background-color: #000;
  width: 70px;
  padding: 3px 5px 3px 10px;
}
.image {
  max-width: 100%;
  height: auto;
}
.title {
  font-family: 'Lato', sans-serif;
  font-size: 16px;
  font-weight: bold;
  margin-bottom: 10px;
}
.abstract {
  font-family: 'Roboto', sans-serif;
  font-size: 14px;
}
.copyright {
  margin-bottom: 10px;
  text-align: right;
  font-size: 11px;
  color: #999;
  text-transform: uppercase;
}
.byline {
  float: left;
  max-width: 200px;
}
.published {
  float: right;
}
.clearfix {
  clear: both;
}
.footer {
  margin-top: 10px;
  font-size: 11px;
  color: #999;
  text-transform: uppercase;
}
</style>