<template>
	<div>		
		<RootHeader/>
		<InitCardList/>
	</div>
</template>

<script>
import InitCardList from './InitCardList.vue'
import RootHeader from './RootHeader.vue'

export default {
	name: 'Root',
	components: {
		InitCardList,
		RootHeader
	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
@import '../sass/variables.scss';
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
