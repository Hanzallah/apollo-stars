<template>
  <v-container class="my-0">
    <v-card flat>
       <v-text-field v-model="minRange" label="MinRange"></v-text-field>
        <v-text-field v-model="maxRange" label="MaxRange"></v-text-field>
        <v-btn small rounded @click="fetchRange()" outlined class="my-3" color="success">Search</v-btn>
      <v-layout row wrap v-for="instructor in ranges" :key="instructor.email" class="py-2">
        <v-flex xs12 md3>
          <div class="caption grey--text pl-2">Name</div>
          <div> <v-chip color="primary">{{ instructor.name }} {{ instructor.surname }} </v-chip>  </div>
        </v-flex>
        <v-flex xs6 sm3 md3>
          <div class="caption grey--text pl-2">Email</div>
          <div><v-chip color="primary">{{ instructor.email }}</v-chip></div>
        </v-flex>
        <v-flex xs6 sm3 md3>
          <div class="caption grey--text pl-2">Date Joined</div>
          <div><v-chip color="primary">{{ instructor.date_joined }}</v-chip></div>
        </v-flex>
        <v-flex xs6 sm3 md3>
          <div class="caption grey--text pl-2">Salary</div>
          <div><v-chip color="primary">{{ instructor.salary }}$</v-chip></div>
        </v-flex>
      </v-layout>
    </v-card>
  </v-container>
</template>

<script>

export default {
  
data() {
    return {
      ranges: [],
      minRange: "",
      maxRange: "",
    }

    },

  props: {
    rankings: Array
  },


  components: {  },

  methods: {

    fetchRange: async function() {
      const settings = {
        method: "get",
        headers: {
          "content-Type": "application/json"
        }
      };

      var url =
        "http://localhost:8079/general/instructor/salaries/ " + parseInt(this.minRange) + "/" + parseInt(this.maxRange) ;
      const res = await fetch(url, settings)
        .then(response => response.json())
        .then(async function(text) {
          return text;
        })
        .catch(e => {
          return e;
        });

      console.log(res.rows);
      this.ranges = res.rows;
    },


  },

  created: function(){
    this.fetchRange();


  }
};
</script>