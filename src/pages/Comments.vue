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

    accessToken (EAAlZAZB9CA7TIBABFZAd1bXbbfBZAEM0ZCN2ylnyZA42fXG5nZCiO00AEZCoXGDYsTa0idkS2fQ3RjoDGnP6ZARH2aEh0hAe7te9eQlKZCbmKYf8lW6sAS4frRK8LaCnirp6sexYpndKG6Q4FXFhyHC7PG3rJN6Us9YLMDpE1CgYkdQOzJf27vcqaZAJzvB1lTrpVzJq3xZBqufJKQZDZD) {
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
