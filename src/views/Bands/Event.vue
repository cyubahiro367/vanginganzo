<template>
  <div>
    <Header></Header>
    <div id="layoutSidenav">
      <SideBar />
      <div id="layoutSidenav_content">
        <main>
          <div class="container-fluid px-4">
            <h1 class="mt-4">Event</h1>
            <ol class="breadcrumb mb-4">
              <li class="breadcrumb-item active">Event</li>
            </ol>
            <router-link class="btn btn-secondary active" to="/newEvent"
              >Add New Event</router-link
            >
          </div>
        </main>
        <section class="services" style="margin-top: 10px">
          <div class="container">
            <div class="row">
              <!-- <div > -->
                <div v-for="(data, d) in entertainmentData" :key="d"
                  class="col-md-6 col-lg-3 d-flex align-items-stretch"
                  data-aos="fade-up"
                >
                  <div >
                    <!-- <div class="icon"><i class="bx bxl-dribbble"></i></div> -->
                    <div><img :src="urlImage + data.img_path" alt="image"></div>
                    <h4 class="title"><a href="">{{data.name}}</a></h4>
                    
                    <button style="float: left;" class="btn btn-primary" @click="updateEvent(data.id)">
                          Edit
                        </button>

                        <button style="float: right;" class="btn btn-danger active" @click="deleteEvent(data.id)">
                          Delete
                        </button>
                  </div>
                </div>
              <!-- </div> -->

              <!-- <div
                class="col-md-6 col-lg-3 d-flex align-items-stretch"
                data-aos="fade-up"
                data-aos-delay="100"
              >
                <div class="icon-box icon-box-cyan">
                  <div class="icon"><i class="bx bx-file"></i></div>
                  <h4 class="title"><a href="">Sed ut perspiciatis</a></h4>
                  <p class="description">
                    Duis aute irure dolor in reprehenderit in voluptate velit
                    esse cillum dolore eu fugiat nulla pariatur
                  </p>
                </div>
              </div>

              <div
                class="col-md-6 col-lg-3 d-flex align-items-stretch"
                data-aos="fade-up"
                data-aos-delay="200"
              >
                <div class="icon-box icon-box-green">
                  <div class="icon"><i class="bx bx-tachometer"></i></div>
                  <h4 class="title"><a href="">Magni Dolores</a></h4>
                  <p class="description">
                    Excepteur sint occaecat cupidatat non proident, sunt in
                    culpa qui officia deserunt mollit anim id est laborum
                  </p>
                </div>
              </div>

              <div
                class="col-md-6 col-lg-3 d-flex align-items-stretch"
                data-aos="fade-up"
                data-aos-delay="200"
              >
                <div class="icon-box icon-box-blue">
                  <div class="icon"><i class="bx bx-world"></i></div>
                  <h4 class="title"><a href="">Nemo Enim</a></h4>
                  <p class="description">
                    At vero eos et accusamus et iusto odio dignissimos ducimus
                    qui blanditiis praesentium voluptatum deleniti atque
                  </p>
                </div>
              </div> -->
            </div>
          </div>
        </section>
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
  data() {
    return {
      entertainmentData: [],
      urlImage: null,
    };
  },
  components: {
    Header,
    SideBar,
    Footer,
  },
  methods: {
    async submitForm() {
      this.$Progress.start();

      try {
        const response = await axios.get("/api/entertainments/get");
        this.$Progress.finish();

        this.urlImage = localStorage.getItem('urlPath') + "/" ;

        response.data.forEach((el) => {
          this.entertainmentData.push({
            id: el.id,
            name: el.name,
            venue: el.venue,
            startTime: el.startTime,
            endTime: el.endTime,
            img_path: el.img_path,
          });
        });

        console.log(this.entertainmentData);
      } catch (error) {
        this.$Progress.fail();
        this.clicked = false;
      }
    },
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
    async updateEvent(id) {
      this.$Progress.start()
      this.$router.push(`/event/${id}`).catch(()=>{});
      this.$Progress.finish()
    },
    async deleteEvent(id) {
      this.$Progress.start()
     
      try {
        await axios.delete(`/api/entertainments/delete/${id}`);
        this.$Progress.finish()
        this.$noty.success("event deleted");
        this.$router.push("/event").catch(()=>{});
      } catch (error) {
        this.$Progress.fail()
        this.displayUnauthorized(error);
        
      }
    },
  },
  mounted() {
    this.submitForm();
    this.getscript()
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