<template>
	<div id="app">
		<div v-if="joycon_L.controller">
			Left: {{ joycon_L.joystick.dir }}
		</div>
		<div v-if="joycon_R.controller">
			Right: {{ joycon_R.joystick.dir }}
		</div>
		<div class="needle" :style="{ 'transform': 'rotate('+ (this.testrad) +'rad )'}"/>
		<div class="needle red" :style="{ 'transform': 'rotate('+ (joycon_R.joystick.deg) +'deg )'}" />
	</div>
</template>


<script>
	var cancelAnimationFrame  = window.cancelAnimationFrame || window.mozCancelAnimationFrame;
	var requestAnimationFrame = window.requestAnimationFrame || window.mozRequestAnimationFrame ||
                           		window.webkitRequestAnimationFrame || window.msRequestAnimationFrame;


	export default {
		name: 'app',
		
		components: {},

		data:() => ({
			testrad: 0,
			gamepads: [],
			// gamepadPositions: [0, 0.78539816339, 1.57079632679, 2.35619449019, 3.14159265359, 3.92699081699, 4.71238898038, 5.49778714378, 6.28318530718],
			joycon_L: {
				animation: null,
				controller: null,
				joystick: {
					dir: null,
					deg: null
				}
			},
			joycon_R: {
				animation: null,
				controller: null,
				joystick: {
					dir: null,
					deg: null

				}
			},
			controllerMatrix: {
				L: [
					// value:Boolean, degrees:Number, raw:Number, label:String
					[{v:0, d:220, r:0.429, l:'NW'},{v:0, d:270, r:0.714, l:'N'},{v:0, d:320, r:1.000, l:'NE'}],
					[{v:0, d:180, r:0.143, l:'W'},{v:0, d:0, r:1.286, l:'O'},{v:0, d:360, r:-1.000, l:'E'}],
					[{v:0, d:135, r:-0.143, l:'SW'},{v:0, d:90, r:-0.429, l:'S'},{v:0, d:45, r:-0.714, l:'SE'}],
				],
				R: [
					[{v:0, d:220, r:-0.714, l:'NW'},{v:0, d:270, r:-0.429, l:'N'},{v:0, d:320, r:-0.143, l:'NE'}],
					[{v:0, d:180, r:-1.000, l:'W'},{v:0, d:0, r:1.286, l:'O'},{v:0, d:360, r:0.143, l:'E'}],
					[{v:0, d:135, r:1.000, l:'SW'},{v:0, d:90, r:0.714, l:'S'},{v:0, d:45, r:0.429, l:'SE'}],
				]
			},
			pollTimer: {
				timer: null,
				interval: 0,
				running: false
			},
		}),
		
		mounted() {
			this.init()
		},

		methods: {
			init() {
				this.pollTimer.time = setInterval(this.pollConnectedGamepads, this.pollTimer.interval)

				let self = this

				window.addEventListener('gamepadconnected', function(e) {
					let gamepad = navigator.getGamepads()[e.gamepad.index]
					self.gamepads.push(gamepad)
					
					self.initJoycon(gamepad)

					console.log(e)
				})

				window.addEventListener('gamepaddisconnected', function(e) {
					self.cancelAnimationLoop(e.gamepad)
				})
			},



			initJoycon(gamepad) {
				this.pollTimer.running = true

				if (gamepad.id.includes('L')) {
					this.joycon_L.animation = requestAnimationFrame(this.controllerLoop_Left)
					console.log("Joy-Con (L) conneted", gamepad)

				} else if (gamepad.id.includes('R')) {
					this.joycon_R.animation = requestAnimationFrame(this.controllerLoop_Right)
					console.log("Joy-Con (R) conneted", gamepad)

				} else {
					console.log("Gamepad not currently supported")
				}
			},

			

			pollConnectedGamepads() {
				if (this.pollTimer.running) {
					
					this.gamepads = navigator.getGamepads ? navigator.getGamepads() : (navigator.webkitGetGamepads ? navigator.webkitGetGamepads : []);
					
					Array.prototype.forEach.call(this.gamepads, gamepad => {
						if (!gamepad) return

						if (gamepad.id.includes('L')) {
							this.joycon_L.controller = gamepad							
							this.computeData('L')

						} else if (gamepad.id.includes('R')) {
							this.joycon_R.controller = gamepad
							this.computeData('R')
						}
					})
				}
			},



			controllerLoop_Left() {
				this.joycon_L.animation = requestAnimationFrame(this.controllerLoop_Left)
			},

			controllerLoop_Right() {
				this.joycon_R.animation = requestAnimationFrame(this.controllerLoop_Right)
			},


			computeData(C) {
				let c_data = this.controllerMatrix[C]
				let joycon = this['joycon_'+C]
				let joycon_axis = joycon.controller.axes
				
				let rad = Math.atan2(joycon_axis[1], joycon_axis[0]) + (Math.PI/2) // rotate 90deg
				this.testrad = (rad != Math.PI/2) ? rad : 0 

            	let numOfRows = c_data.length;
				let numOfCols = c_data[0].length;
             
				for (let i = 0; i < numOfRows; ++i) {
					for (let j = 0; j < numOfCols; ++j) {

						c_data[i][j].v = 0
						if (joycon_axis == c_data[i][j].r) {
							c_data[i][j].v = 1
							joycon.joystick.dir = c_data[i][j].l
							joycon.joystick.deg = c_data[i][j].d
						}
					}				
				}
			},


			cancelAnimationLoop(gamepad){
				if (gamepad.id.includes('L')) {
					this.joycon_L.controller = null
					cancelAnimationFrame(this.joycon_L.animation)
					console.log("Joy-Con (L) disconnected")

				} else if (gamepad.id.includes('R')) {
					this.joycon_R.controller = null
					cancelAnimationFrame(this.joycon_R.animation)
					console.log("Joy-Con (R) disconnected")
				}
			},

		}
	}
</script>



<style>
	html {
		background-color: black;
		color: white;

	}
		.needle {
			width: 100px;
			height: 2px;
			background-color: blue;
			transform-origin: left;
			/* transition: 200ms linear; */
			/* transform: rotate(-1); */
			position: absolute;
			left: 50%;
			top: 50%;
		}
		
		.needle.red {
			background-color: red;
		}	
</style>
