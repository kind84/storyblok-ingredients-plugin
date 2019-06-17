<template>
  <div class="plugin-container">
    <ul class="ingr-list">
      <li class="ingredient" v-for="(ingredient, index) in model.ingredients" :key="index">
        <p>{{ingredient.quantity}} {{ingredient.unit}} {{ingredient.name}}</p>
        <div class="ingr-buttons">
          <button class="uk-button uk-button-primary uk-button-small ingr-button" @click="editIngr(index)">Edit</button>
          <button class="uk-button uk-button-danger uk-button-small ingr-button" @click="deleteIngr(index)">Delete</button>
        </div>
      </li>
    </ul>
    <div class="uk-form-row">
      <label>Quantity</label>
      <input type="text" placeholder="Quantity" v-model="quantity" class="uk-width-1-1">
      <label>Unit</label>
      <input type="text" placeholder="Unit" v-model="unit" class="uk-width-1-1">
      <label>Name</label>
      <input type="text" placeholder="Name" v-model="name" class="uk-width-1-1">
    </div>
    <br>
    <button 
      class="uk-button uk-button-primary add-button"
      @click="addIngredient"><strong>+</strong></button>
  </div>
</template>

<script>
export default {
  mixins: [window.Storyblok.plugin],
  data() {
    return {
      name: '',
      quantity: '',
      unit: '',
      editMode: false,
      editIndex: null
    }
  },
  methods: {
    initWith() {
      return {
        // needs to be equal to your storyblok plugin name
        plugin: 'ingredients',
        ingredients: []
      }
    },
    pluginCreated() {
      // eslint-disable-next-line
      console.log('View source and customize: https://github.com/storyblok/storyblok-fieldtype')
    },
    addIngredient() {
      if (this.name == '' && this.quantity == '' && this.unit == '') {
        return
      }
      if (this.editMode) {
        this.model.ingredients[this.editIndex].name = this.name
        this.model.ingredients[this.editIndex].quantity = this.quantity
        this.model.ingredients[this.editIndex].unit = this.unit
      } else {
        this.model.ingredients.push({
          name: this.name,
          quantity: this.quantity,
          unit: this.unit
        })
      }
      this.name = ''
      this.quantity = ''
      this.unit = ''
      this.editMode = false
    },
    deleteIngr(index) {
      this.model.ingredients.splice(index, 1)
    },
    editIngr(index) {
      this.name = this.model.ingredients[index].name
      this.quantity = this.model.ingredients[index].quantity
      this.unit = this.model.ingredients[index].unit
      this.editMode = true
      this.editIndex = index
    }
  },
  watch: {
    'model': {
      handler: function (value) {
        this.$emit('changed-model', value);
      },
      deep: true
    }
  }
}
</script>

<style>
.plugin-container {
  box-sizing: border-box;
}

.ingr-list {
  /* width: 100%; */
  padding: 0;
  margin-top: 1px;
}

.ingredient {
  /* width: 99%; */
  margin: -1px 0;
  padding: 2px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  border: 1px solid #ccc;
  border-radius: 2px;
  background-color: white;
}

.ingredient p {
  margin: 0;
  padding-left: 2px;
}

.ingr-buttons {
  justify-self: right;
  display: flex;
}

.ingr-button {
  margin: 2px !important;
}

.add-button {
  width: 100%;
}
</style>
