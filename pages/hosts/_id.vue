<template>
  <main>
    <div class="cards">

        <div class="form-section">
          <h1 class="form-heading">Update Your Airbnb Host</h1>
          <h2 class="form-subheading">Make adjustments about your place</h2>
          <input v-model="title" class="location" placeholder="Title"/>
          <select  v-model="category" class="location" name="Category" >
            <option value="" disabled selected>Category</option>
            <option  v-for="category in categories" :key="category"  :value="category">{{category}}</option>
          </select>
          <input v-model="owner" class="location" placeholder="Owner Name"/>
          <textarea v-model="address" class="location" id="address" placeholder="Address"/>            
          <input v-model="country" class="location" placeholder="Country" />  
          <input v-model="roomNumber" class="location" type="number" placeholder="Room Number"/>
          <input v-model="price" class="location" type="number" placeholder="Price"/>          
        </div>

        <div class="form-section">  
          <h2>Features</h2>
          <div v-for="(feature,index) in features" :key="index"  >
            <input :id="index" class="toggle" v-model="checkedFeatures" type="checkbox" :value="feature"> 
            <label :for="index" class="feature-options"> {{feature}} </label>    
          </div>
  
          <h2 class="photo-title">Host Photo</h2>
          <label for="file-upload" class="custom-file-upload">
            Upload Your Host Photo
          </label>
          <input id="file-upload" type="file" @change="onFileSelected"/>

          <button class="upload-host" @click="onUploadHost">Become a Host</button>

        </div>


    </div>     

  </main>  
</template>


<script>
export default {
  data() {
    return {
      categories: ["House","Bungalov","TreeHouses","Island","North Pole","Cave","Castle"],
      features: ["Wifi Connection","Jakuzi","Pool","Camelia"],
      checkedFeatures: [],
      title: "",
      category: "",
      owner: "",
      roomNumber: 0,
      price: 0,
      address: "",
      country: "",
      selectedFile: null,
      fileName: ""
    }
  },

  methods: {
    onFileSelected(e) {
      this.selectedFile = e.target.files[0];
      console.log(this.selectedFile);
      this.fileName = e.target.files[0].name;
    },
    async onUploadHost() {
      let data = new FormData();
      data.append("title", this.title);
      data.append("category", this.category);
      data.append("owner", this.owner);
      data.append("address", this.address);      
      data.append("country", this.country);  
      data.append("roomNumber", this.roomNumber);   
      data.append("features", this.checkedFeatures); 
      data.append("photo", this.selectedFile, this.selectedFile.name);
      data.append("price", this.price);

      let result = await this.$axios.$put(
        `http://localhost:3000/api/hosts/${this.$route.params.id}`,
        data
      );

      this.$router.push("/");
    }
  }

}
</script>


<style >

  @import '../../static/css/host.css';

</style>