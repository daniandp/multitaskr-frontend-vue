<template>
    <div class="container">
        <h1>Pokemon List</h1>
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
                        @click="onPaginate(-1)"
                        type="button"
                        class="btn btn-primary"
                        :disabled="previous_disabled"
                    >
                        Previous
                    </button>
                    <button
                        @click="onPaginate()"
                        type="button"
                        class="btn btn-primary"
                        :disabled="next_disabled"
                    >
                        Next
                    </button>
                </div>
            </div>
        </div>
        <table class="table">
            <thead>
                <tr>
                    <th scope="col">#</th>
                    <th scope="col">Pokemon</th>
                    <th scope="col">Url</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(pokemon, index) in items.results">
                    <td>{{ index + 1 + form.offset }}</td>
                    <td>{{ pokemon.name }}</td>
                    <td>{{ pokemon.url }}</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
import debounce from 'lodash.debounce'

export default {
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

    watch: {
        form: {
            deep: true,
            async handler() {
                await this.getPokemons();
            },
        },

        search: debounce(async function() {
            this.form.search = this.search
        }, 2000),
    },

    computed: {
        previous_disabled() {
            return this.form.offset <= 0;
        },

        next_disabled() {
            return this.form.offset + this.form.limit >= this.items.count;
        },
    },

    async created() {
        await this.getPokemons();
    },

    methods: {
        onPaginate(pagination = 1) {
            if (pagination > 0) {
                this.form.offset += this.form.limit;
            } else {
                this.form.offset -= this.form.limit;
            }
        },

        async getPokemons() {
            const response = await fetch(
                `https://pokeapi.co/api/v2/pokemon?offset=${this.form.offset}&limit=${this.form.limit}`
            );
            this.items = await response.json();
        },
    },
};
</script>
