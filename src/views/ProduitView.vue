<template>
  <main>
    <div> Liste des produits - {{data.page.number+1}}/{{data.page.totalPages}}
    </div>
    <div>
      <table>
        <caption>Liste des catégories</caption>
        <tr>
          <th>Nom</th>
          <th>Prix</th>
          <th>Stock</th>
          <th>Unitées commandées</th>
        </tr>
        <!-- Si le tableau des produits est vide -->
        <tr v-if="!data.listeProduits">
          <td colspan="4">Veuillez patienter, chargement des catégories...</td>
        </tr>
        <!-- Si le tableau des produits n'est pas vide -->
        <tr v-for="produit in data.listeProduits" :key="produit.code">
          <td>{{ produit.nom }}</td>
          <td>{{ produit.prixUnitaire }}</td>
          <td>{{ produit.unitesEnStock }}</td>
          <td>{{ produit.unitesCommandees }}</td>
        </tr>

        <tr>
          <td><button @click="chargeProduitsPourNavigationFirst">Prem PAGE</button></td>
          <td><button v-if="data.links.prev!=null" @click="chargeProduitsPourNavigationPrevious">PREC</button></td>
          <td><button v-if="data.links.next!=null" @click="chargeProduitsPourNavigationNext">SUIV</button></td>
          <td><button @click="chargeProduitsPourNavigationLast">Dern PAGE</button></td>

        </tr>

      </table>
    </div>
  </main>
</template>

<script setup>
import {reactive, onMounted, ref} from "vue";
import { BACKEND, doAjaxRequest } from "../api";

// Pour réinitialiser le formuaire

const fetchOptions = {method:'GET'}

let data = reactive({

  // La liste des produits affichés sous forme de table
  listeProduits: [],
  links: {},
  page: {}
});

let nombreDePages= ref(0)
let pageActuelle=ref(0)


function chargeProduits() {
  // Appel à l'API pour avoir la liste des produits
  // Trié par code, descendant
  // Verbe HTTP GET par défaut
  doAjaxRequest(BACKEND + "/api/produits?page=0&size=7")
      .then((json) => {
        data.listeProduits = json._embedded.produits;
        data.links = json._links;
        data.page=json.page;
      })
      .catch((error) => alert(error.message));
}

function chargeProduitsPourNavigationFirst() {

  // Appel à l'API pour avoir la liste des produits
  // Trié par code, descendant
  // Verbe HTTP GET par défaut
  doAjaxRequest(data.links.first.href, fetchOptions)
      .then((json) => {
        data.listeProduits = json._embedded.produits;
        data.links = json._links;
        data.page=json.page;
      })
      .catch((error) => alert(error.message));
}
function chargeProduitsPourNavigationPrevious() {

  // Appel à l'API pour avoir la liste des produits
  // Trié par code, descendant
  // Verbe HTTP GET par défaut
  doAjaxRequest(data.links.prev.href , fetchOptions)
      .then((json) => {
        data.listeProduits = json._embedded.produits;
        data.links = json._links;
        data.page=json.page;
      })
      .catch((error) => alert(error.message));
}
function chargeProduitsPourNavigationNext() {

  // Appel à l'API pour avoir la liste des produits
  // Trié par code, descendant
  // Verbe HTTP GET par défaut
  doAjaxRequest(data.links.next.href, fetchOptions)
      .then((json) => {
        data.listeProduits = json._embedded.produits;
        data.links = json._links;
        data.page=json.page;
      })
      .catch((error) => alert(error.message));
}
function chargeProduitsPourNavigationLast() {

  // Appel à l'API pour avoir la liste des produits
  // Trié par code, descendant
  // Verbe HTTP GET par défaut
  doAjaxRequest(data.links.last.href, fetchOptions)
      .then((json) => {
        data.listeProduits = json._embedded.produits;
        data.links = json._links;
        data.page=json.page;
      })
      .catch((error) => alert(error.message));
}


/*function ajouteCategorie() {
  // Ajouter une catégorie avec les données du formulaire
  const options = {
    method: "POST", // Verbe HTTP POST pour ajouter un enregistrement
    body: JSON.stringify(data.formulaireCategorie),
    headers: {
      "Content-Type": "application/json",
    },
  };
  doAjaxRequest(BACKEND + "/api/categories", options)
      .then(() => {
        // Réinitialiser le formulaire
        data.formulaireCategorie = { ...categorieVide };
        // Recharger la liste des catégories
        chargeProduits();
      })
      .catch((error) => alert(error.message));
}*/

/*function deleteEntity(entityRef) {
  doAjaxRequest(entityRef, { method: "DELETE" })
      .then(chargeProduits)
      .catch((error) => alert(error.message));
}*/

// A l'affichage du composant, on affiche la liste
onMounted(chargeProduits);

</script>


<style scoped>
td,
th {
  border: 1px solid #ddd;
  padding: 8px;
}


th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
  background-color: #232623;
  color: rgb(255, 255, 255);
}
</style>