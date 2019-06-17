<template>
  <div>
    <ul class="ingr-list">
      <li class="ingredient" v-for="(ingredient, index) in ingredients" :key="index">
        <p>{{ingredient.quantity}}{{ingredient.unit}} {{ingredient.name}}</p>
        <div class="ingr-buttons">
          <button class="uk-button uk-button-small ingr-button" @click="editIngr(index)">Edit</button>
          <button class="uk-button uk-button-small ingr-button" @click="deleteIngr(index)">Delete</button>
        </div>
      </li>
    </ul>
    <div class="uk-form-row">
      <label>Quantità</label>
      <input type="text" placeholder="Quantità" v-model="quantity" class="uk-width-1-1">
      <label>Unità</label>
      <input type="text" placeholder="Unità" v-model="unit" class="uk-width-1-1">
      <label>Nome</label>
      <input type="text" placeholder="Nome ingrediente" v-model="name" class="uk-width-1-1">
    </div>
    <br>
    <button 
      class="uk-button uk-button-default add-button"
      @click="addIngredient">+</button>
  </div>
</template>

<script>
export default {
  mixins: [window.Storyblok.plugin],
  data() {
    return {
      ingredients: [],
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
        ingredients: this.ingredients
      }
    },
    pluginCreated() {
      // eslint-disable-next-line
      console.log('View source and customize: https://github.com/storyblok/storyblok-fieldtype')
    },
    addIngredient() {
      if (this.editMode) {
        this.ingredients[this.editIndex].name = this.name
        this.ingredients[this.editIndex].quantity = this.quantity
        this.ingredients[this.editIndex].unit = this.unit
      } else {
        this.ingredients.push({
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
      this.ingredients.splice(index, 1)
    },
    editIngr(index) {
      this.name = this.ingredients[index].name
      this.quantity = this.ingredients[index].quantity
      this.unit = this.ingredients[index].unit
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
.ingr-list {
  width: 100%;
  padding: 0;
}

.ingredient {
  width: 100%;
  margin: 2px 0;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.ingredient p {
  margin: 0;
}

.ingr-buttons {
  justify-self: right;
}

.ingr-button {
  margin: 2px;
}

.add-button {
  width: 100%;
}
</style>
