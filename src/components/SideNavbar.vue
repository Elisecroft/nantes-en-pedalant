<template>
  <div id="allSide">
    <span style="font-size:30px;cursor:pointer" id="menu" v-on:click="openNav()">&#10010;</span>
    <div id="mySidenav" class="sidenav">
      <a href="javascript:void(0)" class="closebtn" v-on:click="closeNav()">&times;</a>
      <h2>Nantes en pédalant</h2>
      
      <h3>Conseils du jour</h3>
      <ul>
        <li>N'oubliez pas votre casque</li>
        <li>Couvrez-vous, il fait froid dehors</li>
        <li>Passez de bonnes fêtes de fin d'années !</li>
      </ul>
      <h3>Liens utiles</h3>
      <a href="https://www.nantes.fr/eau" target="_blank">Points d'eau</a>
      <a href="https://www.bicloo.nantesmetropole.fr/home" target="_blank">Bicloos</a>
      <a href="https://data.nantesmetropole.fr/pages/home/" target="_blank">Données utilisée</a>
      <a href="https://github.com/Elisecroft/nantes-en-pedalant" target="_blank">Lien GitHub</a>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'SideNavbar',
    methods: {
        openNav() {
          document.getElementById("mySidenav").style.width = "250px";
        },
        closeNav() {
          document.getElementById("mySidenav").style.width = "0";
        }
    }
  }
</script>

<style>
  #allSide {
    position: fixed;
    z-index: 1000;
  }

  #menu {
    position: fixed;
    z-index: 1;
    top: 90px;
    left: 10px;
    color: rgb(253, 154, 2);
    background-color: #222;
    padding: 0.25% 0.5%;
    border-radius: 50%;
  }

  .sidenav {
    height: 100%;
    width: 0;
    position: fixed;
    z-index: 2;
    top: 0;
    left: 0;
    background-color: #222;
    overflow-x: hidden;
    transition: 0.5s;
    padding-top: 60px;
  }

  .sidenav a {
    padding: 8px 8px 8px 8px;
    text-decoration: none;
    font-size: 22px;
    color: #bbb;
    display: block;
    transition: 0.3s;
  }

  .sidenav a:hover {
    color: #f1f1f1;
  }

  .sidenav .closebtn {
    position: absolute;
    top: 0;
    right: 25px;
    font-size: 36px;
    margin-left: 50px;
  }

  h2, h3 {
    color: rgb(253, 154, 2);
  }

  h2 {
    margin-top: 20%;
    font-size: 35px;
  }

  h3 {
    font-size: 25px;
  }

  li {
    padding: 8px 8px 8px 8px;
    margin-right: 10px;
    font-size: 22px;
    color: #bbb;
  }

  @media screen and (max-height: 450px) {
    .sidenav {padding-top: 15px;}
    .sidenav a {font-size: 18px;}
  }
</style>