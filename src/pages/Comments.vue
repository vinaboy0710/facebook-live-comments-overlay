<template>
  <div>
    <Comment
      v-for="(comment, i) in comments"
      :key="comment.id"

      :from="comment.from"
      :message="comment.message"
    />
  </div>
</template>

<script>
import Comment from '@/components/Comment'

export default {
  components: { Comment },

  data: () => ({
    comments: [],

    eventSource: null
  }),

  computed: {
    videoId (3266501330090321) {
      return this.$route.query.video_id
    },

    accessToken (EAAlZAZB9CA7TIBAHhbVo7tz36p8OSPZB5cEPDp6ZCN4NsiAm3x5qdKw6XjdAuFJ5PD3xHFrnIhQqqHorWfig1nxLz91yBr6X8KU8dmz56dwwX3FIkDcklQCG5gyBraDSOGxIZA9QSb3dcBg4vA7jFUiXRZBQpirNpR5HEtLSnOYnBkBhj1iW4D9KdfhvqZACZCf8AxBZBfG1U3gZDZD) {
      return this.$route.query.access_token
    },

    limit () {
      return this.$route.query.limit || 10
    },

    eventSourceUrl () {
      return `https://streaming-graph.facebook.com/${this.videoId}/live_comments?` +
        `fields=from{name,id},message&access_token=${this.accessToken}`
    }
  },

  created () {
    this.eventSource = new EventSource(this.eventSourceUrl)

    this.eventSource.onmessage = e => this.onComment(e)
  },

  methods: {
    onComment (e) {
      this.comments.push(JSON.parse(e.data))

      if (this.comments.length > this.limit) {
        this.comments.shift()
      }
    }
  }
}
</script>
