<template>
  <div>
    <div v-if="error" class="error">
      Cannot fetch mermaid config: {{ error }}
    </div>
    <div v-else-if="loading" class="loading">
      Loading, please wait...
    </div>
    <div v-else-if="config">
      <mermaid-renderer :config="config" @refresh="refresh()" />
    </div>
  </div>
</template>

<script lang="ts">
import { Mermaid } from "@/types";
import { Component, Vue, Prop } from "vue-property-decorator";
import MermaidRenderer from "@/renderer/MermaidRenderer.vue";

@Component({
  components: { MermaidRenderer },
})
export default class ExtensaMermaidView extends Vue {
  @Prop({ required: true })
  configUrl!: string;

  @Prop({ required: true, type: String })
  componentBaseUrl!: string;

  @Prop({ type: String, default: () => "/backend/" })
  backendPath!: string;

  loading = true;
  error = "";
  config?: Mermaid;

  resolveConfigURL(): string {
    const configUrlSanitized = this.configUrl.substring(
      this.configUrl.startsWith("/") ? 1 : 0
    );
    return `${this.componentBaseUrl}${this.backendPath}${configUrlSanitized}`;
  }

  mounted(): void {
    this.loadConfig();
  }

  refresh(): void {
    this.loadConfig();
  }

  private async loadConfig() {
    this.loading = true;
    this.error = "";
    const resolvedConfigURL = this.resolveConfigURL();
    try {
      this.config = await fetch(resolvedConfigURL).then((resp) => resp.json());
    } catch (err) {
      console.error(
        `extensa-mermaid cannot load config from ${resolvedConfigURL}`,
        err
      );
      this.error = `${err.message}`;
    }
    this.loading = false;
  }
}
</script>
<style scoped>
.error {
  color: red;
}
</style>
