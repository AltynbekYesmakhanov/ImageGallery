<template>
  <div>
    <div class="sort-container">
      <select v-model="filterByAuthor" @change="filterImages">
        <option value="">All Authors</option>
        <option v-for="author in uniqueAuthors" :key="author">{{ author }}</option>
      </select>
    </div>

    <div class="images-container">
      <div 
        v-for="image in filteredImages" 
        :key="image.id" 
        @click="toggleImage(image.id)"
        class="image-container"
      >
        <img
          :src="image.url"
          :class="{ 'zoomed': selectedImage === image.id }"
          alt="Gallery Thumbnail"
        />
        <p>{{ image.title }} by {{ image.author }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      images: [
        { id: 1, url: require('@/assets/1.jpeg'), title: 'Mona Lisa', author: 'Leonardo Da Vinci' },
        { id: 2, url: require('@/assets/2.jpeg'), title: 'The Last Supper', author: 'Leonardo Da Vinci' },
        { id: 3, url: require('@/assets/3.jpeg'), title: 'The Starry Night', author: 'Vincent van Gogh' },
        { id: 4, url: require('@/assets/4.jpeg'), title: 'The Scream', author: 'Vincent van Gogh' },
        { id: 5, url: require('@/assets/5.jpeg'), title: 'Guernica', author: 'Pablo Picasso' },
      ],
      selectedImage: null,
      filterByAuthor: "",
    }
  },
  computed: {
    filteredImages() {
      if (!this.filterByAuthor) {
        return this.images;
      }
      return this.images.filter(img => img.author === this.filterByAuthor);
    },
    uniqueAuthors() {
      return [...new Set(this.images.map(img => img.author))];
    }
  },
  methods: {
    toggleImage(imageId) {
      if (this.selectedImage === imageId) {
      this.selectedImage = null;
      document.body.classList.remove('zoomed-bg'); 
    } else {
      this.selectedImage = imageId;
      document.body.classList.add('zoomed-bg'); 
    }
    }
  }
}
</script>

<style scoped>
body {
  font-family: 'Arial', sans-serif;
}

body.zoomed-bg .image-container img:not(.zoomed) {
  opacity: 0.3;
}

body.zoomed-bg::before {
  content: '';
  display: block;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.7);
  z-index: 4; 
}
.sort-container {
  position: fixed; 
  top: 20px;
  left: 20px;
  z-index: 4;
}
.sort-container select {
  padding: 10px;      
  background-color: #fff;  
  border: 2px solid #ccc;  
  border-radius: 5px;  
  font-size: 16px;     
  outline: none;       
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.images-container {
  padding-top: 60px; 
}

.image-container {
  display: inline-block;
  text-align: center;
  margin: 15px;
  position: relative;
  z-index: 2;
}

.image-container img {
  width: 200px;
  height: auto;
  margin: 0;
  cursor: pointer;
  transition: transform 0.3s, opacity 0.3s;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  z-index: 2;
  opacity: 0.5;
  transition: opacity 0.3s;
}

.image-container img.zoomed {
  transform: scale(2) translate(-50%, -50%);
  z-index: 5;
  position: fixed;
  top: 50%;
  left: 50%;
  opacity: 1;
}


.image-container:hover img {
  opacity: 1;
}
img.zoomed {
  transform: scale(2);
  z-index: 3; 
  position: relative;
}
.image-container p {
  margin-top: 10px;
  font-weight: bold;
  color: #333;
}
</style>