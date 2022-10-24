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

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld msg="You did it!" />
    </div>
  </header>

  <main>
    <TheWelcome />
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
