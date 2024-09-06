<script setup>
import axios from "axios";
import { ref, watch } from "vue";
import sweetModal from "sweetalert2"; 

const catImages = ref([]); 
const page = ref(1);
const accessToken = 'live_tdBiDoCrReqHMuGcEg6BypOtO8e7n5r97D9QSM9LtuIcPeftZeGhQj3KMEPGkIt6';

const fetchData = async () => {
  try {
       const response = await axios.get(`https://api.thecatapi.com/v1/images/search?limit=10&page=${page.value}`);
    catImages.value = response.data;
    console.log(response);
  } catch (error) {
    console.error('Error fetching data:', error);
  }
};

fetchData();

watch(page, async () => {
  fetchData();
});




const showCatInfo = (cat) => {

  sweetModal.fire({
    title: 'Cat Information',
    text: `Height: ${cat.height} cm, Width: ${cat.width} cm`, //walang cat.name according kay console
    imageUrl: cat.url,
    imageWidth: 400,
    imageHeight: 400,
    imageAlt: `Cat image ${cat.id}`,
  });
};

</script>


<template>
  <main class="cat-gallery">
    <h1>Cat Images</h1>
    <div class="cat-grid"> 
      <div
        v-for="image in catImages"
        :key="image.id"
        class="cat-image"
        @click="showCatInfo(image)" 
        
      >
        <img :src="image.url" />
      </div>
    </div>
    <div class="pagination">
      <button
        @click="page--"
        :disabled="page === 1"
        class="nav-button"
      >
        Previous
      </button>
      <button
        @click="page++"
        :disabled="page === maxPages"
        class="nav-button"
      >
        Next
      </button>
    </div>
  </main>
</template>
<style scoped>

h1{
  text-align: center;
  font-size: 48px;
  font-weight:bolder;
  font-family:'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
  
}
.cat-gallery {
  padding: 20px;
  width: 1500px;
  margin-left: -100px;
}

.cat-grid {
  display: grid; 
  grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
  gap: 20px; 
}

.cat-image {
  display: inline-block; 
  width: 300px;
  height: 250px; 
  border: 5px solid white; 
  border-radius: 10px; 
  padding: 10px; 
  box-shadow: 0 4px 12px 0 rgba(0, 0, 0, 0.85); 
  transition: transform 0.3s, box-shadow 0.3s; 
  margin: 10px; 
  overflow: hidden; 
}

.cat-image img {
  display: block; 
  width: 100%; 
  height: 100%; 
  object-fit: cover;
  border-radius: 10px; 
}

.cat-image:hover {
  transform: scale(1.05);
  box-shadow: 0 8px 20px 0 rgba(0, 0, 0, 0.2); 
}

.pagination {
  margin-top: 20px;
  text-align: center; 
}

.nav-button {
  background-color: green; 
  color: white; 
  border: none; 
  padding: 10px 20px; 
  border-radius: 5px; 
  cursor: pointer; 
  transition: background-color 0.3s, transform 0.3s;
  margin-left:5px;
}

.nav-button:disabled {
  background-color: #ccc; 
  cursor: not-allowed; 
}

.nav-button:hover:not(:disabled) {
  background-color: lightgreen; 
  transform: scale(1.05); 
}

.nav-button:focus {
  outline: none; 
  box-shadow: 0 0 0 3px rgba(25, 118, 210, 0.5); 
}
</style>