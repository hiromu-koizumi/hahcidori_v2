<template>
  <button
    :class="classes"
    :disabled="disabled"
    type="button"
    @click="handleClick"
  >
    <slot />
  </button>
</template>

<script lang="ts">
import { Component, Emit, Prop, Vue } from "vue-property-decorator";

@Component
export default class HdButton extends Vue {
  @Prop({ default: "button" })
  public type?: string;

  @Prop({ default: false })
  public disabled?: boolean;

  get classes() {
    console.log("classes");
    // 親コンポーネントからtype="text"を受け取ると枠線のないボタンのcssを適応するする処理
    const cls = this.type === "text" ? "-" + this.type : "";
    return [`hd-button${cls}`];
  }

  @Emit("click")
  public handleClick(ev: {}) {
    console.log("hadleClick");
    console.log(ev, "ev");
    return ev;
  }
}
</script>
<style scoped>
.hd-button {
  padding: 0.6em 1.3em;
}
.hd-button-text {
  border: none;
  padding-right: 0;
  padding-left: 0;
}
</style>
