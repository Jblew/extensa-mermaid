<template>
  <div>
    <h2>Basic mermaid</h2>
    <extensa-mermaid-view
      config-url="/mocks/feedback.json"
      component-base-url=""
      backend-path="/"
    />
    <hr />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import ExtensaMermaidView from "./components/View.vue";
@Component({
  components: {
    ExtensaMermaidView,
  },
})
export default class App extends Vue {}
</script>

<style></style>
