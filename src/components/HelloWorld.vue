<template>
    <div class="hello">
        <h1>{{ msg }}</h1>
        <div @click="push()">인원추가 test</div>
    </div>
</template>

<script>
export default {
	name: 'HelloWorld',
	props: {
		msg: String
	},
	methods :{
		push(){
			alert("추가했습니다.")
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
