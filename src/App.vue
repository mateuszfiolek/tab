<template>
  <v-layout class="rounded rounded-md">
    <v-app-bar title="Application bar"></v-app-bar>

    <v-navigation-drawer>
      <v-list>
        <v-list-item title="Navigation drawer"></v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-main class="d-flex align-start flex-column mb-6 " >
      <v-table density="compact">
      <thead>
        <tr>
          <th class="text-left">
            file
          </th>
          <th class="text-left">
            licenses
          </th>
          <th class="text-left">
            subdir
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in fact" :key="item.dirname">
          <td>{{ item.dirname }}</td>
          <td>{{ item.licenses }}</td>
          <td v-show=item.isdir>
            <v-col cols="auto">
            <v-btn height="36" min-width="82" @click="fetchData(item.dirname)">
              MORE INF
            </v-btn>
          </v-col>
          </td>
        </tr>
      </tbody>
  </v-table>
  <v-col cols="auto">
    <v-btn
      height="72" min-width="164" @click="fetchData(last)">
        BACK!
    </v-btn>
  </v-col>
    </v-main>
  </v-layout>
</template>


<script>
  export default {
    data () {
      return {
        fact: "",
        last: "",
      }
    },
    methods: {
    fetchData(path) {
      fetch('http://127.0.0.1:5000/oss?path=' + path, {
        method: "GET",
      })
        .then((response) => {
          response.json().then((data) => {
            console.error(data);
            console.error(path);
            this.last = this.removeAfterCharacter(path, "/");
            console.error(this.last);
            this.fact = data;
          });
        })
        .catch((err) => {
          console.error(err);
        });
    },
    removeAfterCharacter(inputString, character) {
      var count = 0;
      for (var i = 0; i < inputString.length; i++) {
        if (inputString.charAt(i) === character) {
            count++;
        }
      }

      if (count <= 1) {
        return '/';
      }

      var index = inputString.lastIndexOf(character);
      if (index !== -1) {
          var result = inputString.substring(0, index);
          return result;
      }
      return inputString;
}

  },
  beforeMount() {
    this.fetchData('/')
    this.last = '/'
  },
  }
</script>

<style>
button {
padding: 12px 32px;
font-size: 16px;
border-radius: 8px;
}
</style>