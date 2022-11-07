<template>
  <div class="relative bg-white">
    <div class="">
      <div
        class="flex justify-between items-center border-b-2 border-gray-200 py-6 md:justify-start md:space-x-10"
      >
        <div class="px-4 sm:px-6 lg:px-5 flex justify-start lg:w-0 lg:flex-1">
          <span class="sr-only">Workflow</span>
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-6 w-8 slate-500 hover:bg-slate-200 hover:rounded-xl"
            fill="none"
            viewBox="0 0 24 24"
            stroke="#64748b"
            stroke-width="2"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M4 6h16M4 12h16M4 18h16"
            />
          </svg>
        </div>
        <div class="-mr-2 -my-2 md:hidden">
          <button
            type="button"
            class="bg-white rounded-md p-2 inline-flex items-center justify-center text-gray-400 hover:text-gray-500 hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-inset focus:ring-indigo-500"
            aria-expanded="false"
          >
            <span class="sr-only">Open menu</span>
            <!-- Heroicon name: outline/menu -->
            <svg
              class="h-6 w-6"
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="2"
              stroke="currentColor"
              aria-hidden="true"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M4 6h16M4 12h16M4 18h16"
              />
            </svg>
          </button>
        </div>
      </div>
    </div>

    <!-- Table Data-->
    <div class="px-4 py-8 sm:px-6 py-10 lg:px-6 py-12">
      <div class="sm:flex sm:items-center">
        <div class="sm:flex-auto">
          <h1 class="text-4xl text-black font-extrabold tracking-tight leading-none">
            Tasks
          </h1>
          <p class="ml-0.5 font-medium text-gray-500">{{ taskCount }} remaining tasks</p>
        </div>
        <div class="mt-4 sm:mt-0 sm:ml-8 sm:flex-none">
          <button
            type="button"
            @click="
              showSectionModal();
              addSection();
            "
            class="inline-flex items-center justify-center rounded-full border border-transparent bg-slate-800 px-5 py-2 text-md font-medium text-white shadow-sm sm:w-auto"
          >
            <svg
              fill="#ffffff"
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 24 24"
              width="20px"
              height="20px"
              fill-rule="evenodd"
              class="mr-2"
            >
              <path
                fill-rule="evenodd"
                d="M 11 2 L 11 11 L 2 11 L 2 13 L 11 13 L 11 22 L 13 22 L 13 13 L 22 13 L 22 11 L 13 11 L 13 2 Z"
              /></svg
            >Add Section
          </button>
        </div>
        <div class="mt-4 sm:mt-0 sm:ml-4 sm:flex-none">
          <button
            type="button"
            @click="
              showTaskModal();
              addTask();
            "
            class="inline-flex items-center justify-center rounded-full border border-transparent bg-indigo-600 px-5 py-2 text-md font-medium text-white shadow-sm hover:bg-indigo-700 sm:w-auto"
          >
            <svg
              fill="#ffffff"
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 24 24"
              width="20px"
              height="20px"
              fill-rule="evenodd"
              class="mr-2"
            >
              <path
                fill-rule="evenodd"
                d="M 11 2 L 11 11 L 2 11 L 2 13 L 11 13 L 11 22 L 13 22 L 13 13 L 22 13 L 22 11 L 13 11 L 13 2 Z"
              />
            </svg>
            Add Task
          </button>
        </div>
      </div>
      <div class="mt-8 flex flex-col">
        <div class="-my-2 -mx-6 overflow-x-auto sm:-mx-6 lg:-mx-6">
          <div class="inline-block min-w-full py-2 align-middle">
            <div class="overflow-hidden shadow-sm ring-1 ring-black ring-opacity-5">
              <table
                class="min-w-full divide-y divide-gray-300"
                v-for="value in taskData"
              >
                <thead v-if="value.type == 'section'" class="bg-gray-50">
                  <tr @click="showEditSectionModal(value.sectionId)">
                    <th
                      scope="col"
                      class="border-t border-gray-200 py-2.5 pl-18 pr-3 text-left text-lg font-semibold text-gray-900 sm:pl-6"
                    >
                      {{ value.title }}
                    </th>
                    <th
                      scope="col"
                      class="border-t border-gray-200 relative py-3.5 pl-3 pr-4 sm:pr-6"
                    >
                      <span class="sr-only">Edit</span>
                    </th>
                  </tr>
                </thead>
                <tbody
                  v-if="value.type == 'task'"
                  class="divide-y divide-gray-200 bg-white"
                >
                  <tr class="border-t border-gray-200">
                    <td class="whitespace-nowrap py-3 pl-4 pr-3 text-sm sm:pl-6">
                      <div class="flex items-center">
                        <div class="flex items-center h-10 w-10 flex-shrink-0">
                          <svg
                            xmlns="http://www.w3.org/2000/svg"
                            class="h-6 w-6"
                            fill="none"
                            viewBox="0 0 24 24"
                            stroke="#4f46e5"
                            stroke-width="2"
                          >
                            <path
                              stroke-linecap="round"
                              stroke-linejoin="round"
                              d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"
                            />
                          </svg>
                        </div>
                        <div @click="showEditTaskModal(value.id)">
                          <div class="font-medium text-slate-400">
                            {{ value.title }}
                          </div>
                        </div>
                      </div>
                    </td>
                    <td
                      class="whitespace-nowrap justify-end py-3 pl-4 pr-3 text-right text-sm font-medium sm:pr-6 lg:pr-8"
                    >
                      <div class="flex justify-end items-center">
                        <svg
                          xmlns="http://www.w3.org/2000/svg"
                          class="h-5 w-5 mr-2"
                          fill="none"
                          viewBox="0 0 24 24"
                          stroke="#DC2626"
                          stroke-width="2"
                        >
                          <path
                            stroke-linecap="round"
                            stroke-linejoin="round"
                            d="M8 7l4-4m0 0l4 4m-4-4v18"
                          />
                        </svg>
                        <div class="text-slate-500">{{ value.date }}</div>
                      </div>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div v-if="isEditModalVisible">
      <EditSection
        v-show="isEditModalVisible"
        :key="isEditModalVisible"
        :editTaskData="editTaskData"
        :modalTitle="modalTitle"
        @close="closeModal($event)"
        @deleteEditSectionTask="deleteEditSectionTask($event)"
        @editTaskDetails="editTaskDetails($event)"
        @editSectionDetails="editSectionDetails($event)"
      />
    </div>
    <AddSection
      v-show="isModalVisible"
      :key="isModalVisible"
      :data="taskData"
      :modalTitle="modalTitle"
      @close="closeModal($event)"
      @deleteSectionTask="deleteSectionTask($event)"
      @addTaskDetails="addTaskDetails($event)"
      @addSectionDetails="addSectionDetails($event)"
    />
  </div>
