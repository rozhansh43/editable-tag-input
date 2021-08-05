<template>
  <div class="animated fadeIn">
    <b-alert :show="errors.any()" variant="danger" class="px-4">
      <ul class="m-0 px-1">
        <li v-for="err in errors.all()" :key="err">{{err}}</li>
      </ul>
    </b-alert>

    <b-card :header="$t('shared.basicFeatures')">
      <b-row>
        <b-col md="6">
          <b-form-group>
            <b-input-group size="lg">
              <b-input-group-prepend is-text>
                {{ $t('shared.title') }}
                <required-symbol/>
              </b-input-group-prepend>

              <b-form-input
                rounded
                dir="auto"
                :name="$t('shared.title')"
                v-validate="'required|max: 150'"
                v-model="model.title"
              />
            </b-input-group>
          </b-form-group>

          <b-form-group>
            <b-input-group :prepend="$t('services.excerpt')">
              <b-form-input
                dir="auto"
                v-model="model.excerpt"
                :name="$t('services.excerpt')"
                v-validate="'max: 150'"
              />
            </b-input-group>
          </b-form-group>
        </b-col>

        <b-col md="6">
          <b-form-group>
            <b-input-group :prepend="$t('shared.slug')" size="lg">
              <b-form-input v-model="model.slug"/>
            </b-input-group>
          </b-form-group>

          <b-form-group>
            <b-input-group>
              <b-input-group-prepend is-text>
                {{ $t('shared.sortOrder') }}
                <required-symbol/>
              </b-input-group-prepend>

              <numeric-input
                :name="$t('shared.sortOrder')"
                v-validate="'required'"
                v-model="model.sortOrder"
              />
            </b-input-group>
          </b-form-group>
        </b-col>
      </b-row>
    </b-card>

    <b-form-group>
      <tiny-mce mode="noMedia" v-model="model.body"/>
    </b-form-group>

    <b-card :header="$t('shared.seoFeatures')" class="mb-3">
      <b-form-group>
        <b-input-group :prepend="$t('shared.pageTitle')">
          <b-form-input
            v-model="model.metaTitle"
            :placeholder="$t('shared.showsInBrowserTab')"
          />
          </b-input-group>
      </b-form-group>

      <b-form-group class="mb-0">
        <b-input-group :prepend="$t('shared.pageDescription')">
          <b-textarea
            :rows="2"
            v-validate="'min:50'"
            v-model="model.metaDescription"
            :name="$t('shared.pageDescription')"
            :placeholder="$t('shared.pageDescriptionForSeo')"
          />
          </b-input-group>
      </b-form-group>
    </b-card>

    <b-card bg-variant="bright">
      <b-btn :disabled="progressing" @click.prevent="submit" variant="success">
        <font-awesome-icon
          :icon="progressing ? 'cog' : 'plus'"
          :spin="progressing"
          class="align-middle"
          :class="{'mr-1': $dir.ltr, 'ml-1': $dir.rtl}"
        />
        {{ $t('shared.create') }}
      </b-btn>
    </b-card>
  </div>
</template>

<script>
import EditForm from './EditForm'

export default {
  name: 'Create',
  mixins: [EditForm],
  mounted () {
    this.setBreadcrumb()
  },
  methods: {
    setBreadcrumb () {
      const items = []
      let path = ''

      path += '/our-services'
      items.push({ path, text: this.$t('nav.ourServices') })

      path += '/create'
      items.push({ path, text: this.$t('nav.add') })

      this.commitBreadcrumb(items)
    },
    submit () {
      this.preSubmit()
      .then(proceed => {
        if (proceed) {
          return this.$api.services.create(this.model, true)
          .then(() => {
            return this.success('ourServices.added', './index')
          })
          .catch(this.fail)
        }
      })
    }
  }
}
</script>

/////////////<template>

</template>


<script>
export default {

}
</script>

<style>

</style>