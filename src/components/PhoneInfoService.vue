<script setup lang="ts">

import {Search} from "@element-plus/icons-vue";
import axios from "axios";
import {ref} from "vue";

const URL = 'http://localhost:8080/getPhoneInfo'
const isButtonEnabled = ref(false);
const isPhoneValid = ref(true);
const infoVisible = ref(false);
const isLoading = ref(false);
const phoneNumber = ref('')
const phoneInfo = ref({
  carrier: '',
  city: '',
  prov: ''
})
const mobilePrefixes = new Set([
  '134', '135', '136', '137', '138', '139', '147', '150', '151', '152', '157', '158',
  '159', '172', '178', '182', '183', '184', '187', '188', '195', '197', '198'
]);
const unicomPrefixes = new Set([
  '130', '131', '132', '140', '145', '155', '156', '166', '171', '175', '176', '185', '186', '196'
]);
const telecomPrefixes = new Set([
  '133', '141', '149', '153', '173', '177', '179', '180', '181', '189', '190', '191', '193', '199'
]);


function checkPhoneNumber() {
  isButtonEnabled.value = phoneNumber.value.length === 11;
}

function isValidPhoneNumber(phoneNumber: string) {
  const regExp = /^[0-9]{11}$/;
  return regExp.test(phoneNumber);
}

function determineCarrier(phoneNumber: string) {
  const prefix = phoneNumber.substring(0, 3);
  if (mobilePrefixes.has(prefix)) {
    return '中国移动';
  } else if (unicomPrefixes.has(prefix)) {
    return '中国联通';
  } else if (telecomPrefixes.has(prefix)) {
    return '中国电信';
  } else {
    return '其他';
  }
}

function fetchPhoneInfo() {
  if(!isValidPhoneNumber(phoneNumber.value)){
    isPhoneValid.value = false;
    infoVisible.value = true;
    return;
  }
  isLoading.value = true;
  infoVisible.value = true;
  phoneInfo.value.carrier = determineCarrier(phoneNumber.value);
  axios.get(URL, {params: {phoneNumber: phoneNumber.value}})
      .then(response => {
        phoneInfo.value.city = response.data.city;
        phoneInfo.value.prov = response.data.prov;
      })
      .catch(() => {
        
      })
      .finally(() => {
        isLoading.value = false;
      })
}
</script>

<template>
  <header>
    <h1>
      手机号运营商归属地查询
    </h1>
  </header>

  <div>
    <el-input
        v-model="phoneNumber"
        style="width: 200px"
        placeholder="请输入手机号码"
        clearable
        @input="checkPhoneNumber"
        @focus="infoVisible = false"
    />
    <el-button
        @click="fetchPhoneInfo"
        type="primary"
        :icon="Search"
        :disabled="!isButtonEnabled"
        style="width: 60px"
    >查询
    </el-button>
  </div>

  <div v-if="infoVisible" v-loading="isLoading">
    <el-alert
        v-if="!isPhoneValid || !phoneInfo.city"
        title="查询失败，请检查输入的手机号码是否正确"
        type="error"
        effect="dark"
        :closable="false"
        show-icon>
    </el-alert>
    <div v-else>
      <p>运营商:
        <el-tag size="small">{{ phoneInfo.carrier }}</el-tag>
      </p>
      <p>省份:
        <el-tag size="small">{{ phoneInfo.prov }}</el-tag>
      </p>
      <p>城市:
        <el-tag size="small">{{ phoneInfo.city }}</el-tag>
      </p>
    </div>

  </div>

</template>

<style scoped>

</style>