</template>

<script>
import AddSection from "./views/Addsection.vue";
import EditSection from "./views/EditSection.vue";
import moment from "moment";

export default {
  name: "App",
  components: {
    AddSection,
    EditSection,
    moment,
  },
  data() {
    return {
      isModalVisible: false,
      isEditModalVisible: false,
      modalTitle: "",
      taskCount: "",
      editTaskData: [],
      taskData: [
        {
          userId: "1",
          id: "1",
          title: "",
          date: "",
          note: "",
          type: "",
        },
      ],
      data: [
        {
          id: "9",
          title: "Company Internal",
          date: "20-09-08",
          note: "This is the Note this is the note",
          type: "section",
        },
        {
          id: "1",
          title: "Start Twitter promotions using the company Twitter account",
          date: "20-09-08",
          note: "This is the Note this is the note",
          type: "task",
        },
        {
          id: "2",
          title: "Fix permission issues that the 0.0.7-alpha.2 has introduced",
          date: "20-09-08",
          note: "This is the Note this is the note",
          type: "task",
        },
        {
          id: "3",
          title: "Developer Api for the payment system",
          date: "20-09-08",
          note: "This is the Note this is the note",
          type: "section",
        },
        {
          id: "4",
          title: "Re-think the current API restrictions to loosen them a bit",
          date: "20-09-08",
          note: "This is the Note this is the note",
          type: "task",
        },
        {
          id: "5",
          title: "Pre-flight checks causes random crashes on logging service",
          date: "21-09-08",
          note: "This is the Note this is the note",
          type: "task",
        },
        {
          id: "6",
          title: "Create the landing/marketing page and host it on the beta channel",
          date: "21-09-08",
          note: "This is the Note this is the note",
          type: "task",
        },
      ],
    };
  },
  async mounted() {
    // fetch("https://jsonplaceholder.typicode.com/todos")
    //   .then(async (response) => {
    //     const data = await response.json();
    //     // check for error response
    //     if (!response.ok) {
    //       // get error message from body or default to response statusText
    //       const error = (data && data.message) || response.statusText;
    //       return Promise.reject(error);
    //     }
    //     this.taskData = data.slice(0, 99);
    //     this.taskData.map((val) => {
    //       val["type"] = "task";
    //       val["date"] = "7/18/2022";
    //       val["note"] = "note note note note";
    //     });
    //     console.log(this.taskData);
    //     this.taskData?.map((val) => {
    //       if (val.id) {
    //         this.taskCount++;
    //       }
    //     });
    //   })
    //   .catch((error) => {
    //     this.errorMessage = error;
    //     console.error("There was an error!", error);
    //   });
    if (this.taskData.length > 1) {
      this.taskData = JSON.parse(localStorage.getItem("sectionTask"));
    }
  },
  methods: {
    // Modals
    showEditTaskModal(id) {
      this.editTaskData = this.taskData.filter((x) => {
        return x.id === id;
      });
      this.isEditModalVisible = true;
      this.modalTitle = "Task Title";
    },
    showEditSectionModal(id) {
      this.editTaskData = this.taskData.filter((x) => {
        return x.id === id;
      });
      this.isEditModalVisible = true;
      this.modalTitle = "Section Title";
    },

    showSectionModal() {
      this.isModalVisible = true;
      this.modalTitle = "Section Title";
    },
    showTaskModal() {
      this.isModalVisible = true;
      this.modalTitle = "Task Title";
    },
    closeModal() {
      this.isModalVisible = false;
      this.isEditModalVisible = false;
      localStorage.setItem("sectionTask", JSON.stringify(this.taskData));
    },

    // Local Storage Methods

    // storeSectionTask(){
    //   if (data.length){
    //     this.taskData.push(data)
    //   }
    // }

    // getSectionsTasks(){

    // }

    // Add Section And Task Functions
    addTask() {
      let object = {
        userId: 1,
        id: (this.taskData?.length + 1).toString(),
        title: "Task title",
        date: "",
        note: "",
        type: "task",
      };
      this.taskData?.unshift(object);
      this.taskCount++;
    },
    addSection() {
      let object = {
        userId: 2,
        id: (this.taskData.length + 1).toString(),
        title: "Section title",
        date: "",
        note: "",
        type: "section",
      };
      this.taskData.unshift(object);
    },
    addTaskDetails(event) {
      const formattedDate = moment(event[0].date).format("MMMM DD");
      this.taskData[0].title = event[0].title;
      this.taskData[0].date = formattedDate;
      this.taskData[0].note = event[0].note;
    },

    addSectionDetails(event) {
      this.taskData[0].title = event[0].title;
      this.taskData[0].date = event[0].date;
      this.taskData[0].note = event[0].note;
    },

    // Edit Section and Tasks

    editTaskDetails(event) {
      const formattedDate = moment(event[0].date).format("MMMM DD");
      let index = this.taskData.findIndex((t) => t.id == event[0].id);
      this.taskData[index].title = event[0].title;
      this.taskData[index].date = formattedDate;
      this.taskData[index].note = event[0].note;
    },

    editSectionDetails(event) {
      let index = this.taskData.findIndex((t) => t.id == event[0].id);
      this.taskData[index].title = event[0].title;
      this.taskData[index].note = event[0].note;
    },
    // Delete Section-Task

    deleteEditSectionTask(id) {
      let index = this.taskData.findIndex((x) => {
        if (x.id) {
          return x.id == id;
        } else {
          return x.sectionId == id;
        }
      });
      console.log(index);
      this.taskData.splice(index, 1);
      this.isEditModalVisible = false;
      this.taskCount--;
    },

    deleteSectionTask() {
      this.taskData.splice(0, 1);
      this.isModalVisible = false;
    },
  },
};
</script>

<style scoped></style>
