<template>
  <div class="flex h-screen items-center justify-center sm:px-6 md:px-4 md:py-12 lg:px-8">
    <div class="md:w-[480px]">
      <div
        class="mx-auto w-auto bg-cover bg-center [height:250px] md:rounded-t-xl"
        style="background-image: url(/images/pexels-engin-akyurt-2725744.jpg)"
        role="img"
        aria-label="A picture of a burger and fries"
      ></div>
      <div class="px-10 py-10">
        <form class="w-full max-w-md space-y-8" @submit.prevent="checkForm">
          <div>
            <h2 class="text-center text-3xl font-extrabold text-gray-900 dark:text-white">
              Help Me Eat
            </h2>
            <p class="text-center text-sm dark:text-white">
              v{{ version }} | Last updated: 12/03/2024
            </p>
            <p class="mt-4 text-center font-medium text-gray-500 dark:text-white">
              For all the times you can't decide what to eat...
            </p>
          </div>
          <div class="space-y-6">
            <div>
              <button
                type="submit"
                class="group relative flex w-full justify-center rounded-md border border-transparent bg-green-100 px-4 py-2 font-medium text-green-700 hover:bg-green-700 hover:text-white focus:outline-none focus:ring-2 focus:ring-offset-2 dark:bg-green-500 dark:text-white dark:hover:bg-green-700"
                @click="helpMeEat()"
              >
                Help Me Eat!
              </button>
            </div>
          </div>
        </form>
        <div class="mt-6 space-y-6">
          <button
            @click="toggleDark()"
            type="button"
            class="w-full rounded-md text-gray-300 hover:text-gray-700 dark:text-white"
          >
            Toggle Dark Theme
          </button>
          <p class="text-center font-medium text-gray-300">
            Made with ♥ by
            <a href="https://ridwanazeez.github.io/" class="underline" target="_blank">me</a>
            <br />
          </p>
        </div>
      </div>
    </div>
  </div>
  <!-- Results Modal -->
  <TransitionRoot as="template" :show="(show = show)">
    <Dialog as="div" class="relative z-10" :open="show" @close="show = !show">
      <TransitionChild
        as="template"
        enter="ease-out duration-300"
        enter-from="opacity-0"
        enter-to="opacity-100"
        leave="ease-in duration-200"
        leave-from="opacity-100"
        leave-to="opacity-0"
      >
        <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity" />
      </TransitionChild>

      <div class="fixed inset-0 z-10 overflow-y-auto">
        <div class="flex min-h-full items-center justify-center p-4 text-center sm:p-0">
          <TransitionChild
            as="template"
            enter="ease-out duration-300"
            enter-from="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
            enter-to="opacity-100 translate-y-0 sm:scale-100"
            leave="ease-in duration-200"
            leave-from="opacity-100 translate-y-0 sm:scale-100"
            leave-to="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
          >
            <DialogPanel
              class="relative transform overflow-hidden rounded-lg text-left shadow-xl transition-all sm:my-8 sm:w-full sm:max-w-lg"
            >
              <div class="bg-white px-4 pb-4 pt-5 dark:bg-gray-900 dark:text-white sm:p-6 sm:pb-4">
                <div>
                  <div class="mt-3 sm:mt-0">
                    <DialogTitle
                      as="h3"
                      class="pb-3 text-center text-xl font-bold text-gray-900 dark:text-white"
                    >
                      You Should Eat...
                    </DialogTitle>
                    <div class="mt-2">
                      <table class="w-full table-auto">
                        <tr>
                          <th class="py-1 text-left align-top" colspan="2">Restaurant:</th>
                          <td class="py-1 align-top">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</td>
                          <td class="py-1 text-right align-top">
                            {{ choice.restaurant }}
                          </td>
                        </tr>
                        <tr>
                          <th class="py-1 text-left align-top" colspan="2">Item:</th>
                          <td class="py-1 align-top">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</td>
                          <td class="py-1 text-right align-top">
                            {{ choice.item }}
                          </td>
                        </tr>
                        <tr>
                          <th class="py-1 text-left align-top" colspan="2">Price:</th>
                          <td class="py-1 align-top">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</td>
                          <td class="py-1 text-right align-top">
                            {{ '$ ' + Math.round(choice.price).toLocaleString() + ' ' }}GYD
                          </td>
                        </tr>
                        <tr>
                          <th class="py-1 text-left align-top" colspan="2">Contact:</th>
                          <td class="py-1 align-top">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</td>
                          <td class="py-1 text-right align-top">
                            <span v-for="(contact, index) in choice.contact" :key="index">
                              <a :href="`tel:${contact}`">{{ contact }}</a>
                              <span v-if="index < choice.contact.length - 1">, </span>
                            </span>
                          </td>
                        </tr>
                        <tr>
                          <th class="py-1 text-left align-top" colspan="2">Notes:</th>
                          <td class="py-1 align-top">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</td>
                          <td class="py-1 text-right align-top">
                            {{ choice.notes }}
                          </td>
                        </tr>
                      </table>
                    </div>
                  </div>
                </div>
              </div>
              <div
                class="bg-gray-50 px-4 py-3 dark:bg-gray-800 sm:flex sm:flex-row-reverse sm:px-6"
              >
                <button
                  type="button"
                  class="inline-flex w-full justify-center rounded-md border border-transparent bg-red-600 px-4 py-2 text-base font-medium text-white shadow-sm hover:bg-red-700 focus:outline-none focus:ring-2 focus:ring-red-500 focus:ring-offset-2 sm:ml-3 sm:w-auto sm:text-sm"
                  @click="show = !show"
                >
                  Close
                </button>
              </div>
            </DialogPanel>
          </TransitionChild>
        </div>
      </div>
    </Dialog>
  </TransitionRoot>
