<template>
  <section>

  <!-- Don't delete this please -->
    <div class="b-example-divider"></div>

    <div class="mt-5 trips shadow-lg">
      <div class="p-3 text-center bg-body-tertiary">
        <div class="container py-5">
          <h2 class="info">
            <img alt="jTDataWorks" height="42" width="42"
              src="./../assets/images/logo.webp" 
              class="img-fluid logo mr-1">       
              <span class="success">JT</span>Dataworks
          </h2>
          <h3 class="text-body-secondary">The modern world runs on data.</h3>
          <p class="col-lg-8 mx-auto">
            JTDataworks is a data management platform that allows you to
            take your data and transform it into your information - available to you via your own secure 
            server. No Big Tech.  No Big Brother.  
            Just what's important to you, on your terms.
          </p>
        </div>
      </div>
    </div>

    <div class="container mt-5">
      <BarChart />
    </div>

    <div class="container" id="custom-cards">

      <div class="row row-cols-1 row-cols-lg-3 align-items-stretch g-4 py-5">

        <div class="col mb-5">
          <div class="card card-cover h-100 overflow-hidden text-bg-dark rounded-4 shadow-lg trips">
            <div class="d-flex flex-column h-100 p-5 pb-3 text-white text-shadow-1">
              <h4 class="mb-2 display-6">
                Database Backup
              </h4>
              <p>How safe is your data at rest?</p>
              <ul class="d-flex list-unstyled mt-auto">
                <li class="me-auto">
                  <img src="http://localhost:8081/static/kay-so.png" alt="Bootstrap" width="32" height="32"
                    class="rounded-circle border border-black">
                </li>
                <li class="d-flex align-items-center me-3">
                  <svg class="bi me-2" width="1em" height="1em">
                    <use xlink:href="#geo-fill" />
                  </svg>
                  <small>:Kay-So</small>
                </li>
                <li class="d-flex align-items-center">
                  <svg class="bi me-2" width="1em" height="1em">
                    <use xlink:href="#calendar3" />
                  </svg>
                  <small class="success">Florida, USA</small>
                </li>
              </ul>
            </div>
          </div>
        </div>

        <div class="col mb-5">

          <div class="card card-cover h-100 overflow-hidden text-bg-dark rounded-4 shadow-lg trips">
            <div class="d-flex flex-column h-100 p-5 text-white text-shadow-1">
              <h4 class="mb-2 display-6">Distributed DB</h4>
              <p>Where does your data exist?</p>
              <ul class="d-flex list-unstyled mt-auto">
                <li class="me-auto">
                  <img src="http://localhost:8081/static/frog.jpg" alt="Bootstrap" width="42" height="42"
                    class="rounded-circle border border-black">
                </li>
                <li class="d-flex align-items-center me-3">
                  <svg class="bi me-2" width="1em" height="1em">
                    <use xlink:href="#geo-fill" />
                  </svg>
                  <small>:Trip</small>
                </li>
                <li class="d-flex align-items-center">
                  <svg class="bi me-2" width="1em" height="1em">
                    <use xlink:href="#calendar3" />
                  </svg>
                  <small class="success">Florida, USA</small>
                </li>
              </ul>
            </div>
          </div>

        </div>

        <div class="col mb-5">
          <div class="card card-cover h-100 overflow-hidden text-bg-dark rounded-4 shadow-lg trips">
            <div class="d-flex flex-column h-100 p-5 pb-3 text-white text-shadow-1">
              <h4 class="mb-2 display-6">Data as a Service</h4>
              <p>Your secured data on demand.</p>
              <ul class="d-flex list-unstyled mt-auto">
                <li class="me-auto">
                  <img src="http://localhost:8081/static/kay-so.png" alt="Bootstrap" width="32" height="32"
                    class="rounded-circle border border-black">
                </li>
                <li class="d-flex align-items-center me-3">
                  <svg class="bi me-2" width="1em" height="1em">
                    <use xlink:href="#geo-fill" />
                  </svg>
                  <small>:Kay-So</small>
                </li>
                <li class="d-flex align-items-center">
                  <svg class="bi me-2" width="1em" height="1em">
                    <use xlink:href="#calendar3" />
                  </svg>
                  <small class="success">Florida, USA</small>
                </li>
              </ul>
            </div>
          </div>
        </div>

      </div>

    </div>

  </section>
</template>

<script>
import BarChart from './charts/BarChart.vue'

export default {
  name: 'HomePage',

  data() {
    return {
      // products: []
    }
  },
  components: { BarChart },

}

</script>