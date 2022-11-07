<template>
  <div>
    <transition name="modal-fade">
      <div class="bg-gray-100 p-6">
        <div class="flex">
          <div class="flex-1 pr-4">
            <div class="flex-1">
              <div
                class="relative z-10"
                aria-labelledby="create-group"
                role="dialog"
                aria-modal="true"
              >
                <div
                  class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity"
                ></div>
                <div class="fixed inset-0 z-10 overflow-y-auto">
                  <div
                    class="flex min-h-full items-end justify-center p-4 text-center sm:items-center sm:p-0"
                  >
                    <div
                      class="relative transform overflow-hidden rounded-lg bg-white text-left shadow-xl transition-all sm:my-8 sm:w-full sm:max-w-lg sm:p-6"
                    >
                      <div class="flex border-b-2 border-solid border-slate-400 h-12">
                        <div class="flex gap-2 h-6 w-full">
                          <div
                            class="flex hover:bg-indigo-100 rounded-xl px-2"
                            @click="markCompletion()"
                          >
                            <svg
                              v-if="!isComplete"
                              xmlns="http://www.w3.org/2000/svg"
                              class="h-6 w-6"
                              fill="none"
                              viewBox="0 0 24 24"
                              stroke="#64748B"
                              stroke-width="2"
                            >
                              <path
                                stroke-linecap="round"
                                stroke-linejoin="round"
                                d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"
                              />
                            </svg>
                            <svg
                              v-if="isComplete"
                              xmlns="http://www.w3.org/2000/svg"
                              class="h-6 w-6"
                              fill="none"
                              viewBox="0 0 24 24"
                              stroke="blue"
                              stroke-width="2"
                            >
                              <path
                                stroke-linecap="round"
                                stroke-linejoin="round"
                                d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"
                              />
                            </svg>
                            <div
                              v-if="!isComplete"
                              class="text-slate-600 font-semibold text-sm self-center"
                            >
                              MARK AS COMPLETE
                            </div>
                            <div
                              v-if="isComplete"
                              class="text-slate-600 font-semibold text-sm self-center"
                            >
                              MARK AS INCOMPLETE
                            </div>
                          </div>
                        </div>

                        <div class="flex justify-end">
                          <Menu as="div" class="relative inline-block text-left">
                            <div>
                              <MenuButton
                                class="bg-gray-100 rounded-full flex items-center text-gray-400 hover:text-gray-600 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-offset-gray-100 focus:ring-indigo-500"
                              >
                                <span class="sr-only">Open options</span>
                                <svg
                                  xmlns="http://www.w3.org/2000/svg"
                                  class="h-5 w-5"
                                  fill="none"
                                  viewBox="0 0 24 24"
                                  stroke="#64748B"
                                  stroke-width="2"
                                >
                                  <path
                                    stroke-linecap="round"
                                    stroke-linejoin="round"
                                    d="M12 5v.01M12 12v.01M12 19v.01M12 6a1 1 0 110-2 1 1 0 010 2zm0 7a1 1 0 110-2 1 1 0 010 2zm0 7a1 1 0 110-2 1 1 0 010 2z"
                                  />
                                </svg>
                              </MenuButton>
                            </div>

                            <transition
                              enter-active-class="transition ease-out duration-100"
                              enter-from-class="transform opacity-0 scale-95"
                              enter-to-class="transform opacity-100 scale-100"
                              leave-active-class="transition ease-in duration-75"
                              leave-from-class="transform opacity-100 scale-100"
                              leave-to-class="transform opacity-0 scale-95"
                            >
                              <MenuItems
                                class="origin-top-right absolute right-0 mt-2 w-56 rounded-md shadow-lg bg-white ring-1 ring-black ring-opacity-5 focus:outline-none"
                              >
                                <div class="py-1">
                                  <MenuItem @click="showAlert()" v-slot="{ active }">
                                    <a
                                      href="#"
                                      :class="[
                                        active
                                          ? 'bg-gray-100 text-gray-900'
                                          : 'text-gray-700',
                                        'block px-4 py-2 text-sm',
                                      ]"
                                    >
                                      <span class="flex">
                                        <svg
                                          xmlns="http://www.w3.org/2000/svg"
                                          class="h-5 w-5"
                                          fill="none"
                                          viewBox="0 0 24 24"
                                          stroke="currentColor"
                                          stroke-width="2"
                                        >
                                          <path
                                            stroke-linecap="round"
                                            stroke-linejoin="round"
                                            d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"
                                          />
                                        </svg>
                                      <div class="ml-3"> Delete {{sectiontask}} </div>  
                                      </span></a
                                    >
                                  </MenuItem>
                                </div>
                              </MenuItems>
                            </transition>
                          </Menu>

                          <svg
                            @click="closeModal()"
                            xmlns="http://www.w3.org/2000/svg"
                            class="h-5 w-5"
                            fill="none"
                            viewBox="0 0 24 24"
                            stroke="#64748B"
                            stroke-width="2"
                          >
                            <path
                              stroke-linecap="round"
                              stroke-linejoin="round"
                              d="M6 18L18 6M6 6l12 12"
                            />
                          </svg>
                        </div>
                      </div>
                      <div class="pt-6">
                        <div class="text-slate-600 font-semibold">{{modalTitle}} </div>
                        <textarea
                          v-on:input="myChangeFunction(whatever)"
                          type="value"
                          v-model="details[0].title"
                          class="mt-1 block w-full rounded-md border shadow py-2 pl-3 pr-8 focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm text-slate-500"
                        ></textarea>
                      </div>
                      <div class="pt-6">
                        <div class="text-slate-600 font-semibold">Tags</div>

                        <span
                          class="flex gap-1 bg-blue-200 px-2 pl-3 h-9 rounded-full text-xs font-small text-slate-500 w-20 align-center"
                        >
                          <svg
                            xmlns="http://www.w3.org/2000/svg"
                            class="h-5 w-5 self-center"
                            viewBox="0 0 20 20"
                            fill="currentColor"
                          >
                            <path
                              fill-rule="evenodd"
                              d="M10 18a8 8 0 100-16 8 8 0 000 16zm1-11a1 1 0 10-2 0v2H7a1 1 0 100 2h2v2a1 1 0 102 0v-2h2a1 1 0 100-2h-2V7z"
                              clip-rule="evenodd"
                            />
                          </svg>
                          <div class="self-center">Add</div>
                        </span>
                      </div>
                      <div class="pt-6 flex">
                        <div>
                          <div class="text-slate-600 font-semibold">Priority</div>
                          <div>
                            <Listbox v-model="details[0].priority">
                              <div class="relative mt-1">
                                <ListboxButton
                                  class="bg-blue-200 px-2 pl-3 h-9 rounded-full text-xs font-small text-slate-500 w-40 align-center sm:text-sm"
                                >
                                  <span class="block truncate">{{
                                    details[0].priority
                                  }}</span>
                                 
                                </ListboxButton>
                                <transition
                                  leave-active-class="transition duration-100 ease-in"
                                  leave-from-class="opacity-100"
                                  leave-to-class="opacity-0"
                                >
                                  <ListboxOptions
                                    class="absolute w-full z-50 mt-1 max-h-60  overflow-auto rounded-md bg-white py-1 text-base shadow-lg ring-1 ring-black ring-opacity-5 focus:outline-none sm:text-sm"
                                  >
                                    <ListboxOption
                                      v-slot="{ active, selected }"
                                      v-for="priority in priorities"
                                      :key="priority"
                                      :value="priority"
                                      as="template"
                                    >
                                      <li
                                        :class="[
                                          active
                                            ? 'bg-amber-100 text-amber-900'
                                            : 'text-gray-900',
                                          'relative cursor-default select-none py-2 pl-10 pr-4',
                                        ]"
                                      >
                                        <span
                                          :class="[
                                            selected
                                              ? 'font-medium'
                                              : 'font-normal',
                                            'block truncate',
                                          ]"
                                          >{{ priority }}</span
                                        >
                                        
                                      </li>
                                    </ListboxOption>
                                  </ListboxOptions>
                                </transition>
                              </div>
                            </Listbox>
                          </div>
                        </div>
                        <div class="ml-6">
                          <div class="text-slate-600 font-semibold">Due Date</div>
                          <div class="mt-1">
                           <Datepicker placeholder="Select Date"  v-on:input="myChangeFunction(whatever)" v-model="details[0].date" :enableTimePicker="false" inputClassName="border-none bg-blue-200 h-9 rounded-full text-xs font-small text-slate-500 w-25" />
                          </div>
                        </div>
                      </div>
                      <div class="pt-6">
                        <div class="text-slate-600 font-semibold">Notes</div>
                        <textarea
                          type="text"
                          v-on:input="myChangeFunction(whatever)"
                          v-model="this.details[0].note"
                          class="mt-1 block w-full rounded-md border shadow py-2 pl-3 pr-8 focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm text-slate-500"
                        >
                        </textarea>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </transition>

    <TransitionRoot as="template" :show="open">
      <Dialog as="div" class="relative z-10" @close="open = false">
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

        <div class="fixed z-10 inset-0 overflow-y-auto">
          <div
            class="flex items-end sm:items-center justify-center min-h-full p-4 text-center sm:p-0"
          >
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
                class="relative bg-white rounded-lg text-left overflow-hidden shadow-xl transform transition-all sm:my-8 sm:max-w-lg sm:w-full"
              >
                <div class="bg-white px-4 pt-5 pb-4 sm:p-6 sm:pb-4">
                  <div class="sm:flex sm:items-start">
                    <div
                      class="mx-auto flex-shrink-0 flex items-center justify-center h-12 w-12 rounded-full bg-red-100 sm:mx-0 sm:h-10 sm:w-10"
                    >
                      <ExclamationIcon class="h-6 w-6 text-red-600" aria-hidden="true" />
                    </div>
                    <div class="mt-3 text-center sm:mt-0 sm:ml-4 sm:text-left">
                      <DialogTitle
                        as="h3"
                        class="text-lg leading-6 font-medium text-gray-900"
                      >
                        Delete {{sectiontask}}
                      </DialogTitle>
                      <div class="mt-2">
                        <p class="text-sm text-gray-500">
                          Are you sure you want to delete this {{sectiontask}}? This action cannot
                          be undone!
                        </p>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="bg-gray-50 px-4 py-3 sm:px-6 sm:flex sm:flex-row-reverse">
                  <button
                    type="button"
                    class="inline-flex items-center justify-center rounded-full border border-transparent bg-red-600 px-5 py-2 text-md font-medium text-white shadow-sm sm:w-auto"
                    @click="deleteHandler()"
                  >
                    Delete
                  </button>
                  <button
                    type="button"
                    class="mr-3 inline-flex items-center justify-center rounded-full border border-gray-300 bg-neutral-50 px-5 py-2 text-md font-medium text-slate-800 shadow-sm sm:w-auto"
                    @click="open = false"
                    ref="cancelButtonRef"
                  >
                    Cancel
                  </button>
                </div>
              </DialogPanel>
            </TransitionChild>
          </div>
        </div>
      </Dialog>
    </TransitionRoot>
  </div>
