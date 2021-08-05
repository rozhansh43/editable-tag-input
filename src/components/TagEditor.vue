<template>
  <b-form-group>
    <b-input-group :prepend="prepend">
      <b-input
        :placeholder="$t('shared.maxChars', { max })"
        class='position-relative'
        :maxlength="maxChar" 
        @keydown.enter='add'
        v-model="keyword"
      />
        
      <span class="d-flex align-items-center mx-2 position-absolute char-length">
        {{ maxChar }} / {{ (keyword && keyword.length) || 0 }} 
      </span>

      <b-input-group-append>
        <b-btn variant="info" @click="add">
          <font-awesome-icon
            icon="plus"
            class="align-middle"
            :class="{ 'mr-1': $dir.ltr, 'ml-1': $dir.rtl }"
          />

          {{ $t('shared.add') }}
        </b-btn>
      </b-input-group-append>
    </b-input-group>

    <b-row v-if="model && model.length">
      <b-col
        v-for="item in model"
        :key="item"
        cols="auto"
        class="mt-3"
      >
        <div class="py-1 px-3 bg-warning text-dark rounded-pill d-flex align-items-center">
          {{ item }}

          <b-btn variant="link" class="inherited-link p-0" @click="remove(item)">
            <font-awesome-icon
              icon="times"
              class="align-middle"
              :class="{ 'ml-2': $dir.ltr, 'mr-2': $dir.rtl }"
            />
          </b-btn>
        </div>
      </b-col>
    </b-row>
  </b-form-group>
</template>

<script>
  export default {
    name: 'MyTagEditor',
    props: {
      value: Array,
      prepend: String,
      max: Number,
      maxChar: Number
    },
    data () {
      return {
        keyword: null,
      }
    },
    methods: {
      add () {
        if (!this.keyword || (this.model && this.model.length && this.model.find(o => o === this.keyword)))
          return this.keyword = null
        else if (this.model && this.model.length && this.model.length >=6)
          return

        this.model.push(this.keyword)
        this.keyword = null
      },
      remove (item) {
        this.model = this.model.filter(o => o !== item)
      }
    },
    computed: {
      model: {
        get () {
          return this.value
        },
        set (val) {
          this.$emit('input', val)
        }
      }
    }
  }
</script>
<style scoped>
  .char-length {
    right: 78%;
    top: 30%;
    z-index: 3;
  }
</style>
