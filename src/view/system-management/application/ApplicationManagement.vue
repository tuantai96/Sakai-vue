<template>
  <div class="card">
    <h5>{{ this.$t("application.title") }}</h5>
  </div>
</template>
<script>
import {useI18n} from 'vue-i18n';

export default {
  setup() {
    const {t} = useI18n();
    return {t}
  }
}
</script>