</template>

<script>
    import Datepicker from '@vuepic/vue-datepicker';
    import '@vuepic/vue-datepicker/dist/main.css'
    import {
        Menu,
        MenuButton,
        MenuItem,
        MenuItems
    } from "@headlessui/vue";
    import {
        Dialog,
        DialogPanel,
        DialogTitle,
        Listbox,
        ListboxLabel,
        ListboxButton,
        ListboxOptions,
        ListboxOption,
        TransitionChild,
        TransitionRoot,
    } from "@headlessui/vue";
    import {
        ref
    } from 'vue';
    import {
        ExclamationIcon
    } from "@heroicons/vue/outline";
    export default {
        name: "AddSection",
        components: {
            Menu,
            MenuItem,
            MenuButton,
            MenuItems,
            ExclamationIcon,
            Dialog,
            DialogPanel,
            DialogTitle,
            TransitionChild,
            TransitionRoot,
            Datepicker,
            Listbox,
            ListboxLabel,
            ListboxButton,
            ListboxOptions,
            ListboxOption,
        },
        props: {
            data: Array,
            modalTitle: String,
        },
        setup() {
            const date = ref();
            return {
                date,
            }
        },
        data() {
            return {
                isComplete: false,
                open: false,
                sectiontask: "",
                priorities: ["Normal", "High", "Low"],
                details: [{
                    title: "",
                    date: "",
                    note: "",
                    priority: "Normal"
                }],
            };
        },



        mounted() {
            console.log("mounted1");
            console.log(this.data);
            if (this.modalTitle === "Task Title") {
                this.sectiontask = "Task";
            } else this.sectiontask = "Section"
        },

        methods: {
            dateSelected(e) {
                console.log(this.details[0].date);
                this.$nextTick(() => console.log(this.details[0].date));
            },
            deleteHandler() {
                this.open = false;
                this.$emit("deleteSectionTask");
            },
            closeModal() {
                this.$emit("close");
            },
            markCompletion() {
                this.isComplete = !this.isComplete;
            },
            showAlert() {
                this.open = true;
            },
            myChangeFunction() {
                console.log("data", this.details)
                if (this.modalTitle === "Task Title") {
                    this.$emit("addTaskDetails", this.details);
                } else {
                    this.$emit("addSectionDetails", this.details);
                }

            },
        },
    };
</script>

<style>
    .modal-fade-enter,
    .modal-fade-leave-to {
        opacity: 0;
    }
    
    .modal-fade-enter-active,
    .modal-fade-leave-active {
        transition: opacity 0.5s ease;
    }
</style>