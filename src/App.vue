<template>
  <div class="bg-gray-50 font-sans min-h-screen">
    <div class="max-w-screen-lg mx-auto py-5">
      <h1 class="font-semibold text-xl leading-10 tracking-wide text-gray-900">List of Apink member</h1>
      <h1>Nama saya adalah {{ state.name }}</h1>
      <input v-model="state.name" />
      <p
        class="mt-1 max-w-2xl text-base leading-5 text-gray-500"
      >{{ favoriteCount }} Favorited Member</p>
      <ul class="grid grid-cols-1 gap-6 sm:grid-cols-2 lg:grid-cols-3 mt-5">
        <li
          class="col-span-1 flex flex-col text-center bg-white rounded-lg shadow"
          v-for="(member, index) in state.members"
          :key="index"
        >
          <div class="flex-1 flex flex-col p-8">
            <img
              class="w-32 h-32 flex-shrink-0 mx-auto bg-black rounded-lg object-cover object-top"
              :src="member.image"
              alt
            />
            <h3 class="mt-6 text-gray-900 text-sm leading-5 font-medium">{{member.name}}</h3>
            <dl class="mt-1 flex-grow flex flex-col justify-between">
              <dt class="sr-only">Title</dt>
              <dd class="text-gray-500 text-sm leading-5">{{ member.dateOfBirth }}</dd>
            </dl>
          </div>
          <div class="border-t border-gray-200">
            <div class="-mt-px flex">
              <div class="w-0 flex-1 flex border-r border-gray-200" @click="addToFavorite(member)">
                <a
                  href="#"
                  class="relative -mr-px w-0 flex-1 inline-flex items-center justify-center py-4 text-sm leading-5 text-gray-700 font-medium border border-transparent rounded-bl-lg hover:text-gray-500 focus:outline-none focus:shadow-outline-blue focus:border-blue-300 focus:z-10 transition ease-in-out duration-150"
                >
                  <svg
                    fill="currentColor"
                    viewBox="0 0 20 20"
                    class="w-5 h-5 text-gray-400"
                    :class="{'text-red-400': member.isFavorite}"
                  >
                    <path
                      fill-rule="evenodd"
                      d="M3.172 5.172a4 4 0 015.656 0L10 6.343l1.172-1.171a4 4 0 115.656 5.656L10 17.657l-6.828-6.829a4 4 0 010-5.656z"
                      clip-rule="evenodd"
                    />
                  </svg>
                  <span class="ml-3" :class="{'text-red-400': member.isFavorite}">Favorite</span>
                </a>
              </div>
            </div>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import { reactive, onMounted, computed, watch } from "vue";
export default {
  name: "App",

  setup() {
    console.log("setup dijalankan");
    const state = reactive({
      name: "semmi",
      members: []
    }); // reactive == data

    onMounted(async () => {
      console.log("ini di mounted");

      const response = await fetch("http://localhost:3002/apink");
      const data = await response.json();

      state.members = data;

      console.log(state);
    }); // onMounted = mounted

    function addToFavorite(member) {
      member.isFavorite = true;
      console.log("add to favorite ditekan dengan member", member);
    }

    const favoriteCount = computed(
      () => state.members.filter(member => member.isFavorite).length
    ); // computed = computed

    watch(
      () => state.name,
      (newValue, oldValue) => {
        console.log(newValue, "ini new value"),
          console.log(oldValue, "ini old value");
      }
    ); // watch = watch

    return {
      name: "Semmi",
      state,
      addToFavorite,
      favoriteCount
    };
  }
};
</script>
