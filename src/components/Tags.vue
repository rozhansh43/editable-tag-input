<template>
  <b-badge 
    pill 
    variant="warning" 
    class="p-2 my-2 d-flex align-items-center"
    :style="editable ? badgeStyle : null"
  >
    <span @click="clicked" v-show="!editable">
      {{ tag.name }}
    </span>

    <input 
      type="text" 
      v-show="editable" 
      v-model="tag.name" 
      @keyup.enter="saved"
    />  

    <span 
      v-if="this.editable == false" 
      @click="removeTag(tag.id)" 
      :class="{'ml-2': $dir.ltr, 'mr-2': $dir.rtl}"
    >
      x
    </span>

    <span 
      v-else @click="removeTag(tag.id)" 
      :class="{'ml-2': $dir.ltr, 'mr-2': $dir.rtl}"
    >
      >
    </span>
  </b-badge>
</template>

<script>
export default {
  name: 'Tags',
  props: {
    tag: { type: String },
  },
  data () {
    return {
      editable: false,
      badgeStyle: {
        height: "30px"
      }
    }
  },
  methods: {
    clicked () {
      this.editable = true
    },
    saved () {
      this.editable = false
    },
    removeTag (tagId) {
      if ( this.editable == true ) {
        this.editable = false
      } else {
        this.$emit('removeTag', tagId);
      }
    }
  }
}
</script>
