<template>
    <div>
      <Header></Header>
      <div id="layoutSidenav">
        <SideBar />
        <div id="layoutSidenav_content">
          <main>
            <div class="container-fluid px-4">
              <h1 class="mt-4">Blog</h1>
              <ol class="breadcrumb mb-4">
                <li class="breadcrumb-item active">Blog</li>
              </ol>
              <div class="row">
                <div class="col-xl-3 col-md-6"></div>
                <div class="col-xl-6 col-md-6">
                  <form @submit.prevent="submitForm">
                    <div class="form-floating mb-3">
                      <input
                        class="form-control"
                        id="inputName"
                        type="text"
                        :placeholder="blogData.title"
                        v-model="blogData.title"
                      />
                      <label for="inputEmail">Title</label>
                    </div>
                    <div class="form-floating mb-3">
                      <input
                        class="form-control"
                        id="inputVenue "
                        type="text"
                        :placeholder="blogData.description"
                        v-model="blogData.description"
                      />
                      <label for="inputPassword">Venue</label>
                    </div>
                    <!-- <div class="form-floating mb-3"> -->
                      <input
                        class="form-control"
                        id="inputFlier "
                        type="file"
                        @change="onFileChanged"
                        :placeholder="blogData.image_path"
                      />
                      <!-- <label for="inputPassword">Flier</label> -->
                    <!-- </div> -->
                    <div
                      class="
                        d-flex
                        align-items-center
                        justify-content-between
                        mt-4
                        mb-0
                      "
                    >
                    <div></div>
                      <button
                        style="margin-right: 10px"
                        class="btn btn-primary"
                      >
                        Add
                      </button>
                    </div>
                  </form>
                </div>
                <div class="col-xl-3 col-md-6"></div>
              </div>
            </div>
          </main>
          <Footer />
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import axios from "axios";
  import Header from "../../components/Band/Header.vue";
  import SideBar from "../../components/Band/SideBar.vue";
  import Footer from "../../components/Band/Footer.vue";
  export default {
    components: {
      Header,
      SideBar,
      Footer,
    },
    data(){
      return{
        userID: null,
        imageFile: null,
        image: null,
        updatePicture: false,
        imagePath: null,
        config:{
          enableTime: true,
          dateFormat: "Y-m-d H:i"
        },

        blogData: null,
      }
    },
    mounted() {
    this.getBlog();
    this.getscript()
  },
    methods:{
      getscript() {
      window.addEventListener("DOMContentLoaded", (event) => {
        const sidebarToggle = document.body.querySelector("#sidebarToggle");
        if (sidebarToggle) {
          sidebarToggle.addEventListener("click", (event) => {
            event.preventDefault();
            document.body.classList.toggle("sb-sidenav-toggled");
            localStorage.setItem(
              "sb|sidebar-toggle",
              document.body.classList.contains("sb-sidenav-toggled")
            );
          });
        }
      });
    },
      async getBlog() {
      this.$Progress.start();

      const id = this.$route.params.id;

      try {
        const response = await axios.get(`/api/blog/get/${id}`);

        this.$Progress.finish();

        this.blogData = response.data;

      } catch (error) {
        this.$Progress.fail();
        console.log(error);
        // this.displayUnauthorized(error);
        // this.status.showErrorPage = true;
        // this.status.loading = false;
      }
    },
      async submitForm() {
        this.$Progress.start();
        this.clicked = true;
        this.userID = this.$store.state.userID
        const formData = new FormData();
        console.log(this.blogData);
        formData.append("userID", this.userID);
        formData.append("title", this.blogData.title);
        formData.append("description", this.blogData.description);
        formData.append("image", this.image);

        const id = this.$route.params.id;
  
        try {
          const response = await axios.post(`/api/blog/update/${id}`, formData, { headers: {'Content-Type': 'multipart/form-data' }});
          this.$Progress.finish();
          this.$router.push("/blog").catch(()=>{});
          
        } catch (error) {
          this.$Progress.fail();
        }
      },
      onFileChanged(event) {
      this.image = event.target.files[0];
      if (event.target.files[0] === undefined) {
        this.$noty.error("Select an image please");
      } else {
        if (this.image.size > 10000000) {
          this.$noty.error("Image size limit is 10MB");
        } else {
          if (
            this.image.type !== "image/jpeg" &&
            this.image.type !== "image/jpg" &&
            this.image.type !== "image/png"
          ) {

            this.$noty.error("Only JPG, JPEG and PNG file types are allowed");
          }
        }
      }
    },
    },
  };
  </script>
  
  <style scoped>
  @import "https://cdn.jsdelivr.net/npm/simple-datatables@latest/dist/style.css";
  @import "../../assets/Assets1/css/styles.css";
  @import "https://use.fontawesome.com/releases/v6.1.0/js/all.js";
  @import "https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js";
  @import "https://cdn.jsdelivr.net/npm/simple-datatables@latest";
  @import "https://cdnjs.cloudflare.com/ajax/libs/Chart.js/2.8.0/Chart.min.js";
  </style>