<template>
  <div>
    <b-button id="show-btn" @click="$bvModal.show('bv-modal-example')" size="sm" class="my-2 my-sm-0 nav-but add pt-2 pb-2 mr-3" >
     Favorite Tag</b-button>

    <b-modal id="bv-modal-example" hide-footer>
      <template v-slot:modal-title>
        Favorite Tags <code>Hackoverflow</code>
      </template>
      <div class="d-block text-center">
        <form @submit.stop.prevent="login">
        <b-form-tags
            v-model="tags"
            separator=" "
            placeholder="Enter new tags separated by space"
            remove-on-delete
            no-add-on-enter
            class="mb-2"
          ></b-form-tags>
          <label v-for="(tag, index) in fetchTags" :key="index">
          <button type="button" class="btn btn-sm btn-danger disabled ml-2 tags" data-toggle="button" aria-pressed="false">
          {{tag}}
          </button></label>
          <b-button class="mt-5" block @click="create" variant="info">
            Add</b-button>
        </form>
      </div>
    </b-modal>
  </div>
</template>

<script>
import swal from 'sweetalert2'

export default {
  data () {
    return {
      tags: []
    }
  },
  methods: {
    create () {
      this.$store.dispatch('editTags', this.tags)
        .then(({ data }) => {
          swal.fire({
            title: 'Success!',
            text: 'Tags successfully to added',
            icon: 'success',
            confirmButtonText: 'Ok'
          })
          this.$store.commit('SET_TAGS', data.tags)
          this.$bvModal.hide('bv-modal-example')
          this.onReset()
        })
        .catch((err) => {
          swal.fire({
            title: 'Success!',
            text: err.response,
            icon: 'success',
            confirmButtonText: 'Ok'
          })
        })
    },
    onReset () {
      this.tags = []
    }
  },
  computed: {
    fetchTags () {
      return this.$store.state.tags
    }
  },
  created () {
    this.$store.dispatch('fetchTags')
  }
}
</script>

<style scoped>

.title {
  border-radius: 0 !important;
  border-color: #18a2b8;
  border-width: 0 0 0 4px;
}

.title:hover {
  border-color: #18a2b8;
  border-radius: 0 !important;
  border-width: 1px 1px 1px 4px;
}

.title:focus {
  border-color: #18a2b8;
  border-radius: 0 !important;
  border-width: 1px 1px 1px 4px;
  box-shadow: none;
}

.add,
.add:focus {
  background-color: #0196ff;
  color: white;
  border-color: #39739e;
  box-shadow: none !important;
}

.add:hover {
  background-color: #0077cc;
}

</style>
