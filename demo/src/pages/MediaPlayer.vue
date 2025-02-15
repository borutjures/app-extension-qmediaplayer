<template>
  <div class="q-layout-padding q-mx-auto q-gutter-sm" style="max-width: 1000px; width: 100%; min-height: 100vh;">

    <div class="row flex-center">
      <p>This page is intended to test multiple scenarios of QMediaPlayer.</p>
      <p>Music courtesy of <a href="http://freemusicarchive.org/music/Dee_Yan-Key/years_and_years_ago/01--Dee_Yan-Key-Driving_Home" target="blank">Free Music Archive</a></p>
      <p style="text-align: center;">Videos and subtitles courtesy of <a href="https://mango.blender.org/download/" target="blank">Blender Organization</a>.</p>
    </div>

    <div class="row flex-center">
      <q-item tag="label" class="q-my-sm bg-blue-grey-6 shadow-1" :dark="!darkbg" style="border-radius: 30px">
        <q-item-section>
          <q-item-label>Dark background (audio)</q-item-label>
        </q-item-section>

        <q-item-section side>
          <q-toggle v-model="darkbg" color="blue-grey-2" :dark="darkbg"></q-toggle>
        </q-item-section>
      </q-item>

      <q-card class="q-mx-auto" style="max-width: 800px; width: 100%; max-width: 90vw;">
        <q-card-section class="text-center">
          <div class="row flex-center q-gutter-sm">
            <q-toggle v-model="dense" label="Dense"></q-toggle>
            <q-toggle v-model="dark" label="Dark"></q-toggle>
            <q-toggle v-model="radius" label="Radius"></q-toggle>
            <q-toggle v-model="muted" label="Muted"></q-toggle>
            <q-toggle v-model="playsinline" label="Plays Inline"></q-toggle>
            <q-toggle v-model="loop" label="Loop"></q-toggle>
          </div>
          <div class="row flex-center q-gutter-sm">
            <q-toggle v-model="bigPlay" label="Big Play Button" :disable="!videoType"></q-toggle>
            <q-toggle v-model="overlay" label="Overlay" :disable="!videoType"></q-toggle>
            <q-toggle v-model="mobileMode" label="Mobile Mode" :disable="!videoType"></q-toggle>
          </div>
          <div class="row flex-center q-gutter-sm">
            <q-toggle v-model="videoType" label="Video"></q-toggle>
            <q-btn label="Next Video" :disable="!videoType" @click="nextVideo"></q-btn>
            <q-toggle v-model="autoPlay" label="Autoplay"></q-toggle>
          </div>
        </q-card-section>
      </q-card>
    </div>

    <div class="row flex-center" style="min-height: 2rem;">
      <q-media-player
        :type="videoType ? 'video' : 'audio'"
        :dense="dense"
        :dark="dark"
        :background-color="darkbg ? 'black' : 'white'"
        :mobile-mode="mobileMode"
        :muted="muted"
        :playsinline="playsinline"
        :loop="loop"
        :radius="radius ? '1rem' : 0"
        :autoplay="autoPlay"
        :show-big-play-button="bigPlay"
        :sources="videoType ? video[videoIndex].sources : audio.sources"
        :poster="videoType ? video[videoIndex].poster : ''"
        :tracks="videoType ? video[videoIndex].tracks : []"
        track-language="English"
        @ended="onEnded"
      >
        <template v-if="overlay" v-slot:overlay>
          <div>
            <img
              src="statics/quasar-logo.png"
              style="width: 30vw; max-width: 50px; opacity: 0.25;"
            >
          </div>
        </template>
      </q-media-player>
    </div>
    <div class="text-center">
      <p>Quasar App Extension QMediaPlayer <a href="https://github.com/quasarframework/app-extension-qmediaplayer" target="_blank">home page</a>.</p>
      <p>This project's <a href="https://github.com/quasarframework/app-extension-qmediaplayer/tree/master/demo" target="_blank">home page</a>.</p>
    </div>

  </div>
</template>

<style>
</style>