</template>

<script>
import { Dialog, DialogPanel, DialogTitle, TransitionChild, TransitionRoot } from '@headlessui/vue'
import { useDark, useToggle } from '@vueuse/core'
import { version } from '../package.json'

const filePath = '/restaurants.json'
const isDark = useDark()
const toggleDark = useToggle(isDark)
export default {
  components: {
    // eslint-disable-next-line vue/no-reserved-component-names
    Dialog,
    DialogPanel,
    DialogTitle,
    TransitionChild,
    TransitionRoot
  },
  data() {
    return {
      show: false,
      version: version,
      isDark,
      toggleDark,
      choice: {
        restaurant: '',
        item: '',
        price: '',
        contact: [],
        notes: ''
      },
      restaurants: []
    }
  },
  mounted() {
    this.fetchData()
  },
  computed: {},
  methods: {
    async fetchData() {
      try {
        const response = await fetch(filePath)
        this.restaurants = await response.json()
      } catch (error) {
        console.error('Error fetching JSON:', error)
      }
    },
    helpMeEat() {
      // Check if the array is empty
      if (this.restaurants.length == 0) {
        return 'No restaurants available'
      }

      // Generate a random index within the array length
      const randomIndex = Math.floor(Math.random() * this.restaurants.length)

      // Get a randomly selected restaurant
      const randomRestaurant = this.restaurants[randomIndex]

      // Check if the restaurant has a menu
      if (!randomRestaurant.menu || randomRestaurant.menu.length === 0) {
        return 'No menu items available for this restaurant'
      }

      // Generate a random index within the menu array
      const randomMenuItemIndex = Math.floor(Math.random() * randomRestaurant.menu.length)

      // Get a random menu item with price from the selected restaurant
      const randomMenuItem = {
        item: randomRestaurant.menu[randomMenuItemIndex],
        price: randomRestaurant.price[randomMenuItemIndex]
      }

      this.choice.restaurant = randomRestaurant.name
      this.choice.item = randomMenuItem.item
      this.choice.price = randomMenuItem.price
      this.choice.contact = randomRestaurant.contact
      this.choice.notes = randomRestaurant.notes
      this.show = !this.show
    }
  }
}
</script>
