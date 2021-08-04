<template>
  <div>
    <b-form-group>
      <b-input-group :prepend="prepend" class="item-input">
        <b-input
          :placeholder="placeholder"
          rounded
          class='position-relative'
          maxlength="4" 
          @keydown.enter='additem(newItem)'
          @keydown.delete='removeLastitem'
          v-model="newItem"
        />
          
        <span class="d-flex align-items-center mx-2 position-absolute char-length">
          4 / {{ newItem.length }} 
        </span>

        <b-input-group-append>
          <b-btn @click="additem(newItem)" variant="info">
            <font-awesome-icon
              icon="plus"
              class="align-middle"
              :class="{'mr-1': $dir.ltr, 'ml-1': $dir.rtl}"
            />
              {{ $t('shared.add')}}
          </b-btn>
        </b-input-group-append>
      </b-input-group>
    </b-form-group>

    <div class="d-flex align-items-center">    
      <ul class="p-0 list-unstyled"> 
        <li 
          v-for="item in model" 
          :key="item.id" 
          :class="{'mr-3': $dir.ltr, 'ml-3': $dir.rtl}"
          class="text-dark float-left"
        >
          <b-badge 
            pill 
            variant="warning" 
            class="p-2 d-flex align-items-center"
          >
            <span>
              {{ item }}
            </span>
            
            <span @click="removeitem(item)">
              <font-awesome-icon
                icon="times"
                class="align-middle"
                :class="{'ml-3': $dir.ltr, 'mr-3': $dir.rtl}"
              />
            </span>
           </b-badge>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'TagEditor',
    props: {
      value: { type: Array },
      prepend: { type: String },
      placeholder: { type: String }
    },
    data() {
      return {
        newItem: "",
      }
    },
    methods: {
      additem (newItem) {
        if (this.newItem && !this.model.find(o => o === this.newItem)) {
          if (this.model.length < 6 && newItem.length > 1 ) {
            this.model.push(newItem)
            this.newItem = ""
          }
        }
      },
      removeitem (item) {
        this.model = this.model.filter(o => o !== item)
      },
      removeLastitem () {
        if ( this.newItem == 0 ) {
          this.model.pop()
        }
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
.badge {
  font-size: 12px;
}
  .char-length {
    right: 78%;
    top: 30%;
    z-index: 3;
  }
</style>
