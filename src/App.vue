<template>
    <div id="app">
        <audio-test ref="audio" />
		
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
	import AudioTest from '@/components/WebAudio'
	import ButtonTest from '@/components/ButtonTest'
	
	var cancelAnimationFrame  = window.cancelAnimationFrame || window.mozCancelAnimationFrame;
	var requestAnimationFrame = window.requestAnimationFrame || window.mozRequestAnimationFrame ||
                           		window.webkitRequestAnimationFrame || window.msRequestAnimationFrame;
   
    export default {
        name: 'app',

        components: {
			'audio-test'  : AudioTest,
			'button-test' : ButtonTest
		},

        data:() => ({
            gamepads: [],
            joycon_L: {
				connected : false,
				radians   : 0,
				buttons: {
					up: false,
					down: false,
					left: false,
					right: false,
					l: false,
					zl: false,
					l3: false,
					minus: false,
					sl: false,
					sr: false,
					capture: false
				}
            },
            joycon_R: {
				connected: false,
				radians: 0,
				buttons: {
					a: false,
					b: false,
					x: false,
					y: false,
					r: false,
					zr: false,
					r3: false,
					plus: false,
					sl: false,
					sr: false,
					home: false
				}
            },
            pollTimer: {
                timer: null,
                interval: 0,
                running: false
            }
        }),

        computed: {},

        mounted() {
            this.init()
        },

        methods: {
            init() {
				let self = this
                this.pollTimer.timer = setInterval(this.pollConnectedGamepads, this.pollTimer.interval)

                window.addEventListener('gamepadconnected', function(e) {
					let gamepad = navigator.getGamepads()[e.gamepad.index]	
					self.initJoycon(gamepad)
				})

				window.addEventListener('gamepaddisconnected', function(e) {
					self.cancelAnimationLoop(e.gamepad)
				})
            },

            initJoycon(gamepad) {
				 if (gamepad.id.includes('Joy-Con L+R')) {
					this.joycon_L.connected = true
					this.joycon_R.connected = true
					this.pollTimer.running = true
				} else if (gamepad.id.includes('Joy-Con (L)')) {
					this.joycon_L.connected = true
					console.log("Please connect the RIGHT Joy-Con")
				} else if (gamepad.id.includes('Joy-Con (R)')) {
					this.joycon_R.connected = true
					console.log("Please connect the LEFT Joy-Con")
				}
            },

            pollConnectedGamepads() {
				if (this.pollTimer.running) {
					this.gamepads = navigator.getGamepads ? navigator.getGamepads() : (navigator.webkitGetGamepads ? navigator.webkitGetGamepads : []);
					
					Array.prototype.forEach.call(this.gamepads, gamepad => {
						if (!gamepad) return

						if (gamepad.id.includes('Joy-Con L+R')) {
							this.computeData(gamepad)
						}
					})
				}
			},
			
			computeData(gamepad) {
				// console.log(gamepad)

				// RIGHT
				this.joycon_R.radians = Math.atan2(gamepad.axes[3], gamepad.axes[2])
				
				this.joycon_R.buttons.a = gamepad.buttons[1].pressed
				this.joycon_R.buttons.b = gamepad.buttons[0].pressed
				this.joycon_R.buttons.x = gamepad.buttons[3].pressed
				this.joycon_R.buttons.y = gamepad.buttons[2].pressed
				this.joycon_R.buttons.r = gamepad.buttons[5].pressed
				this.joycon_R.buttons.zr = gamepad.buttons[7].pressed
				this.joycon_R.buttons.r3 = gamepad.buttons[11].pressed
				this.joycon_R.buttons.plus = gamepad.buttons[9].pressed
				this.joycon_R.buttons.home = gamepad.buttons[16].pressed
				this.joycon_R.buttons.sl = gamepad.buttons[20].pressed
				this.joycon_R.buttons.sr = gamepad.buttons[21].pressed

				// LEFT
				this.joycon_L.radians = Math.atan2(gamepad.axes[1], gamepad.axes[0])

				this.joycon_L.buttons.up = gamepad.buttons[12].pressed
				this.joycon_L.buttons.down = gamepad.buttons[13].pressed
				this.joycon_L.buttons.left = gamepad.buttons[14].pressed
				this.joycon_L.buttons.right = gamepad.buttons[15].pressed
				this.joycon_L.buttons.l = gamepad.buttons[4].pressed
				this.joycon_L.buttons.zl = gamepad.buttons[6].pressed
				this.joycon_L.buttons.l3 = gamepad.buttons[10].pressed
				this.joycon_L.buttons.minus = gamepad.buttons[8].pressed
				this.joycon_L.buttons.capture = gamepad.buttons[17].pressed
				this.joycon_L.buttons.sl = gamepad.buttons[18].pressed
				this.joycon_L.buttons.sr = gamepad.buttons[19].pressed
			},

			cancelAnimationLoop(gamepad){
				this.pollTimer.running = false

				if (gamepad.id.includes('Joy-Con (L)')) {
					this.joycon_L.connected = false
					console.log("Joy-Con (L) disconnected")

				} else if (gamepad.id.includes('Joy-Con (R)')) {
					this.joycon_R.connected = false
					console.log("Joy-Con (R) disconnected")

				} else if (gamepad.id.includes('Joy-Con L+R')) {
					this.joycon_L.connected = false
					this.joycon_R.connected = false
				}
			}
		},
		
		watch:	 {
			joycon_L: {
				deep: true,
				handler(oldVal, newVal) {
					console.log(oldVal.buttons.up, newVal.buttons.up)
					if (newVal.buttons.zl) this.$refs.audio.snare()
					if (newVal.buttons.l) this.$refs.audio.clap()
				}
			},
			joycon_R: {
				deep: true,
				handler(oldVal, newVal) {
					if (newVal.buttons.a) this.$refs.audio.play()
					if (newVal.buttons.zr) this.$refs.audio.kick()
				}
			}
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

	p {
		background-color: red;
	}

	p.active {
		background-color: blue;
	}

	.running-div {

	}

	.running-div p.running {
		background-color: green;
	}
</style>