<template>
  <div class="hello">
    <form @submit.prevent="insert">
      <p><input type="text" name="name" v-model="name"/></p>
      <p><input type="submit" value="저장"/></p>
    </form>

    <ul>
      <li v-for="item in data" :key="item.date">
        {{ item.name }}  [♥{{ item.count }}] <!-- item.count: 좋아요 횟수 -->
        <button type="button" @click="update(item.date, item.count)">좋아용</button>
        <button type="button" @click="del(item.date)">삭제</button>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'HelloWorld',
  mounted(){
    this.select();
  },
  data(){
    return{ name:'', data: [] }
  },
  methods:{
    select(){
      axios.get('http://localhost:3000/api')
      .then((res) => {
        this.data = res.data;
      })
    },

    insert(){
      const data = {name: this.name, date: Date.now(), count: 0};
      axios.post("http://localhost:3000/api/insert", data)
      .then(res => {
        this.data = res.data;
      })
    },

    del(date){
      axios.delete(`http://localhost:3000/api/delete?date=${date}`)
      .then(res => {
        this.data = res.data;
      })
    },

    update(date, count){
      axios.put(`http://localhost:3000/api/update?date=${date}`, {count: count+1})
      .then(res => {
        this.data = res.data;
      })

    },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  // list-style-type: none;
  padding: 0;
}
li {
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
