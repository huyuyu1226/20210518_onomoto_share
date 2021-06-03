<template>
  <div class="flex">
    <div class="left">
      <SideNavi />
    </div>
    <div class="right">
      <div class="title">
        <p>最近のお気に入り</p>
      </div>
      <div v-for="(value,index) in share" :key="index">
        <p class="main" v-if="(value.id == id)">{{value.share}}</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import SideNavi from "../components/SideNavi";
export default {
  data() {
    return {
      name:this.$store.state.user.name,
      id:"",
      share:[],
      like:[]
    }
  },
  methods: {
    async getLikes() {
      await axios.get("https://tranquil-bastion-65917.herokuapp.com/api/like")
      .then((response => {
        console.log(response.data.data)
        const responseLikes = response.data.data
        this.like = responseLikes
      }))
      await axios.get("https://tranquil-bastion-65917.herokuapp.com/api/shares")
      .then((response => {
       console.log(response.data.data)
       const responseShares = response.data.data
       this.share = responseShares
     }))
     this.like.forEach((element) => {
       this.id = element.share_id
     })
    }
  },
  created() {
    this.getLikes();
  },
  components: {
    SideNavi
  }
}
</script>

<style scoped>
.left {
  width: 22%;
  height: 100vh;
}
.right {
  width: 78%;
  height: 100vh;
}
.flex {
  display: flex;
}
.title {
  border-bottom: 1px solid white;
  border-left: 1px solid white;
  padding: 15px;
}
.title p {
  font-size: 20px;
  font-weight: bold;
}
.main {
  border-bottom: 1px solid white;
  border-left: 1px solid white;
  padding: 15px;
}
</style>