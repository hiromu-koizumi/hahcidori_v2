<template>
  <button @click="onClick">Reset</button>
</template>

<script lang="ts">
import { Component, Emit, Prop, Vue } from "vue-property-decorator";

@Component
export default class ResetButton extends Vue {
  private initialValue!: string;

  /** モデルバインドのために記述必須 */
  @Prop()
  public value!: string;

  /** ライフサイクルフック */
  public created() {
    console.log(this.value);
    this.initialValue = this.value;
  }

  /** モデルバインドのために記述必須 */
  //@emit() inputで親コンポーネントのv-modelに値を送る
  @Emit()
  public input(value: string) {
    console.log(value);
  }

  public onClick() {
    this.input(this.initialValue);
  }
}
</script>
