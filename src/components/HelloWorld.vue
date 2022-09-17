<template>
    <div class="hello">
        <h1>{{ msg }}</h1>
        <input type="text" placeholder="인원 수 입력" v-model="all"/>
		<input class="input is-success" type="text" placeholder="Success input"/>
    </div>
</template>

<script>
export default {
	name: 'HelloWorld',
	props: {
		msg: String
	},
	data(){
		return {
			all : 0
		}
	},
	methods :{
		push(){
			alert(this.all)
		}
	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
