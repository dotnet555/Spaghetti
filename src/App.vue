<template lang="pug">
h1 Spaghetti

div
	input(type="text" v-model="spaghetti" maxlength="100")

div(style="margin-top: 15px;")
	svg(xmlns="http://www.w3.org/2000/svg" :width="svgWidth" height="600")
		image(
			v-for="p in spaghettiParts"
			:href="p.img"
			:y="p.y"
			:x="imagesX"		
		)

</template>

<script setup lang="ts">
import { ref, onMounted, watch } from "vue";
import ghe from "/ghe.png";
import s from "/s.png";
import spa from "/spa.png";
import tti from "/tti.png";
import spaghet from "/spaghet.png";

const spaghetti = ref("spaghetti");

const imagesWidth = 264;
const svgWidth = 400;
const imagesX = svgWidth / 2 - imagesWidth / 2;

const spaghettiParts = ref(new Array<ISpaghettiPart>());

let timeoutHandle = undefined as number | undefined;

const imgs: Record<string, any> = {
	ghe: ghe,
	s: s,
	spa: spa,
	tti: tti,
	spaghet: spaghet,
};

const invalidSpaghetti = [{ img: imgs["spaghet"], y: 0 }];

watch(
	() => spaghetti.value,
	() => {
		clearTimeout(timeoutHandle);
		timeoutHandle = setTimeout(() => {
			spaghettiParts.value = buildSpaghetti();
		}, 500);
	}
);

onMounted(() => {
	spaghettiParts.value = buildSpaghetti();
});

function buildSpaghetti() {
	const parts = new Array<ISpaghettiPart>();

	var spaghettiString = spaghetti.value.toLocaleLowerCase().trim();

	if (spaghettiString.length == 0) return invalidSpaghetti;

	let i = 0;
	while (spaghettiString.length > 0 && i < 100) {
		i++;
		let foundPart = null as string | null;

		if (spaghettiString.startsWith("spa")) {
			foundPart = "spa";
		} else if (spaghettiString.startsWith("ghe")) {
			foundPart = "ghe";
		} else if (spaghettiString.startsWith("tti")) {
			foundPart = "tti";
		} else if (spaghettiString.startsWith("s")) {
			foundPart = "s";
		}

		if (foundPart == null) return invalidSpaghetti;

		pushFront(parts, imgs[foundPart], parts.length * 15);
		spaghettiString = spaghettiString.replace(foundPart, "");
	}

	if (i >= 100) return invalidSpaghetti;

	return parts;
}

function pushFront(arr: Array<ISpaghettiPart>, img: string, y: number) {
	arr.splice(0, 0, { img: img, y: y });
}

interface ISpaghettiPart {
	img: string;
	y: number;
}
</script>

<style lang="stylus">
#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #2c3e50;
	margin-top: 60px;
}

input[type="text"]
	text-align center
</style>
