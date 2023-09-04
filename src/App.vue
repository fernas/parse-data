<template>
  <div class="min-h-screen flex flex-col justify-center py-12 sm:px-6 lg:px-8 bg-gray-50">
    <div class="sm:mx-auto sm:w-full sm:max-w-md">
      <h2 class="mt-6 text-center text-3xl font-extrabold text-gray-900">Parse your string</h2>
      <p class="mt-2 text-center text-sm text-gray-600">
        <span class="font-medium text-indigo-600 hover:text-indigo-500"> let's find out what your string hides  </span>
      </p>
    </div>

    <div class="mt-8 sm:mx-auto sm:w-full sm:max-w-md">
      <div class="bg-white py-8 px-4 shadow sm:rounded-lg sm:px-10">
        <form class="space-y-6" @submit.prevent="decrypt">
          <AlertComponent v-if="error" />
          <div class="text-left">
            <label for="string" class="block text-sm font-medium text-gray-700"> Your encrypted string </label>
            <div class="mt-1">
              <input v-model="string" id="string" type="text" required class="appearance-none block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm placeholder-gray-400 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
            </div>
          </div>
          <div v-if="parsedData" class="bg-gray-100 p-4 rounded">
            <p>Firstname: {{ parsedData.first_name }}</p>
            <p>Lastname: {{ parsedData.last_name }}</p>
            <p>ID: {{ parsedData.id }}</p>
          </div>
          <div>
            <button type="submit" class="w-full flex justify-center py-2 px-4 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">Get Data</button>
          </div>
        </form>
      </div>
    </div>
  </div>
  <FooterComponent />
</template>

<script>
import { ref } from 'vue'
import FooterComponent from './components/FooterComponent'
import AlertComponent from './components/AlertComponent'

export default {
  name: 'App',
  components: { AlertComponent, FooterComponent },
  setup() {
    const string = ref('')
    const parsedData = ref(null)
    const error = ref(false)

    const decrypt = () => {
      error.value = false
      const cleanedString = string.value.replace(/0+/g, '0').trim()
      const parts = cleanedString.split('0').filter(part => part !== '')

      if (parts.length === 3) {
        const [first_name, last_name, id] = parts

        parsedData.value = {
          first_name,
          last_name,
          id
        }
      } else {
        parsedData.value = null;
        error.value = true
      }
    }

    return {
      error,
      parsedData,
      string,

      decrypt
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
