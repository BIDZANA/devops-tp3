<template>
  <div class="hello">
    <nav>
      <router-link to="/">Accueil</router-link>
      <router-link to="/department-list">Liste des départements</router-link>
      <router-link to="/department/:name">Détails du département</router-link>
    </nav>
    <DepartmentList />
  </div>
</template>


<script>
import DepartmentList from './DepartmentList';

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  components: { DepartmentList }
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
  text-decoration: none;
  font-weight: bold;
}
a:hover {
  color: #2c3e50;
}
</style>