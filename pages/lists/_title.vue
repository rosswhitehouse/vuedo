<template>
  <fieldset>
    <legend>{{ title }}</legend>
    <b-form @submit.prevent="submitTodo">
      <b-input-group>
        <b-form-input
          placeholder="Add new VueDo"
          v-model="newItem"
          size="lg"
        ></b-form-input>
        <b-button variant="primary" size="lg">
          Add New
        </b-button>
      </b-input-group>
    </b-form>
    <b-list-group>
      <b-list-group-item 
        v-for="(item, index) in listItems"
        :key="item.value"
      >
        <b-input-group>
          <b-form-checkbox
            v-model="item.checked"
            size="lg"
          >
            <span :class="[ item.checked ? 'strikethrough' : '' ]">
              {{ item.value }}
            </span>
          </b-form-checkbox>
          <b-icon
            icon="trash-fill"
            font-scale="1.5"
            @click="deleteTodo(index)"
          ></b-icon>
        </b-input-group>
      </b-list-group-item>
    </b-list-group>
  </fieldset>
</template>

<script>
  export default {
    data() {
      return {
        'title': this.$route.params.title,
        'listItems': [],
        'newItem': null
      }
    },
    computed: {
      listSlug: function () {
        return this.title.toLowerCase().split(' ').join('-');
      }
    },
    methods: {
      submitTodo() {
        this.listItems = [...this.listItems, {
          value: this.newItem,
          checked: false
        }];
        this.newItem = null
      },
      deleteTodo(index) {
        const items = this.listItems;
        items.splice(index, 1);
        this.listItems = items;
      }
    },
    created: function() {
      const list = window.localStorage.getItem(this.listSlug);
      if (list) this.listItems = JSON.parse(list);
    },
    updated: function() {
      console.log(this.listItems);
      console.log(JSON.stringify(this.listItems));
      window.localStorage.setItem(this.listSlug, JSON.stringify(this.listItems));
    }
  }
</script>

<style scoped>
.strikethrough {
  text-decoration: line-through;
}

fieldset {
  border: solid 1px silver;
  padding: 0 1.25rem 1.25rem;
}

legend {
  display: inline;
  width: auto;
}

form {
  margin-bottom: 0.75rem;
}

.list-group-item {
  border: none;
}

.list-group-item:hover {
  background: whitesmoke;
}

.custom-checkbox {
  flex-grow: 1;
}

.b-icon {
  align-self: center;
}

.b-icon:hover {
  color: red;
}

.custom-control-label {
  display: inline-block;
}

</style>