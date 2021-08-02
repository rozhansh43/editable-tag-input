<template>
  <div>
    <b-input-group :prepend="$t('shared.tags')" class="mt-3 tag-input">
      <b-form-input
        :placeholder="$t('shared.tag')"
        class='tag-input-text'
        @keydown.enter='addTag(name)'
        @keydown.delete='removeLastTag'
        maxlength="4" 
        v-model="name"
      />
        
      <b-input-group-append>
        <span class="d-flex align-items-center mx-2">
         4 / {{ name.length }} 
        </span>

        <b-button variant="info" @click="addTag(name)">
          <font-awesome-icon
            icon="plus"
            class="align-middle"
            :class="{'mr-1': $dir.ltr, 'ml-1': $dir.rtl}"
          />
          {{ $t('shared.add')}}
        </b-button>
      </b-input-group-append>
    </b-input-group>

    <div class="tag-input-tags d-flex align-items-center py-3">
      <b-button 
        @click="removeAll" 
        :class="{'mr-3': $dir.ltr, 'ml-3': $dir.rtl}" 
      >
        {{ $t('shared.remove')}}
      </b-button>
      
      <ul>
        <li v-for="tag in tags" :key="tag">
          <tags :tag="tag" @removeTag="removeTag"/>
        </li>
      </ul>

      <span class="mx-3" >
        6 / {{ tags.length }}
      </span>
    </div>
  </div>
</template>

<script>
import Tags from '@/components/shared/Tags'

  export default {
    name: "TagEditor",
    components: {
      Tags
    },
    data() {
      return {
        name: "",
        tags: [
          { 
            id: String(Math.floor(Math.random() * 999999999)),
            name: 'tag1'
          },
          {
            id: String(Math.floor(Math.random() * 999999999)),
            name: 'tag2'
          },
          {
            id: String(Math.floor(Math.random() * 999999999)),
            name: 'tag3'
          },
          {
            id: String(Math.floor(Math.random() * 999999999)),
            name: 'tag4'
          }
        ],
      }
    },
    methods: {
      addTag (name) {
      if (this.tags.map((e) => e.name).includes(name)) {
        return;
      }
      if (this.tags.length < 6 && name.length > 1 ) {
        this.tags.push({
          id: String(Math.floor(Math.random() * 999999999)),
          name,
        });
        this.name = "";
        }
      },
      removeTag (tagId) {
        this.tags = this.tags.filter(o => o.id !== tagId)
      },
      removeLastTag () {
        if ( this.name == 0 ) {
          this.tags.pop()
        }
      },
      removeAll () {
        this.tags = []
      }
    }
  }
</script>

<style>
  .tag-input {
    border: 1px solid #D9DFE7;
    border-radius: 4px;
    width: 100%;
  }

  .tag-input input {
    border:none
  }

  .tag-input-tags ul {
    margin: 0!important;
    padding-inline-start: 0!important
  }

  .tag-input-tags li input{
    text-align: center;
    border: none;
    border-radius: 5px;
  }

  .tag-input-tags li {
    color: black;
    float: left;
    margin-right: 5px;
    list-style-type: none;
    font-size: 18px;
  }
  .tag-input-tags .badge {
    font-weight: 500!important;
    cursor: pointer;
  }

  .tag-input-tags > span {
    cursor: pointer;
    opacity: 0.75;
  }

  .tag-input-text {
    border: none;
    background: none;
    width: 100%;
    padding-right: 8px;
  }
  .tag-input-tags button {
    background: transparent;
    border: 1px solid lightgray;
    border-radius: 15px;
    height: min-content;
    padding: 6px;
  }
</style>
