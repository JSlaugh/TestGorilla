<template>
  <div>
    <div>
      <h2>Options</h2>
      <div
        v-for="(stateOption, key) in StateOptions"
        :key="key"
        @click="filterLoanOfficer(stateOption)"
        style="display: inline; padding-right: 1rem; text-decoration: underline"
      >
        {{ stateOption }}
      </div>
    </div>
    <div
      v-for="(loanOfficer, key) in loanOfficers.filter(
        (item) => item.state == filterValue
      )"
      :key="key"
    >
      <div>
        <img :src="loanOfficer.imageUrl" :alt="loanOfficer.name" />
        <p>
          {{ loanOfficer.sid }}: {{ loanOfficer.name }}: {{ loanOfficer.city }},
          {{ loanOfficer.state }}
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      loanOfficers: [],
      filterValue: "UT",
      StateOptions: [],
    };
  },
  methods: {
    filterLoanOfficer(filterVal) {
      this.filterValue = filterVal;
    },
    makeStateOptionArray() {
      let arrayTemplate = [];
      this.loanOfficers.forEach((loanOfficer) => {
        arrayTemplate.push(loanOfficer.state);
      });
      this.StateOptions = Array.from(new Set(arrayTemplate));
    },
  },
  async created() {
    var requestOptions = {
      method: "GET",
      redirect: "follow",
    };

    fetch(
      "https://api.simplenexus.com/group_profiles/json/c7598ae3-64c4-4e8e-b625-c73e68444640/loanofficers.json",
      requestOptions
    )
      .then((response) => response.json())
      .then((result) => (this.loanOfficers = result.loanOfficers))
      .then((result) => this.makeStateOptionArray(result.loanOfficers))
      .catch((error) => console.log("error", error));
  },
};
</script>

<style></style>
