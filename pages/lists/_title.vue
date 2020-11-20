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
        <b-button type="submit" variant="primary" size="lg">
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
        'title': this.$route.params.title.split('-').join(' '),
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
  border: solid 5px #D2FF01;
  padding: 0 1.25rem 1.25rem;
  border-radius: 0.3rem;
}

legend {
  display: inline;
  width: auto;
  color: #D2FF01;
  font-weight: bold;
  padding: 0 20px;
  text-transform: capitalize;
}

form {
  margin-bottom: 0.75rem;
}

.list-group-item {
  border: none;
  background: transparent;
  color: #D2FF01;
}

.list-group-item:hover {
  background: #631FAA;
}

button {
  background: #631FAA;
}

button:hover, button:visited, button:focus, button:active {
  background: #2E144A !important;
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