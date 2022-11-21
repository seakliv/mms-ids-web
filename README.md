# mms-web

## Build Setup

```bash
# install dependencies
$ yarn install

# serve with hot reload at localhost:3000
$ yarn dev

# build for production and launch server
$ yarn build
$ yarn start

# generate static project
$ yarn generate
```

## Create a page

Recommended [Nuxt Property Decorator](https://github.com/nuxt-community/nuxt-property-decorator)

```
import { Component, mixins } from 'nuxt-property-decorator'

@Component
export default class MyPage extends Vue {}
```

### Create list page

Bellow example uses custom datatable component with filter and search functionalities provided by [@nestjsx/crud-request](https://github.com/nestjsx/crud/wiki/Requests#filter-conditions)

```
<template>
  <list-content :title="$t('patient.list.pageTitle')">
    <template #tool>
      <div class="float-right">
        <search-modal v-model="formModel.filter" @search="onSearch" />
      </div>
    </template>
    <template #table>
      <data-table
        v-model="data"
        :headers="headers"
        :items="data.items"
        @page-changed="pageChanged"
      >
        <template #name="{ item }">
          <nuxt-link
            class="text-blue-400"
            :to="`/patient/${item.id}`"
            >{{ item.name }}</nuxt-link
          >
        </template>
        </template>
      </data-table>
    </template>
  </list-content>
</template>

<script lang="ts">
import { Component, mixins } from 'nuxt-property-decorator'
import ListContent from '~/components/content/ListContent.vue'
import DataTable from '~/components/table/DataTable.vue'
import SearchForm from '~/components/pages/patient/SearchForm.vue'
import DataTableMixin from '~/mixins/datatable'
import SampleService from '~/services/sample.service'

@Component({
  layout: 'staff',
  components: {
    DataTable,
    SearchModal,
    ListContent,
  },
})
export default class MyListPage extends mixins(DataTableMixin) {
  // api service class
  service = new SampleService()

  // filter fields
  formModel = {
    filter: {
      name: '',
      age: '',
    },
  }

  // condition of each filter field
  conditions = {
    name: {
      operator: '$cont', // operator
      target: 'name', // column name of the database
    },

    age: {
      operator: '$eq',
      target: 'age',
    },
  }

  // define table columns and values
  get headers(): any[] {
    return [
      {
        text: this.$t('patient.model.name'),
        value: 'name', // field name of item of api response
      },
      {
        text: this.$t('patient.model.age'),
        value: 'age',
      },
    ]
  }
}
</script>

```

## Tailwind config viewer

You can find tailwind configuration by accessing to URL [http://localhost:8000/\_tailwind](http://localhost:8000/_tailwind)

## Components

We use basic built component provided by [vue-tailwind](https://www.vue-tailwind.com/docs/text-input).

Configuration path `~/plugins/tailwind-ui.ts`

## Form

Example of input element

```
<t-input-group
    :label="$t('inputLabel')"
    :feedback="$errors.first('name')"
    :variant="$errors.has('name') ? 'danger' : ''"
>
  <t-input v-model="model.name" />
</t-input-group>
```

## Form validation

The [@chantouchsek/validatorjs](https://www.npmjs.com/package/@chantouchsek/validatorjs) library makes data validation in JavaScript very easy in both the browser and Node.js.

HTML

```
<t-input-group
    :label="$t('inputLabel')"
    :feedback="$errors.first('name')"
    :variant="$errors.has('name') ? 'danger' : ''"
>
  <t-input v-model="validator.input.name" />
</t-input-group>
```

Script

```
import { Component, mixins } from 'nuxt-property-decorator'
import { Validation } from '~/mixins/validator'

@Component
export default class MyPage extends mixins(Validation) {
  validator: any = {
    input: {
      name: '',
    },
    rules: {
      name: 'required',
    },
    attributes: {
      name: this.$t('patient.name'),
    },
  }

  validate() {
    return true
  }

  ...
}
```

See [available rules](https://www.npmjs.com/package/@chantouchsek/validatorjs#available-rules)

## Git

### Commit message

**"type" must be one of the following mentioned below!**

- `build`: Build related changes (eg: npm related/ adding external dependencies)
- `chore`: A code change that external user won't see (eg: change to .gitignore file or .prettierrc file)
- `feat`: A new feature
- `fix`: A bug fix
- `docs`: Documentation related changes
- `refactor`: A code that neither fix bug nor adds a feature. (eg: You can use this when there is semantic changes like renaming a variable/ function name)
- `perf`: A code that improves performance
- `style`: A code that is related to styling
- `test`: Adding new test or making changes to existing test

More: https://www.conventionalcommits.org/en/v1.0.0

## Further Documentations

- [Nuxt.js docs](https://nuxtjs.org) https://nuxtjs.org
- [Nuxt Property Decorator](https://github.com/nuxt-community/nuxt-property-decorator) https://github.com/nuxt-community/nuxt-property-decorator
- [Nuxt i18n](https://i18n.nuxtjs.org) https://i18n.nuxtjs.org
- [Nuxt Auth](https://auth.nuxtjs.org) https://auth.nuxtjs.org
- [Tailwind CSS](https://tailwindcss.com/docs) https://tailwindcss.com/docs
- [Vue Tailwind](https://www.vue-tailwind.com) https://www.vue-tailwind.com
- [Git Convention](https://www.conventionalcommits.org/en/v1.0.0) https://www.conventionalcommits.org/en/v1.0.0
- [NestJs Crud](https://github.com/nestjsx/crud/wiki/Requests#filter-conditions) https://github.com/nestjsx/crud/wiki/Requests#filter-conditions
