<template>
  <div class="container">
    <h1>Pokemon's Abilitys List</h1>
        <div class="row justify-content-end">
            <div class="col">
                <input v-model="search" class="form-control" placeholder="Search pokemon..." type="search">
            </div>
            <div class="col-3">
                <select v-model="form.limit" class="form-select">
                    <option v-for="limit in [20, 40, 80, 160]" :value="limit">
                        Limit: {{ limit }}
                    </option>
                </select>
            </div>
            <div class="col-2 d-flex justify-content-end">
                <div class="btn-group w-100" role="group">
                    <button
                        @click="onSearch(-1)"
                        type="button"
                        class="btn btn-primary"
                        :disabled=" is_previos_disabled"
                    >Previous
                    </button>
                    <button
                        @click="onSearch()"
                        type="button"
                        class="btn btn-primary"
                        :disabled="is_next_disabled"
                    >Next
                    </button>
                </div>
            </div>
        </div>
    <table class="table">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">Ability</th>
          <th scope="col">Url</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(abilitys, index) in items.results">
          <td>{{ index + 1 + form.offset }}</td>
          <td>{{ abilitys.name }}</td>
          <td>{{ abilitys.url }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import debounce from "lodash.debounce";
import axios from "axios";

export default {
  // Se definen variables y datos que se mostrarán en el HTML
  data() {
    return {
      items: {
        count: 0,
      },
      search: '',
      form: {
        offset: 0,
        limit: 20,
        search: ''
      },
    };
  },
  
  // Objeto que observa cuando los valores de las variables cambian para ejecutar una función (handler())
  watch: {
    form: {
      deep: true,
      async handler() {
        await this.getAbilitys();
        console.log(this.form.offset, 'OFFSET');
      },
    },

    search: debounce(async function () {
      this.form.search = this.search;
    }, 2000),

  },
  
  computed: {
    is_previos_disabled() {
      return this.form.offset <= 0;
    },
    
    is_next_disabled() {
      return this.form.offset + this.form.limit >= this.items.count;
    },
  },

  // Consulta HTTP a la Api pokemon
  async created() {
    await this.getAbilitys();
  },

  // Se definen los métodos que ejecutarán alguna acción, como el @click de los botones
  methods: {
    // Método para aumentar o disminuir el offset en la paginación
    onSearch(pagination = 1) {
      if (pagination > 0) {
        this.form.offset += this.form.limit;
      } else {
        this.form.offset -= this.form.limit;
      }
    },

    // Método para hacer la consulta a la API
    async getAbilitys() {
      const response = await axios.get('https://pokeapi.co/api/v2/ability?',
        {
          ...this.form,
        }
      );
      this.items = await response.data;
    },
  },
};
</script>
