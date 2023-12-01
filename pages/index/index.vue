<template>
	<view class="container">
		<view><text>{{province}}</text></view>
		<view><text>{{city}}</text></view>
		<view><text>{{county}}</text></view>
	</view>
	<button @click="onClick" type="primary" class="btn">{{btnText}}</button>
</template>

<script setup>
import { ref } from 'vue';
import area from '../../static/area.js';

const province = ref('');
const city = ref('');
const county = ref('');
const provinceCode = ref('');
const cityCode = ref('');
const countyCode = ref('');
const timer = ref();
const btnText = ref('开始');


const getProvince = () => {
	const provinceKey = Object.keys(area.province);
	const pkIdx = parseInt(String(Math.random() * provinceKey.length));
	const pkCode = provinceKey[pkIdx];
	const target = area.province[pkCode];
	return {
		name: target,
		code: pkCode
	}
}
const getCity = (pCode) => {
	const cityKey = Object.keys(area.city).filter(d => d.startsWith(pCode.slice(0, 2)));
	const cityIdx = parseInt(String(Math.random() * cityKey.length));
	const cityKeyCode = cityKey[cityIdx];
	const target = area.city[cityKeyCode];
	return {
		name: target,
		code: cityKeyCode
	}
}
const getCounty = (cCode) => {
	const countyKey = Object.keys(area.county).filter(d => d.startsWith(cCode.slice(0, 4)));
	const countyIdx = parseInt(String(Math.random() * countyKey.length));
	const countyKeyCode = countyKey[countyIdx];
	const target = area.county[countyKeyCode];
	return {
		name: target,
		code: countyCode
	}
}

const onClick = () => {
	if (btnText.value === '开始') {
		onStart();
		return;
	}
	if (btnText.value === '停止') {
		onStop();
		return;
	}
}

const onStart = () => {
	btnText.value = '停止';
	if (!province.value) {
		timer.value = setInterval(() => {
			const { name, code } = getProvince();
			province.value = name;
			provinceCode.value = code;
		}, 50)
	}
	
	if (province.value && !city.value) {
		timer.value = setInterval(() => {
			const { name, code } = getCity(provinceCode.value);
			city.value = name;
			cityCode.value = code;
		}, 50)
	}
	if (province.value && city.value && !county.value) {
		timer.value = setInterval(() => {
			const { name, code } = getCounty(cityCode.value);
			county.value = name;
			countyCode.value = code;
		}, 50)
	}
	if (province.value && city.value && county.value) {
		province.value = '';
		city.value = '';
		county.value = '';
		provinceCode.value = '';
		cityCode.value = '';
		countyCode.value = '';
		onStart()
	}
	
}
const onStop = () => {
	btnText.value = '开始';
	clearInterval(timer.value)
	timer.value = null;
}

</script>

<style scoped lang="scss">
	.container {
		width: 100%;
		height: calc(100vh - 150px);
		box-sizing: border-box;
		display: flex;
		flex-direction: column;
		justify-content: center;
		font-size: 38px;
		font-weight: bold;
		text-align: center;
	}
	.btn {
		width: 50%;
	}
</style>
