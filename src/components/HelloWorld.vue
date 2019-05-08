<template>
  <div>
    <div>
      <h1>Vue js syntax</h1>
      <p>Using mustaches: {{ rawHtml }}</p>
      <p>
        Using v-html directive:
        <span v-html="rawHtml"></span>
      </p>
      <a v-bind:href="url">link1</a>
      <br>
      <br>
      <a :href="url">link2</a>
      <br>
      <br>
      <form v-on:submit.prevent="onSubmit(a,b)">
        <input type="text" v-model="a">
        <input type="text" v-model="b">
        <button>submit</button>
      </form>
      <br>
      <input type="text" v-model="c">
      <input type="text" v-model="d">
      <button v-on:click="onSubmit(c,d)">submit</button>
    </div>
    <hr>
    <div>
      <h1>Computed Properties and Watchers</h1>
      <div>
        <p>Computed</p>
        <input type="text" v-model="num">
        <p v-if="num != 0">calculated : {{cpCaculate}}</p>
        <p>Watched Property</p>
        <input type="text" v-model="question">
        <p>new : {{questions[0]}}</p>
        <p>old : {{questions[1]}}</p>
        <p></p>
      </div>
    </div>
    <hr>
    <div>
      <h1>List Rendering</h1>

      <p>Mapping an Array to Elements with v-for</p>
      <ul>
        <li v-for="item in items">{{ item.message }}</li>
      </ul>
      <ul>
        <li v-for="(item, index) in items">{{ parentMessage }} - {{ index }} - {{ item.message }}</li>
      </ul>

      <p>v-for with an Object</p>
      <ul>
        <li v-for="value in object">{{ value }}</li>
      </ul>
    </div>
    <hr>
    <div>
      <h1>get Api</h1>
      <ul>
        <li v-for="items in resultbyID">
          <p>{{items.title}}</p>
        </li>
      </ul>
    </div>
    <hr>
    <div>
      <h1>Load more</h1>
      <table class="tables">
        <tr>
          <th>id</th>
          <th>name</th>
        </tr>
        <tr v-for="itemsview in ItemListView">
          <td>{{itemsview.Id}}</td>
          <td>{{itemsview.names}}</td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      a: "",
      b: "",
      c: "",
      d: "",
      url: "https://google.com",
      rawHtml: "<span style='color:red'>my name is</span>",
      num: 0,
      question: "",
      questions: [],
      parentMessage: "Parent",
      items: [{ message: "Foo" }, { message: "Bar" }],
      object: {
        title: "How to do lists in Vue",
        author: "Jane Doe",
        publishedAt: "2016-04-10"
      },
      resultbyID: [],
      ItemListView: []
    };
  },
  methods: {
    onSubmit: function(a, b) {
      console.log(a + b);
    },
    onCalculate: function() {
      console.log(this.num);
    },
    getData: async function() {
      const res = await fetch("https://jsonplaceholder.typicode.com/posts/", {
        headers: {
          "Content-Type": "application/json",
          Accept: "application/json"
        },
        method: "GET"
      });
      console.log(res);
      const result = await res.json();
      console.log(result);
    },
    getDatabyId: async function(param) {
      var url = new URL("https://jsonplaceholder.typicode.com/posts");
      Object.keys(param).forEach(key =>
        url.searchParams.append(key, param[key])
      );
      //console.log(url);
      const res = await fetch(url, {
        headers: {
          "Content-Type": "application/json",
          Accept: "application/json"
        },
        method: "GET"
      });
      //console.log(res);
      this.resultbyID = await res.json();
      console.log(this.resultbyID);
    },
    posData: async function() {
      const res = await fetch("https://jsonplaceholder.typicode.com/posts", {
        headers: {
          "Content-Type": "application/json",
          Accept: "application/json"
        },
        method: "POST",
        body: JSON.stringify({ userId: 1 })
      });
      console.log(res);
      const result = await res.json();
      console.log(result);
    },
    Items: function() {
      for (var i = 0; i < 10; i++) {
        this.ItemListView.push({ Id: i, names: "item" + i });
      }
      console.log(JSON.stringify(this.ItemListView))
    }
  },
  computed: {
    cpCaculate: function() {
      return this.num * 5;
    }
  },
  watch: {
    question: function(newquestion, oldquestion) {
      return (this.questions = [newquestion, oldquestion]);
    },
    cpCaculate: function(d) {
      console.log(d);
    }
  },
  created() {
    //this.getData();
    this.getDatabyId({
      userId: 1
    });
    //this.posData();

    this.Items();
  }
};
</script>

<style>
.tables{
  width: 100%;
  height: 100px;
  text-align: center
}
</style>
