<template>
  <Layout>
    <div
      class="site-blocks-cover overlay inner-page hero-mask"
      style="background-image: url(/images/class-samples/IMG_3114.jpg);"
    >
      <div class="container">
        <div class="row align-items-center">
          <div class="col-md-10">
            <span class="sub-text"></span>
            <h1>Our Class Schedule</h1>
          </div>
        </div>
      </div>
    </div>

    <div class="site-section">
      <div class="container pt-4">
        <div class="row">
          <div class="col-12 text-center">
            <span class="sub-title">Summer 2019</span>
            <h2 class="font-weight-bold text-black mb-5">Class Schedule</h2>
          </div>
        </div>

        <div class="row">
          <div class="col-12 text-center">
            <div class='occsn_stack' id='occsn_stack_5301_9938'></div>
          </div>
        </div>
      </div>

      <div class="row">
        <div class="col-12 text-center">
          <span class="sub-title">Testimonials from our</span>
          <h2 class="font-weight-bold text-black mb-5">Happy Students</h2>
        </div>
      </div>
      <div class="nonloop-block-13 owl-carousel owl-loaded owl-drag">
        <div class="owl-stage-outer">
          <div class="owl-content-wrapper">
            <div class="owl-item active" style="width: 380px;">
              <div class="testimony">
                <img src="/images/reannin_sirianni.jpg" alt="Image" class="img-fluid">
                <h3>Lisa C.</h3>
                <span class="sub-title"></span>
                <p>“<em>Stephanie’s class was super relaxed and super fun. We learned new techniques and new products and were able to make 2 beautiful pieces to take home. I loved the instruction and the freedom with which we could pick our own colours and embellishments. I will definitely be back!</em>”</p>
              </div>
            </div>
            <div class="owl-item active" style="width: 380px;">
              <div class="testimony">
                <img src="/images/nellie_thompson.jpg" alt="Image" class="img-fluid">
                <h3>Linda C.</h3>
                <span class="sub-title"></span>
                <p>“<em>Registered in the Inks, Resin, Crystals, Stones and Glitter Art Escape. Stephanie was a fabulous instructor. She was welcoming, patient and inspiring. I was very happy with my painting and the experience. Would absolutely take another Wilder Than the Wind Creations class!</em>”</p>
              </div>
            </div>
            <div class="owl-item active" style="width: 380px;">
              <div class="testimony">
                <img src="/images/julie_d_king.jpg" alt="Image" class="img-fluid">
                <h3>Reannin S.</h3>
                <span class="sub-title"></span>
                <p>“<em>I recently took an art class here. It was ridiculously amazing. I had so much fun! I felt like a little kid creating my artwork. Stephanie is such an awesome teacher she teaches you many practical techniques but also gives you plenty of time to play around make a mess and encourages you to have fun. Stephanie’s energy is fabulous!</em>”</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </Layout>
</template>

<script>
  import ContentBlockLayout from '~/components/layouts/ContentBlockLayout.vue';

  // Import static data
  import ServiceData from '~/data/Services.yml';

  export default {
    components: {
      ContentBlockLayout,
    },
    metaInfo: {
      title: "Classes"
    },
    data() {
      return {
        occasionScript: null,
        activeService: null,
        displayBookingForm: false,
        displayQuestionForm: false
      }
    },
    computed: {
      serviceContent() {
        return ServiceData;
      }
    },
    methods: {
      getServices() {
        return (this.serviceContent.services instanceof Array) ? this.serviceContent.services : [];
      },
      getService(idx) {
        let items = this.serviceContent.services;

        if (items instanceof Array && items.length > idx) {
          return items[idx];
        }

        return null
      },
      setActiveService(idx) {
        this.activeService = this.getService(idx);
        // Hide Booking / Question forms as they are only relevant to the selected class

        if (typeof window !== 'undefined') {
          // TODO: Maybe use some kind of route method?
          window.history.pushState({}, this.activeService.title, `${this.$route.path}?id=${this.activeService.id}`);
          window.setTimeout(() => {
            this.$refs.classDetails.scrollIntoView();
          }, 333);
        }
      },
      unsetActiveService() {
        this.activeService = null;
      },
    },
    mounted() {
      // Loop over scripts and strip any occasion ones, there's no API to relaunch this script
      for (let idx = 0; idx < document.scripts.length; idx++) {
        if (document.scripts[idx].src === 'https://app.getoccasion.com/p/preboot.js') {
          delete document.scripts[idx];
          console.log('deleted occasion script');
          window.location.reload();
        }
      }

      this.occasionScript = document.createElement('script');
      this.occasionScript.setAttribute('src', 'https://app.getoccasion.com/p/preboot.js');
      this.occasionScript.setAttribute('id', 'bc-occasion-calendar-script');
      document.head.appendChild(this.occasionScript);

      // Grab the current path
      const { id } = this.$route.query;

      let service = null;
      let matchedServices = this.serviceContent.services.filter((service) => id === service.id);
      if (matchedServices.length > 0) service = matchedServices[0];

      if (service !== null) this.setActiveService(this.serviceContent.services.indexOf(service));
    },
  }
</script>

<style>
  .class-action-buttons {
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .class-action-buttons > * {
    flex: 1;
  }

  .owl-content-wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  @media screen and (min-width: 768px) {
    .owl-content-wrapper {
      display: flex;
      flex-direction: row;
      align-items: flex-start;
      justify-content: center;
    }
  }

  @media screen and (min-width: 1280px) {
    .class-action-buttons {
      display: block;
    }
  }

  .project-entry {
    text-align: center;
  }

  .project-entry .img-fluid {
    height: 40vh;
  }

  @media screen and (min-width: 1280px) {
    .project-entry .img-fluid {
      height: 50vh;
    }
  }

  .post-entry {
    text-align: center;
  }

  .post-entry .img-fluid {
    height: 30vh;
  }

  @media screen and (min-width: 1280px) {
    .post-entry .img-fluid {
      height: 40vh;
    }
  }
</style>
