
<script>
import firebase from "firebase/app";
import { groupBy } from "lodash";
import "firebase/firestore";
const c = firebase.initializeApp({ projectId: "building-bfcec" });
console.log(c);
const db = c.firestore();

export default {
  name: "App",
  data: () => ({
    selected: null,
    companies: []
  }),
  computed: {
    companiesByFloor() {
      return groupBy(this.companies, "floor");
    }
  },
  watch: {
    companies: function(v) {
      if (v.length) this.selected = v[0];
    }
  },
  methods: {
    select(company) {
      this.selected = company;
    }
  },
  firestore: {
    companies: db.collection("company")
  }
};
</script>
<template>
  <div class="app">
    <div class="nav">
      <div class="logo">BUILDING DIRECTORY</div>
      <div class="floor" :key="floor" v-for="(companies, floor) in companiesByFloor">
        <div class="floor-no">Floor {{floor}}</div>
        <div
          class="company"
          :key="company.id"
          v-for="company in companies"
          @click="select(company)"
        >
          <div class="company-name">{{company.name}}</div>
          <div class="company-introduce">{{company.introduce}}</div>
        </div>
      </div>
    </div>
    <template v-if="selected">
      <div class="slide" :style="`background-image: url(${selected.slideUrl})`">
        <div class="slide-title">{{selected.slideTitle}}</div>
      </div>
    </template>
  </div>
</template>
<style>
* {
  margin: 0;
  padding: 0;
  font-family: "Nanum Gothic", sans-serif;
  line-height: 1;
}
.app {
  width: 100vw;
  height: 100vh;
  display: flex;
}
.nav {
  padding-top: 45px;
  width: 400px;
  background: #f4f4f4;
  box-shadow: 1px 1px 10px #ccc;
}
.slide {
  flex: 1;
  overflow: hidden;
  background-size: 100% 100%;
  display: flex;
}
.slide-title {
  margin: auto;
  width: 65%;
  color: #fff;
  font-size: 70px;
  text-align: center;
  font-weight: bold;
}
.logo {
  margin: 0 0 40px 45px;
  padding: 2px 15px;
  font-size: 32px;
  line-height: 1.2;
  color: #000000;
  border-left: 2.5px solid #277995;
  font-weight: 800;
}
.floor {
  margin-bottom: 23px;
}
.floor-no {
  padding-left: 48px;
  color: #277995;
  font-size: 12px;
  font-weight: 800;
  margin-bottom: 10px;
}
.company {
  background-color: #fff;
  padding: 14px 48px;
  border-bottom: 2px solid #e6e6e6;
  margin-bottom: 8px;
  cursor: pointer;
}
.company-name {
  color: #000;
  font-weight: bold;
  font-size: 16px;
  margin-bottom: 4px;
}
.company-introduce {
  color: #4d4d4d;
  font-size: 13px;
}
</style>
