<template>
  <form novalidate>
    <div class="form-item">
      <label for="email">メールアドレス</label>
      <input
        id="email"
        v-model="email"
        type="text"
        autocomplete="off"
        placeholder="例: hachidori@domain.com"
        @focus="resetError"
      />
      <ul class="validation-errors">
        <li v-if="!validation.email.format">
          メールアドレスの形式が不正です。
        </li>
        <li v-if="!validation.email.required">
          メールアドレスが入力されていません。
        </li>
      </ul>
    </div>
    <div class="form-item">
      <label for="passowrd">パスワード</label>
      <input
        id="password"
        v-model="password"
        type="password"
        autocomplete="off"
        placeholder="例: xxxxxxxx"
        @focus="resetError"
      />
      <ul class="validation-errors">
        <li v-if="!validation.password.required">
          パスワードが入力されていません。
        </li>
      </ul>
    </div>
    <div class="form-actions">
      <HdButton :disabled="disableLoginAction" @click="handleClick">
        ログイン
      </HdButton>
      <p v-if="progress" class="login-progress">
        ログイン中...
      </p>
      <p v-if="error" class="login-error">
        {{ error }}
      </p>
    </div>
  </form>
</template>

<script lang="ts">
import { Component, Emit, Prop, Vue } from "vue-property-decorator";

// KbnButtonをインポート
import HdButton from "@/components/atoms/HdButton.vue";
// // メールアドレスのフォーマットをチェックする正規表現
const REGEX_EMAIL = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;

// // trim空白を自動的に取り除く。!!は二重否定。valが定義されていれば空白を取り除く処理
const required = (val: string) => !!val.trim();

interface validation {
  //emailとpasswordをstring型にしている。その返り値をオブジェクト型にしている。オブジェクトの中のrequiredとformatをstring型に、その返り値をboolean型に。
  [key: string]: { [key: string]: boolean };
  email: { required: boolean; format: boolean };
  password: { required: boolean };
}

@Component({
  components: {
    HdButton
  }
})
export default class HdLoginForm extends Vue {
  // @Prop({ required: true })
  // public onlogin?: Function;
  email: string = "";
  password: string = "";
  progress: boolean = false;
  error: string = "";

  get validation() {
    //1
    console.log("validation");
    return {
      email: {
        required: required(this.email),
        // testは正規表現と指定した文字列が一致しているか調べる
        format: REGEX_EMAIL.test(this.email)
      },
      password: {
        required: required(this.password)
      }
    };
  }

  get valid() {
    //3
    console.log("valid");
    // this.validationでvalidation()の結果を返している。呼び出されているわけではない。
    const validation: validation = this.validation; // 先に定義したvalidationを用いて可否を返す
    console.log("valid2");
    console.log(validation, "validation");
    console.log(Object.keys(validation), "fields");
    const fields = Object.keys(validation);
    console.log("valid3");
    let valid: boolean = true;
    for (let i = 0; i < fields.length; i++) {
      const field = fields[i];
      console.log(field, "field");
      //Object.keys(validation[field])がkeyに代入される
      //fieldにはemailかpasswordが入る
      //validation[field]でfieldがemailだったらrequired,formatが呼び出される
      //emailの場合は、required,formatが順々にkeyに代入される
      //validation[field(email)][key(required)]が実行される
      //すべての処理が通ればtrueが返される
      valid = Object.keys(validation[field]).every(
        key => validation[field][key]
      );
      console.log(valid, "valid for");
      if (!valid) {
        console.log("valid break");
        break;
      }
    }
    console.log(valid, "valid last");
    //ここでdisabledのtrueとfalseを切り替えている
    return valid;
  }

  get disableLoginAction() {
    //2
    console.log(this, "this");

    console.log(!this.valid, "this.valid");
    console.log("disabledLoginAction");

    // この中の変数が変化するとまた呼び出される
    // validを使ってログイン処理の可否、progressは後述
    return !this.valid || this.progress;
  }

  public resetError() {
    console.log("resetError");
    this.error = "";
  }

  public handleClick(ev: {}) {
    if (this.disableLoginAction) {
      console.log("handleClickError");
      return;
    } // 不備があればログイン処理が実行されないようガード

    this.progress = true; // ログイン処理実行中をあらわす
    this.error = "";
    console.log("handleClick");
    // nextTickはDOMの更新後に処理を実行する
    this.$nextTick(() => {
      console.log("nextTick");
      // this.onlogin({ email: this.email, password: this.password })
      //   .catch(err => {
      //     this.error = err.message;
      //   })
      //   .then(() => {
      //     this.progress = false;
      //   });
    });
  }
}
</script>

<style scoped>
form {
  display: block;
  margin: 0 auto;
  text-align: left;
}
label {
  display: block;
}
input {
  width: 100%;
  padding: 0.5em;
  font: inherit;
}
ul {
  list-style-type: none;
  padding: 0;
  margin: 0.25em 0;
}
ul li {
  font-size: 0.5em;
}
.validation-errors {
  height: 32px;
}
.form-actions p {
  font-size: 0.5em;
}
</style>