<script>
export default {
  name: 'PageIndex',
  data () {
    return {
      darkbg: true,
      dark: false,
      dense: false,
      videoType: true,
      mobileMode: false,
      muted: false,
      playsinline: false,
      loop: false,
      bigPlay: true,
      radius: false,
      overlay: false,

      videoIndex: 0,
      autoPlay: false,
      sources: [],

      audio: {
        sources: [
          {
            src: 'statics/media/Dee_Yan-Key_-_01_-_Driving_Home.mp3',
            type: 'audio/mp3'
          }
        ]
      },

      video: [
        {
          label: 'Tears of Steel',
          poster: 'statics/media/TearsOfSteel/TearsOfSteel.jpeg',
          sources: [
            {
              src: 'https://ftp.nluug.nl/pub/graphics/blender/demo/movies/ToS/ToS-4k-1920.mov',
              type: 'video/mp4'
            }
          ],
          tracks: [
            {
              src: 'statics/media/TearsOfSteel/TOS-en.vtt',
              kind: 'subtitles',
              srclang: 'en',
              label: 'English'
            },
            {
              src: 'statics/media/TearsOfSteel/TOS-de.vtt',
              kind: 'subtitles',
              srclang: 'de',
              label: 'German'
            },
            {
              src: 'statics/media/TearsOfSteel/TOS-es.vtt',
              kind: 'subtitles',
              srclang: 'es',
              label: 'Spanish'
            },
            {
              src: 'statics/media/TearsOfSteel/TOS-fr-Goofy.vtt',
              kind: 'subtitles',
              srclang: 'fr',
              label: 'French'
            },
            {
              src: 'statics/media/TearsOfSteel/TOS-it.vtt',
              kind: 'subtitles',
              srclang: 'it',
              label: 'Italian'
            },
            {
              src: 'statics/media/TearsOfSteel/TOS-nl.vtt',
              kind: 'subtitles',
              srclang: 'nl',
              label: 'Dutch'
            }
          ]
        },
        {
          label: 'Sintel',
          poster: 'statics/media/sintel/sintel-poster2.jpeg',
          sources: [
            {
              src: 'https://peach.themazzone.com/durian/movies/sintel-2048-surround.mp4',
              type: 'video/mp4'
            }
          ],
          tracks: [
            {
              src: 'statics/media/sintel/sintel-en.vtt',
              kind: 'subtitles',
              srclang: 'en',
              label: 'English'
            },
            {
              src: 'statics/media/sintel/sintel-de.vtt',
              kind: 'subtitles',
              srclang: 'de',
              label: 'Deutsch'
            },
            {
              src: 'statics/media/sintel/sintel-es.vtt',
              kind: 'subtitles',
              srclang: 'es',
              label: 'Español'
            },
            {
              src: 'statics/media/sintel/sintel-fr.vtt',
              kind: 'subtitles',
              srclang: 'fr',
              label: 'Français'
            },
            {
              src: 'statics/media/sintel/sintel-it.vtt',
              kind: 'subtitles',
              srclang: 'it',
              label: 'Italiano'
            },
            {
              src: 'statics/media/sintel/sintel-nl.vtt',
              kind: 'subtitles',
              srclang: 'nl',
              label: 'Nederlands'
            },
            {
              src: 'statics/media/sintel/sintel-pt.vtt',
              kind: 'subtitles',
              srclang: 'pt',
              label: 'Português'
            },
            {
              src: 'statics/media/sintel/sintel-pl.vtt',
              kind: 'subtitles',
              srclang: 'pl',
              label: 'Polski'
            },
            {
              src: 'statics/media/sintel/sintel-ru.vtt',
              kind: 'subtitles',
              srclang: 'ru',
              label: 'Russian'
            }
          ]
        }
      ]
    }
  },

  created () {
    this.setSource()
  },

  mounted () {
  },

  computed: {
  },

  watch: {
    videoType (val) {
      this.setSource()
    }
  },

  methods: {
    setSource () {
      if (this.videoType) {
        this.sources = [...this.video[this.videoIndex].sources]
      } else {
        this.sources = [...this.audio.sources]
      }
    },
    onEnded () {
      this.nextVideo()
    },
    nextVideo () {
      if (this.videoIndex === 0) {
        this.videoIndex = 1
      } else {
        this.videoIndex = 0
      }
      this.setSource()
    }
  }
}
</script>
