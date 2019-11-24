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

    accessToken (EAAFZBbIm1LMMBAOFzADD0jTQHue9mP9P7o5bcuOaJEmO4JVuCJyXXZCDgVYMuA7OJB2fv7svYVC06FA6x5AD7VVxFZBBwOjZAwmMuv3yHmHIZAL85CukuCvgTCuAodIdSdvvMiJJeUseCL4AUsbgqU9A9GAL4mATKQaZBDRkHZB1yKmNpIL4lVwAgva5dJVHg9RTY9pZBGLs7QZDZD) {
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
