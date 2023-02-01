<template>
    
    <div class="audio-player">

        <div class="flex">
            <button @click="play" v-if="paused">&#9658;</button>
            <button @click="pause" v-if="playing">&#9208;</button>
            <div class="waveform" ref="waveform"></div>
            <span>{{ currentTime }} / {{ duration }}</span>
        </div>
    </div>
</template>
<style >
* {
    box-sizing: border-box;
}

.audio-player {
    border-radius: 0.25em;
    padding: 1em;
    margin-top: 2em;
    background-color: white;
    box-shadow: 0 0 10px -5px rgba(0, 0, 0, 0.15);
}

.flex {
    display: flex;
    width: 400px;
    height: 40px;
    align-items: center;
}

.waveform {
    flex-grow: 1;
    height: 40px !important;
    margin-left: 1em;
    margin-right: 1em;
}

span {
    font-size: 0.75em;
    color: #777;
    font-family: sans-serif;
}


</style>
<script>
import WaveSurfer from 'wavesurfer.js';

export default {
    props: ['file'],
    data() {
        return {
            waveSurfer: null,
            paused: true,
            playing: false,
            playbackRate: 1,
            currentTime: '0:00',
            duration: '0:00'         
        };
    },
    mounted() {
        this.waveSurfer = WaveSurfer.create({
            container: this.$refs.waveform,
            waveColor: '#ddd',
            progressColor: '#aaa',
            backgroundColor: 'white',
            barWidth: 3,
            height: 40,
            cursorWidth: 0,
            barGap: 3
        });

        this.waveSurfer.on('play', () => {
            this.playing = true;
            this.paused = false;
        })

        this.waveSurfer.on('pause', () => {
            this.playing = false;
            this.paused = true;
        })
        this.waveSurfer.on('finish', () => {
            this.playing = false;
            this.paused = true;
        })
        this.waveSurfer.on('ready', this.updateDuration);
        this.waveSurfer.on('audioprocess', this.updateCurrentTime);
    },
    methods: {
        play() {
            this.waveSurfer.play();
        },
        pause() {
            this.waveSurfer.pause()
        },
        loadFile() {            
            this.waveSurfer.loadBlob(this.file);            
        },
        updateDuration() {
            this.duration = this.formatTime(this.waveSurfer.getDuration());
        },
        updateCurrentTime() {
            this.currentTime = this.formatTime(this.waveSurfer.getCurrentTime());
        },
        formatTime(time) {
            const minutes = Math.floor(time / 60);
            const seconds = Math.floor(time % 60);
            return `${minutes}:${seconds < 10 ? '0' : ''}${seconds}`;
        }
    },
    watch: {
        file: {
            immediate: true,
            handler(value) {
                this.$nextTick(() => {

                    this.loadFile()
                })
            }
        }
    }
};
</script>

<style>
.waveform {
    height: 120px;
    background-color: #ddd;
}
button {
    border-radius: 8px;
    border: 1px solid transparent;
    padding: 0.6em 1.2em;
    font-size: 1em;
    font-weight: 500;
    font-family: inherit;
    background-color: #1a1a1a;
    cursor: pointer;
    transition: border-color 0.25s;
}

button:hover {
    border-color: #646cff;
}

button:focus,
button:focus-visible {
    outline: 4px auto -webkit-focus-ring-color;
}

@media (prefers-color-scheme: light) {
    :root {
        color: #213547;
        background-color: #ffffff;
    }

    a:hover {
        color: #747bff;
    }

    button {
        background-color: #f9f9f9;
    }
}
</style>