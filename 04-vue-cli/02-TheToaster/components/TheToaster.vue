<template>
  <div class="toasts">
    <UiToast
      v-for="toast in toasts"
      :id="toast.id"
      :key="toast.id"
      :type="toast.type"
      :message="toast.message"
      @remove="remove"
    />
  </div>
</template>

<script>
import UiToast from './UiToast.vue';
import { defaultDelay } from '../helpers';

export default {
  name: 'TheToaster',

  components: { UiToast },

  data: () => ({
    toasts: {},
  }),

  unmounted() {
    this.toasts.forEach((toast) => clearTimeout(toast.id));
  },

  methods: {
    remove(id) {
      delete this.toasts[id];
    },
    initToast(type, message = '', delay = defaultDelay) {
      const id = setTimeout(() => this.remove(id), delay);
      this.toasts[id] = { type, message, id };
    },
    success(message, delay) {
      this.initToast('success', message, delay);
    },
    error(message, delay) {
      this.initToast('error', message, delay);
    },
  },
};
</script>

<style scoped>
.toasts {
  position: fixed;
  bottom: 8px;
  right: 8px;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  white-space: pre-wrap;
  z-index: 999;
}

@media all and (min-width: 992px) {
  .toasts {
    bottom: 72px;
    right: 112px;
  }
}
</style>
