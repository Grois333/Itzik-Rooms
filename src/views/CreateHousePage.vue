<template>
  <page-layout>
    <section class="py-4 bg-teal-dark">
      <div class="container">
        <form class="form">
          <div class="form__field relative">
            <i class="input-icon material-icons absolute text-grey-darker">search</i>
            <input
              class="input__search"
              id="where"
              type="text"
              placeholder="Mexico City, Mexico">
          </div>
        </form>
      </div>
    </section>
    <section class="section_create py-6">
     <div class="container">
       <h1 class="text-3xl">Publish a new room</h1>
       <form action="submit">
         <div class="mb-4">
           <label for="" class="input__label">Title</label>
           <input v-model="publication.title"
                  type="text" class="input__field" placeholder="John Doe">
         </div>
         <div class="mb-4">
           <label for="" class="input__label">Description</label>
           <textarea v-model="publication.description" type="text" class="input__field"
                     rows="10" placeholder="John Doe"> </textarea>
         </div>
         <div class="mb-4">
           <label for="" class="input__label">Feature Image</label>
           <input v-model="publication.featureImage" type="text" class="input__field"
                  placeholder="https://images.unsplash.com/photo-1573770397940-1bb341182ac0?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=634&q=80">
         </div>
         <div class="mb-4" v-for="service in services" :key="service['.key']">
           <input type="checkbox"
                  @click="checked(service['.key'])"
                  :id="service['.key']">
           <label for="checkbox"> {{ service.name }}</label><br>
         </div>
         <div class="mb-4 text-right">
           <button v-on:click.prevent="save" class="w-full bg-yellow-dark
           font-semibold py-3 px-6 rounded">Create</button>
         </div>
       </form>
     </div>
    </section>
  </page-layout>
</template>

<script>
import { mapGetters } from 'vuex';
import PageLayout from '../layouts/PageLayout.vue';

export default {
  name: 'CreateHousePage',
  components: {
    PageLayout,
  },
  computed: {
    ...mapGetters([
      'services',
    ]),
  },
  data() {
    return {
      publication: {
        title: '',
        description: '',
        featureImage: '',
        idservice: {},
      },
    };
  },
  methods: {
    save() {
      const {
        title, description, featureImage, idservice,
      } = this.publication;
      const room = {
        title,
        description,
        featured_image: featureImage,
        services: idservice,
        publishedAt: Date.now(),
      };
      this.$store.dispatch('CREATE_ROOM', room)
        .then(() => {
          this.$router.push({ name: 'SearchPage' });
        });
    },
    checked(idserv) {
      this.publication.idservice[idserv] = idserv;
    },
  },
  beforeCreate() {
    this.$store.dispatch('FETCH_SERVICES');
  },
};
</script>