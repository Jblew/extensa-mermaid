<template>
  <div>
    <div ref="mmdtarget"></div>
  </div>
</template>

<script lang="ts">
import { Mermaid } from "@/types";
import { Component, Vue, Prop } from "vue-property-decorator";
import mermaid from "mermaid";

@Component
export default class MermaidRendered extends Vue {
  @Prop({ required: true })
  config!: Mermaid;

  mounted(): void {
    this._initializeMermaid();
    this._renderGraph();
  }

  _initializeMermaid(): void {
    mermaid.mermaidAPI.initialize({
      startOnLoad: false,
    });
  }

  _renderGraph(): void {
    const insertSvg = (svgCode: any, _bindFunctions: any) => {
      (this.$refs.mmdtarget! as any).innerHTML = svgCode;
    };
    const id = `mmdtarget-${Math.random()
      .toString()
      .substring(2)}`;

    const graphDefinition = this.config.mermaid.mermaid;
    mermaid.mermaidAPI.render(id, graphDefinition, insertSvg);
  }
}
</script>
<style scoped></style>
