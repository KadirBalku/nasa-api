<script>
export default {
  data() {
    return {
      apod: "",
      apiKey: "api_key=" + process.env.VUE_APP_API_KEY, //api key
      url: "https://api.nasa.gov/planetary/apod?",
    };
  },
  computed: {
    currentDate() {
      const date = new Date();
      let day = date.getDate() - 1;
      let month = date.getMonth() + 1;
      let year = date.getFullYear();

      return `&date=${year}-${month}-${day}`;
    },

    isImage() {
      return this.apod.media_type === "image";
    },

    hasCopyright() {
      return this.apod.copyright !== undefined;
    },
  },
  methods: {
    async fetchData(date) {
      this.apod = "";
      const res = await fetch(this.url + this.apiKey + date);
      this.apod = await res.json();
    },

    datePicker(event) {
      let date = `&date=${event.target.value}`;
      this.fetchData(date);
    },
  },
  mounted() {
    this.fetchData(this.currentDate);
  },
};
</script>

<template>
  <section class="component__container" id="apod">
    <h2>Astronomy Picture of the Day</h2>
    <p>{{ apod.date }}</p>
    <a :href="apod.hdurl" target="_blank">
      <div class="apod__card">
        <div class="apod__card--body">
          <div v-show="isImage" class="apod__card--img">
            <img
              :src="apod.url"
              class="apod__card--img--obj"
              alt="Astronomy Pict of the Day"
            />
            <h3 class="apod__card--title">
              {{ apod.title }}
              <div class="apod__card--title-border"></div>
            </h3>

            <p class="apod__card--desc">{{ apod.explanation }}</p>
          </div>
          <div class="apod__card--video" v-show="!isImage">
            <iframe
              v-show="!isImage"
              class="apod__card--video-obj"
              :src="apod.url"
            ></iframe>
            <h3 class="apod__card--title">
              {{ apod.title }}
              <div class="apod__card--title-border"></div>
            </h3>

            <p class="apod__card--desc">{{ apod.explanation }}</p>
          </div>
        </div>
        <div class="apod__card--footer">
          <p class="copyright" v-show="hasCopyright">{{ apod.copyright }}</p>
        </div>
      </div>
    </a>
    <div class="datepicker__wrapper">
      <label class="datepicker__label" for="apod-date">Choose a date</label>
      <input
        class="datepicker"
        type="date"
        name="pic-of-day-date"
        id="apod-date"
        @change="datePicker"
      />
    </div>
  </section>
</template>

<style scoped>
.component__container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 1rem 0;
  gap: 1rem;
}

[type="date"] {
  background: #fff
    url(https://cdn1.iconfinder.com/data/icons/cc_mono_icon_set/blacks/16x16/calendar_2.png)
    95% 55% no-repeat;
}

[type="date"]::-webkit-inner-spin-button {
  display: none;
}

[type="date"]::-webkit-calendar-picker-indicator {
  opacity: 0;
}

.datepicker__wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 1rem;
  width: 100%;
  margin-top: 3rem;
}

.datepicker__label {
  font-weight: bold;
  min-width: max-content;
  align-self: center;
}
.datepicker {
  padding: 0.25rem 0.5rem;
  border: 1px solid #999;
  border-radius: 10px;
  text-align: center;
  width: 200px;
}

.datepicker:focus {
  padding: 0.25rem 0.5rem;
  border: 1px solid #999;
  border-radius: 10px;
}

.apod__card {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  height: auto;
  width: 100%;
  max-width: 400px;
  height: 100%;
  max-height: 600px;
  padding: 0;
  margin: 0;
  border-radius: 10px;
  box-shadow: 0px 2px 12px black;
  overflow: hidden;
}

.apod__card--title {
  font-size: 1.5rem;
  font-weight: bold;
  color: #fff;
  padding: 1rem;
  position: absolute;
  top: 0;
  left: 0;
  transition: all 0.5s ease-in;
  filter: drop-shadow(3px 3px 2px black);
}

.apod__card--title-border {
  background: rgb(255, 255, 255);
  background: -moz-linear-gradient(
    90deg,
    rgba(255, 255, 255, 1) 0%,
    rgba(255, 255, 255, 0.5970763305322129) 40%,
    rgba(255, 255, 255, 0) 100%
  );
  background: -webkit-linear-gradient(
    90deg,
    rgba(255, 255, 255, 1) 0%,
    rgba(255, 255, 255, 0.5970763305322129) 40%,
    rgba(255, 255, 255, 0) 100%
  );
  background: linear-gradient(
    90deg,
    rgba(255, 255, 255, 1) 0%,
    rgba(255, 255, 255, 0.5970763305322129) 40%,
    rgba(255, 255, 255, 0) 100%
  );
  filter: progid:DXImageTransform.Microsoft.gradient(startColorstr="#ffffff",endColorstr="#ffffff",GradientType=1);
  margin-top: 0.5rem;
  height: 3px;
  width: 100%;
}

.apod__card--body {
}
.apod__card--img {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
}

.apod__card--img--obj {
  width: auto;
  height: 600px;
  filter: grayscale(100%) invert(50);
  transition: 0.5s ease-in-out all;
}

.apod__card--video {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 400px;
  height: 600px;
}

.apod__card--video-obj {
  width: 400px;
  height: 600px;
  filter: grayscale(100%);
}

.apod__card--desc {
  position: absolute;
  background-color: rgba(255, 255, 255, 0.705);
  font-style: italic;
  font-size: 0.75rem;
  font-weight: 600;
  align-self: flex-end;
  padding: 1rem;
  color: rgb(48, 48, 48);
  top: 100%;
  left: 0;
  opacity: 1;
  transform: translate(0, -100%);
  transition: opacity 0.5s ease-in;
}

.apod__card--desc::first-letter {
  color: black;
  font-size: 1.5rem;
}

.apod__card:hover .apod__card--img--obj {
  transform: translateX(60px) scaleX(150%) scaleY(150%);
  filter: grayscale(0);
}
.apod__card:hover .apod__card--video-obj {
  filter: grayscale(0);
}
.apod__card:hover .apod__card--desc,
.apod__card:hover .apod__card--title {
  opacity: 0;
}

.apod__card--video:hover .apod__card--desc,
.apod__card--video:hover .apod__card--title {
  display: none;
}

.copyright {
  position: absolute;
  color: white;
  font-weight: bold;
  background-color: rgba(0, 0, 0, 0.5);
  padding: 0.25rem 0.5rem;
  top: 99%;
  left: 90%;
  transform: translateX(-90%) translateY(-99%);
  width: max-content;
}
.copyright::before {
  content: "© ";
}

@media screen and (max-width: 1605px) {
}
</style>
