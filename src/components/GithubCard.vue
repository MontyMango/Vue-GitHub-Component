<script setup>
import { onMounted, ref } from 'vue'
const githubLogin = ref('')
const githubName = ref('')
const githubAvatarURL = ref('')
const githubAccountURL = ref('')
const githubFollowers = ref(0)
const githubFollowing = ref(0)

const props = defineProps({
  username: {
    type: String,
    required: true,
    validator: (value) => {
      return typeof value == 'string'
    },
  },
})

onMounted(() => {
  // Fetch for GitHub username
  const URL = `https://api.github.com/users/${props.username}`
  console.log(URL)
  fetch(URL)
    .then((response) => response.json()) // DO response.json() first!!!
    .then((response) => {
      githubAvatarURL.value = response.avatar_url
      githubAccountURL.value = response.html_url
      githubLogin.value = response.login
      githubName.value = response.name
      githubFollowers.value = response.followers
      githubFollowing.value = response.following
    })
    .catch((error) => {
      console.error('GitHubCard error', error)
    })
})
</script>

<template>
  <div class="block">
    <div class="avatar">
      <img class="avatar-photo" :src="githubAvatarURL" alt="A GitHub profile picture" />
    </div>

    <div class="information">
      <div class="content">
        <div class="title">
          <p class="login">{{ githubLogin }}</p>
          <p class="aka">(aka {{ githubName }})</p>
        </div>

        <div class="stats">
          <p class="subtitle">Followers: {{ githubFollowers }}</p>
          <p class="subtitle">Following: {{ githubFollowing }}</p>
        </div>
      </div>

      <div class="link-btn">
        <a class="link" :href="githubAccountURL">Go to GitHub profile</a>
      </div>
    </div>
  </div>
</template>

<style lang="css" scoped>
/* Main block style */
.block {
  display: inline flex;
  background-color: darkgreen;
  border-radius: 15px;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
}

/* div sections */
.avatar-photo {
  width: auto;
  height: 125px;
  padding: 15px 15px;
}
.content {
  display: flex;
  flex-flow: column wrap;
  background-color: rgb(28, 160, 0);
  border-radius: 15px;
}
.information {
  display: flex;
  flex-flow: column wrap;
  justify-items: center;
  margin: 15px 15px 0 5px;
}
.title {
  display: flex;
  flex-flow: row wrap;
  font-size: 16px;
  align-items: baseline;
}
.stats {
  display: flex;
  flex-flow: row wrap;
  justify-content: space-evenly;
}
.link-btn {
  display: inline-block;
  margin-top: 10px;
  text-align: center;
}

/* Content div */
.login,
.aka,
.subtitle {
  /* Revert the flexbox margin to make the text look close */
  margin: 5px;
}
.login {
  font-size: 24px;
}

/* Button Formatting */
.link {
  display: inline-block;
  background-color: rgb(28, 160, 0);
  border: 1px black;
  padding: 15px 25px;
  border-radius: 25px;
  text-decoration: none;
  color: black;
  transition: background-color 0.5s;
}
.link:hover {
  background-color: rgb(95, 200, 0);
}
</style>
