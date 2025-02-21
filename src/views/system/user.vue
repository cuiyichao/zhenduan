<template>
  <div class="diagnostic-page">
    <!-- 环境选择、uid 输入框、生成诊断 URL 按钮 容器 -->
    <div class="input-container">
      <!-- 环境选择 -->
      <el-form ref="form" label-width="100px" :inline="true" class="demo-form-inline">
        <el-form-item class="form-item-align">
          <label class="env-label">环境：</label>
          <el-radio-group v-model="diagnoseEnv">
            <el-radio-button label="0">测试</el-radio-button>
            <el-radio-button label="1">线上</el-radio-button>
          </el-radio-group>
        </el-form-item>
      </el-form>

      <!-- uid 输入框 -->
      <el-input
          v-model="uid"
          type="text"
          placeholder="请输入 uid"
          class="uid-input"
          @input="handleInput"
      />

      <!-- 生成诊断 URL 按钮 -->
      <el-button @click="generateDiagnosticUrl" type="primary" class="generate-btn">
        生成诊断 URL
      </el-button>
    </div>

    <!-- 显示生成的 URL -->
    <div v-if="diagnosticUrl" class="url-display">
      诊断 URL: <a :href="diagnosticUrl" target="_blank">{{ diagnosticUrl }}</a>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

// 选中的环境
const diagnoseEnv = ref('0');

// uid 输入值
const uid = ref('');

// 生成的诊断 URL
const diagnosticUrl = ref(null);

// 处理输入框输入事件，只允许输入数字
const handleInput = (value) => {
  // 使用正则表达式过滤非数字字符
  uid.value = value.replace(/\D/g, '');
};

// 将 uid 转换为 utoken 的函数，这里简单示例，实际可能需要更复杂的逻辑
const convertUidToUtoken = (uid) => {
  // 这里只是简单示例，实际需要根据具体的转换逻辑实现
  return `eyJpdiI6IkRYaWNoWGg4OEgwZTVuZ0FUYXB0c1E9PSIsInZhbHVlIjoicGZ4bWRxaEdYUmlUU0t2YWFqREtRaGdmWEd1SzNpeWRXaHhPL1lpNWVhOHBWcTVBVnBVZHdUTnREOFdiRmZkU3VSYW9TVitQN2tRWENzd3B6TnhpeXc9PSJ9`;
};

// 生成诊断 URL 的函数
const generateDiagnosticUrl = () => {
  if (!uid.value) {
    alert('请输入 uid');
    return;
  }

  let baseUrl;
  if (diagnoseEnv.value === '0') {
    // 测试环境的 baseUrl
    baseUrl = 'https://evaluation-beta.speiyou.com/h5/answer-xpad';
  } else {
    // 线上环境的 baseUrl
    baseUrl = 'https://evaluation.speiyou.com/h5/answer-xpad';
  }

  const utoken = convertUidToUtoken(uid.value);

  diagnosticUrl.value = `${baseUrl}?rootFrom=polymerize&source=0&shareWorkCode=&busChannel=classroom&examId=30764271a67912ad1dc9383ac7b9789e&duration=600000&utoken=${utoken}`;
};
</script>

<style scoped>
.diagnostic-page {
  padding: 20px;
}

/* 使用 Flexbox 布局的容器 */
.input-container {
  display: flex;
  align-items: center; /* 垂直居中 */
  gap: 10px; /* 设置统一间距 */
  margin-bottom: 30px;
  margin-left: 20px; /* 将三者水平向左移动 */
}

.demo-form-inline {
  margin-bottom: 0; /* 移除原有间距 */
}

.form-item-align {
  margin-bottom: 0; /* 移除 form-item 底部间距 */
  display: flex;
  align-items: center;
}

.env-label {
  line-height: 36px; /* 使“环境：”文字垂直居中 */
}

.uid-input {
  width: 200px;
  margin-bottom: 0; /* 移除原有间距 */
  height: 36px; /* 设置输入框高度 */
  line-height: 36px; /* 设置行高，使文字垂直居中 */
}

.generate-btn {
  margin-bottom: 0; /* 移除原有间距 */
  height: 36px; /* 设置按钮高度 */
  line-height: 36px; /* 设置行高，使文字垂直居中 */
}

.el-radio-group {
  display: flex;
  align-items: center;
  height: 36px; /* 设置单选按钮组的高度，使其与输入框和按钮高度一致 */
}

.el-form-item {
  margin-right: 0; /* 移除 el-form-item 的右边距 */
}

.url-display {
  font-size: 16px;
}
</style>