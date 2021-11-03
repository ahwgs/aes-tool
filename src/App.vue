<template>
  <div class="container">
    <div class="row-header">在线AES Tools</div>
    <div class="row-container">
      <div class="row">
        <header>加密</header>
        <div class="itme">
          <n-input
            clearable
            v-model:value="value1"
            type="textarea"
            placeholder="加密内容"
          />
        </div>
        <!-- <div class="itme">
          图片内容：
          <n-upload
            action="/"
            accept=".jpg, .jpeg, .png"
            max="1"
            v-model:file-list="fileList"
            list-type="image-card"
          />
        </div> -->
        <div class="itme">
          <n-input
            clearable
            v-model:value="value2"
            type="text"
            placeholder="加密秘钥"
          />
        </div>
        <div class="itme">
          <n-button @click="onCreate" type="primary">生成</n-button>
          <n-button style="margin-left: 5px" @click="onReset1" type="default"
            >清空</n-button
          >
        </div>
        <div v-if="value5" @click="onCopy(value5)" class="itme copy-text">
          生成内容(点击复制)： {{ value5 }}
        </div>
      </div>
      <div class="row">
        <header>解密</header>
        <div class="itme">
          <n-input
            clearable
            v-model:value="value3"
            type="textarea"
            placeholder="解密内容"
          />
        </div>
        <div class="itme">
          <n-input
            clearable
            v-model:value="value4"
            type="text"
            placeholder="解密秘钥"
          />
        </div>
        <div class="itme">
          <n-button @click="onDec" type="primary">解密</n-button>
          <n-button style="margin-left: 5px" @click="onReset2" type="default"
            >清空</n-button
          >
        </div>
        <div v-if="value6" @click="onCopy(value6)" class="itme copy-text">
          解密结果(点击复制)： {{ value6 }}
        </div>
      </div>
    </div>
    <footer>
      娱乐一下，欢迎Star
      <a
        style="margin-left: 5px"
        target="_blank"
        href="https://github.com/ahwgs/aes-tool"
        >https://github.com/ahwgs/aes-tool</a
      >
    </footer>
  </div>
</template>
<script>
import { defineComponent, ref } from "vue";
import { NInput, NButton, NUpload } from "naive-ui";
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
  components: { NInput, NButton, NUpload },
  setup() {
    const value1 = ref(null),
      value2 = ref(null),
      value3 = ref(null),
      value4 = ref(null),
      value5 = ref(null),
      value6 = ref(null);
    // const fileListRef = ref([]);

    const onCopy = (str) => {
      navigator.clipboard.writeText(str).then(() => {
        alert(`${str} 已复制到粘贴板`);
      });
    };

    const onCreate = () => {
      if (!value1.value || !value2.value) {
        alert("请输入加密内容或秘钥");
        return;
      }
      const str = encrypt(value1.value, value2.value);
      value5.value = str;
      onCopy(str);
    };

    const onDec = () => {
      if (!value3.value || !value4.value) {
        alert("请输入解密内容或秘钥");
        return;
      }
      try {
        const str = decrypt(value3.value, value4.value);
        value6.value = str;
      } catch (e) {
        alert("解密失败");
      }
    };

    const onReset1 = () => {
      value1.value = null;
      value2.value = null;
      value5.value = null;
    };

    const onReset2 = () => {
      value3.value = null;
      value4.value = null;
      value6.value = null;
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
      onCopy,
      onReset2,
      onReset1,
      // fileList: fileListRef,
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
footer {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 60px;
  background: cyan;
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  font-size: 16px;
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
.copy-text {
  cursor: pointer;
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
