<template>
    <div>
        <div v-if="joycon_L.radians != 0" class="needle" :style="{ 'transform': 'rotate('+ (joycon_L.radians) +'rad )'}"/>
		<div v-if="joycon_R.radians != 0" class="needle red" :style="{ 'transform': 'rotate('+ (joycon_R.radians) +'rad )'}" />

		<div class="running-div">
			<p :class="{'running':pollTimer.running}">{{pollTimer.running}}</p>
		</div>

		<div v-if="joycon_L.connected">
			LEFT:
			<p :class="{'active':joycon_L.buttons.up}">UP: {{joycon_L.buttons.up}}</p>
			<p :class="{'active':joycon_L.buttons.down}">DOWN: {{joycon_L.buttons.down}}</p>
			<p :class="{'active':joycon_L.buttons.left}">LEFT: {{joycon_L.buttons.left}}</p>
			<p :class="{'active':joycon_L.buttons.right}">RIGHT: {{joycon_L.buttons.right}}</p>
			<p :class="{'active':joycon_L.buttons.l}">L: {{joycon_L.buttons.l}}</p>
			<p :class="{'active':joycon_L.buttons.zl}">ZL: {{joycon_L.buttons.zl}}</p>
			<p :class="{'active':joycon_L.buttons.l3}">L3: {{joycon_L.buttons.l3}}</p>
			<p :class="{'active':joycon_L.buttons.minus}">-: {{joycon_L.buttons.minus}}</p>
			<p :class="{'active':joycon_L.buttons.capture}">CAPTURE: {{joycon_L.buttons.capture}}</p>
			<p :class="{'active':joycon_L.buttons.sl}">SL: {{joycon_L.buttons.sl}}</p>
			<p :class="{'active':joycon_L.buttons.sr}">SR: {{joycon_L.buttons.sr}}</p>
		</div>

		<div v-if="joycon_R.connected">
			RIGHT:
			<p :class="{'active':joycon_R.buttons.x}">X: {{joycon_R.buttons.x}}</p>
			<p :class="{'active':joycon_R.buttons.b}">B: {{joycon_R.buttons.b}}</p>
			<p :class="{'active':joycon_R.buttons.y}">Y: {{joycon_R.buttons.y}}</p>
			<p :class="{'active':joycon_R.buttons.a}">A: {{joycon_R.buttons.a}}</p>
			<p :class="{'active':joycon_R.buttons.r}">R: {{joycon_R.buttons.r}}</p>
			<p :class="{'active':joycon_R.buttons.zr}">ZR: {{joycon_R.buttons.zr}}</p>
			<p :class="{'active':joycon_R.buttons.r3}">R3: {{joycon_R.buttons.r3}}</p>
			<p :class="{'active':joycon_R.buttons.plus}">+: {{joycon_R.buttons.plus}}</p>
			<p :class="{'active':joycon_R.buttons.home}">HOME: {{joycon_R.buttons.home}}</p>
			<p :class="{'active':joycon_R.buttons.sl}">SL: {{joycon_R.buttons.sl}}</p>
			<p :class="{'active':joycon_R.buttons.sr}">SR: {{joycon_R.buttons.sr}}</p>
		</div>
    </div>
</template>

<script>
    export default {
        name: ''
    }
</script>