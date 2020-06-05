<template>
    <div>
        <audio ref="audioElement" :src="require('@/assets/audio/99_Bbm_PanChords_01_712.mp3')"></audio>
        <audio ref="audioElement_Kick" :src="require('@/assets/audio/Kick_06_712_01.mp3')"></audio>
        <audio ref="audioElement_Snare" :src="require('@/assets/audio/Snare_06_712_01.mp3')"></audio>
        <audio ref="audioElement_Clap" :src="require('@/assets/audio/Clap_07_712_01.mp3')"></audio>
    </div>
</template>

<script>
    let AudioContext = window.AudioContext || window.webkitAudioContext;
    let audioCtx = new AudioContext();
    
    export default {
        name: '',

        data:() => ({
            isPlaying: false
        }),

        mounted() {
            // const audioElement = document.querySelector('audio');
            // const track = audioCtx.createMediaElementSource(audioElement);
           
        },

        methods: {
            play() {
                // check if context is in suspended state (autoplay policy)
                if (audioCtx.state === 'suspended') {
                    audioCtx.resume()
                }
                if (!this.isPlaying) {
                    this.$refs.audioElement.play()

                    this.isPlaying = true
                // if track is playing pause it
                } else if (this.isPlaying) {
                    this.$refs.audioElement.pause()
                    this.isPlaying = false
                }
                // let state = this.getAttribute('aria-checked') === "true" ? true : false;
                // this.setAttribute( 'aria-checked', state ? "false" : "true" );
            },

            snare() {
                this.$refs.audioElement_Snare.play()
                // check if context is in suspended state (autoplay policy)
                /* if (audioCtx.state === 'suspended') {
                    audioCtx.resume()
                }
                if (!this.isPlaying) {
                    this.$refs.audioElement_Snare.play()

                    this.isPlaying = true
                // if track is playing pause it
                } else if (this.isPlaying) {
                    this.$refs.audioElement_Snare.pause()
                    this.isPlaying = false
                } */
                // let state = this.getAttribute('aria-checked') === "true" ? true : false;
                // this.setAttribute( 'aria-checked', state ? "false" : "true" );
            },

            kick() {
                this.$refs.audioElement_Kick.play()
                // check if context is in suspended state (autoplay policy)
                /* if (audioCtx.state === 'suspended') {
                    audioCtx.resume()
                }
                if (!this.isPlaying) {
                    this.$refs.audioElement_Kick.play()

                    this.isPlaying = true
                // if track is playing pause it
                } else if (this.isPlaying) {
                    this.$refs.audioElement_Kick.pause()
                    this.isPlaying = false
                } */
                // let state = this.getAttribute('aria-checked') === "true" ? true : false;
                // this.setAttribute( 'aria-checked', state ? "false" : "true" );
            },

            clap() {
                this.$refs.audioElement_Clap.play()
                // check if context is in suspended state (autoplay policy)
                /* if (audioCtx.state === 'suspended') {
                    audioCtx.resume()
                }
                if (!this.isPlaying) {
                    this.$refs.audioElement_Kick.play()

                    this.isPlaying = true
                // if track is playing pause it
                } else if (this.isPlaying) {
                    this.$refs.audioElement_Kick.pause()
                    this.isPlaying = false
                } */
                // let state = this.getAttribute('aria-checked') === "true" ? true : false;
                // this.setAttribute( 'aria-checked', state ? "false" : "true" );
            }
        }
    }
</script>