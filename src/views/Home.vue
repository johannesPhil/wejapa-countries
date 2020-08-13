<template>
  <div class="home">
    <!-- <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <div class="topbar">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        version="1.1"
        width="65"
        height="65"
        style="
          shape-rendering: geometricPrecision;
          text-rendering: geometricPrecision;
          image-rendering: optimizeQuality;
          fill-rule: evenodd;
          clip-rule: evenodd;
        "
        viewBox="0 0 444 282"
        xmlns:xlink="http://www.w3.org/1999/xlink"
      >
        <g id="Layer_x0020_1">
          <metadata id="CorelCorpID_0Corel-Layer"></metadata>
          <polygon
            style="fill: #ffcc00;"
            points="0,57 102,248 103,248 107,253 108,252 155,220 69,55 "
          ></polygon>
          <path
            style="fill: #19bf69;"
            d="M223 56c2,-5 5,-10 8,-14l0 0 0 0c17,-27 38,-41 62,-42l100 0c45,0 65,31 40,83 -20,41 -52,110 -82,157 -5,7 -10,17 -15,23 -53,60 -100,-42 -114,-75l-26 51c-22,41 -60,65 -93,9 0,0 0,0 0,0 0,0 0,0 -1,0 25,0 60,-78 87,-123l-61 -125 68 0c9,19 18,37 27,56l0 0zm76 149l77 -143 -66 1c-7,0 -18,0 -24,9 -9,17 -22,41 -29,54l-1 -2 0 0c14,28 29,54 43,81z"
          ></path>
        </g>
      </svg>
      <h2>WeJapa Countries</h2>
    </div>
    <div class="content">
      <div class="form-container">
        <form class="form">
          <div class="form__control">
            <label for="country" class="form__el">Select Country</label>
            <select
              name="country"
              id=""
              class="form__el"
              @change="selectCountry($event)"
            >
              <option value="">Country</option>
              <option
                :value="country.name"
                v-for="country in countryList"
                :key="country.id"
                >{{ country.name }}</option
              >
            </select>
          </div>
          <div class="form__control">
            <label for="state" class="form__el">Select State</label>
            <select
              name="state"
              id=""
              class="form__el"
              @change="selectState($event)"
            >
              <option
                :value="state.name"
                v-for="state in stateList"
                :key="state.id"
                >{{ state.name }}</option
              >
              <option value="" v-if="nullStates">No known State(s)</option>
            </select>
          </div>
          <div class="form__control">
            <label for="city" class="form__el">Select City</label>
            <select name="city" id="" class="form__el">
              <option
                :value="city.name"
                v-for="city in cityList"
                :key="city.id"
                >{{ city.name }}</option
              >
              <option value="" v-if="nullCities">No Known City</option>
            </select>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
/* eslint-disable */

export default {
  name: "Home",
  components: {},
  data() {
    return {
      country: "",
      state: "",
      city: "",
      countryList: [],
      stateList: [],
      cityList: [],
      nullStates: false,
      nullCities: false
    };
  },
  created() {
    fetch(
      "https://raw.githubusercontent.com/dr5hn/countries-states-cities-database/master/countries%2Bstates%2Bcities.json"
    )
      .then((response) => {
        return response.json();
      })
      .then((data) => {
        this.countryList = data;

        // data.forEach((country) => {
        //   this.stateList = country.states;
        //   // console.log(country.states);
        //   this.stateList.forEach((state) => {
        //     console.log(state.cities);
        //   });
        // });
      });
  },
  methods: {
    selectCountry(e) {
      // get the curent value (country) of the select element based on the selected option

      const countryValue = e.target.value;

      // find the index value of the country gotten from above

      const country = this.countryList.findIndex(
        (country) => country.name === countryValue
      );

      //find the states for the country that has the index value gotten above

      const state = this.countryList[country]["states"];

      if (state.length === 0) {
        this.nullStates = !this.nullStates;
      }

      this.stateList = state;
    },
    selectState(e) {
      // get the curent value (state) of the select element based on the selected option

      const stateValue = e.target.value;

      // find the index value of the state gotten from above

      const state = this.stateList.findIndex(
        (state) => state.name === stateValue
      );

      //find the states for the state that has the index value gotten above

      const city = this.stateList[state]["cities"];
      if (city.length === 0) {
        this.nullCities = !nullCities;
      }

      this.cityList = city;
    }
  }
};
</script>
<style scoped>
.topbar {
  background: #000;
  height: 10vh;
  display: flex;
  align-items: flex-start;
  padding-left: 2%;
  color: #fff;
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  align-items: center;
}

.topbar h2 {
  margin-left: 20px;
}

.content {
  width: 100%;
  height: 80vh;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
}

.form-container {
  max-width: 800px;
  height: auto;
  margin: auto;
  padding: 0 2%;

  /* display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center; */
}
.form {
  width: 100%;
}

.form__control {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  margin: 1rem 0;
}

label.form__el {
  display: block;
  text-align: left;
  margin: 2%;
}
select.form__el {
  width: 100%;
  height: 2.3rem;
  background: #fff;
  border: solid 1px #e2e2e2;
  border-radius: 5px;
  padding: 2% 0;
}
</style>
