<script>
import { mapActions } from "pinia";
import { usePostStore } from "../stores/post";

export default {
  props: ["post"],
  data() {
    return {
      flagUrl: `https://countryflagsapi.com/png`,
      weatherIconUrl: `http://openweathermap.org/img/w`,
    };
  },
  computed: {
    formattedDate() {
      return this.post.createdAt.substring(0, 19).replace("T", " ");
    },
  },
  methods: {
    ...mapActions(usePostStore, ["fetchPost"]),
    async fetchPostComponent() {
      this.fetchPost(this.post.id);
      this.$router.push(`/post/${this.post.id}`);
    },
  },
};
</script>

<template>
  <div class="col d-flex justify-content-center">
    <div class="card" style="width: 40rem" @click="fetchPostComponent">
      <img :src="post.imageUrl" class="card-img-top" alt="card-image" />
      <div class="card-body">
        <div class="row">
          <div class="col d-flex align-items-center">
            <img
              :src="`${flagUrl}/${post.country}`"
              alt="flag-icon"
              class="flag-icon"
              style="border: black 0.5pt solid"
            />
            <div class="mt-2">
              <h4 class="fw-bolder card-location">{{ post.location }}</h4>
            </div>
          </div>
          <div class="col mt-2">
            <img
              :src="`${weatherIconUrl}/${post.weatherIcon}.png`"
              alt="weather-icon"
              class="weather-icon"
            />
          </div>
        </div>
        <h6 class="weather-description card-weather-desc">
          {{ post.weatherMain }}, {{ post.weatherDescription }}
        </h6>
        <p class="card-caption mb-4">
          {{ post.caption }}
        </p>
        <p class="card-created-at">posted at: {{ formattedDate }}</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.flag-icon {
  float: left;
  width: 3rem;
  margin-right: 0.5rem;
}
.weather-main,
.card-created-at {
  text-align: right;
}

.weather-icon {
  float: right;
}

.card-caption {
  text-align: justify;
}

.card-img-top {
  height: 250px;
}
</style>
