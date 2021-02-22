<template>
  <a
    v-if="isLink"
    :key="action.uri"
    class="action-button button"
    :href="action.uri"
    target="_blank"
    rel="noopener nofollow noreferrer"
  >
    {{ action.text }}
  </a>
  <button
    v-else
    :key="action.payload"
    class="action-button button"
    @click="handleSendMessage(`${action.payload}`)"
  >
    {{ action.text }}
  </button>
  
 
</template>

<script>
import { mapActions } from 'vuex';
import { mixin as clickaway } from 'vue-clickaway';
import configMixin from '../mixins/configMixin';
export default {
  components: {},
  mixins: [clickaway, configMixin],
  props: {
    onSendMessage: {
      type: Function,
      default: () => {},
    },
    action: {
      type: Object,
      default: () => {},
    }
  },
  computed: {
    isLink() {
      return this.action.type === 'link';
    },
    isPayload(){
      return this.action.type === 'postback';
    }
  },
  methods: {
      ...mapActions('conversation', ['sendMessage', 'sendAttachment']),
    handleSendMessage(content) {
      this.sendMessage({
        content,
      });
    },
//(`${action.payload}`)
  },
};
</script>

<style scoped lang="scss">
@import '~widget/assets/scss/variables.scss';
@import '~dashboard/assets/scss/mixins.scss';

.action-button {
  align-items: center;
  border-radius: $space-micro;
  display: flex;
  font-weight: $font-weight-medium;
  justify-content: center;
  margin-top: $space-smaller;
  max-height: 34px;
  padding: 0;
  width: 100%;
}
</style>
