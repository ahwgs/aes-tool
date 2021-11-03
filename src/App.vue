<template>
  <div class="container">
    <div class="row-header">在线AES Tools</div>
    <div class="row-container">
      <div class="row">
        <header>加密</header>
        <div class="itme">
          <n-input
            v-model:value="value1"
            type="textarea"
            placeholder="加密内容"
          />
        </div>
        <div class="itme">
          <n-input v-model:value="value2" type="text" placeholder="加密秘钥" />
        </div>
        <div class="itme">
          <n-button @click="onCreate" type="primary">生成</n-button>
        </div>
        <div v-if="value5" class="itme">生成内容： {{ value5 }}</div>
      </div>
      <div class="row">
        <header>解密</header>
        <div class="itme">
          <n-input
            v-model:value="value3"
            type="textarea"
            placeholder="解密内容"
          />
        </div>
        <div class="itme">
          <n-input v-model:value="value4" type="text" placeholder="解密秘钥" />
        </div>
        <div class="itme">
          <n-button @click="onDec" type="primary">解密</n-button>
        </div>
        <div v-if="value6" class="itme">解密结果： {{ value6 }}</div>
      </div>
    </div>
  </div>
</template>
<script>
import { defineComponent, ref } from "vue";
import { NInput, NButton } from "naive-ui";
import CryptoJS from "crypto-js";

const encrypt = (word, keyStr) => {
  return CryptoJS.AES.encrypt(JSON.stringify(word), keyStr).toString();
};

const decrypt = (word, keyStr) => {
  const bytes = CryptoJS.AES.decrypt(word, keyStr);
  const decryptedData = JSON.parse(bytes.toString(CryptoJS.enc.Utf8));
  return decryptedData;
};

export default defineComponent({
  components: { NInput, NButton },
  setup() {
    const value1 = ref(null),
      value2 = ref(null),
      value3 = ref(null),
      value4 = ref(null),
      value5 = ref(null),
      value6 = ref(null);

    const onCreate = () => {
      if (!value1.value || !value2.value) {
        alert("请输入加密内容或秘钥");
        return;
      }
      const str = encrypt(value1.value, value2.value);
      value5.value = str;
    };

    const onDec = () => {
      if (!value3.value || !value4.value) {
        alert("请输入加密内容或秘钥");
        return;
      }
      const str = decrypt(value3.value, value4.value);
      value6.value = str;
    };

    return {
      value1,
      value2,
      value3,
      value4,
      value5,
      value6,
      onCreate,
      onDec,
    };
  },
});
</script>
<style>
.container {
  display: flex;
  height: 100vh;
  width: 100vw;
  flex-direction: column;
}
.row-header {
  width: 100%;
  height: 50px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.row {
  flex: 1;
  border: 1px solid #f7f7f7;
}
.itme {
  margin: 10px;
}
.row-container {
  display: flex;
}
@media screen and (max-width: 500px) {
  .row-container {
    flex-direction: column;
  }
}
header {
  align-items: center;
  background-color: rgba(0, 128, 0, 0.12);
  box-sizing: border-box;
  display: flex;
  height: 40px;
  justify-content: center;
}
* {
  margin: 0;
  padding: 0;
}
</style>
