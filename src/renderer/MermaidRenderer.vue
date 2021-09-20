<template>
  <div>
    {{ configJSON }}
  </div>
</template>

<script lang="ts">
import { Mermaid } from "@/types";
import { Component, Vue, Prop } from "vue-property-decorator";

@Component
export default class MermaidRendered extends Vue {
  @Prop({ required: true })
  config!: Mermaid;

  get configJSON(): string {
    return JSON.stringify(this.config);
  }
}
</script>
<style scoped></style>
