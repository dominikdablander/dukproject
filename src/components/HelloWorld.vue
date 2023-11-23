<template>
  <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
    <div>
      <div class="flex justify-center p-4 md:p-8" style="padding: 20px">
        <div class="flex overflow-x-auto">
          <div
            v-for="(column, index) in columns"
            :key="index"
            class="w-96 md:w-auto"
          >
            <p
              class="text-gray-700 font-semibold font-sans tracking-wide text-sm"
            >
              {{ column.title }}
            </p>
            <draggable
              :key="index"
              class="t-table"
              :class="{ 't-table-right': index === 0 }"
              style="min-height: 300px"
              v-model="column.tasks"
              item-key="id"
              group="tasks"
              ghost-class="ghost-card"
              :empty-insert-threshhold="50"
            >
              <template #item="{ element }">
                <task-card
                  class="mt-3 cursor-move"
                  :task="element"
                  :key="element.id"
                ></task-card>
              </template>
            </draggable>
          </div>
        </div>
      </div>
    </div>
    <div>
      <div class="flex justify-center p-4 md:p-8">
        <div class="flex overflow-x-auto">
          <div
            v-for="(column, index) in vorrat"
            :key="index"
            class="w-96 md:w-auto"
          >
            <p
              class="text-left text-gray-700 font-semibold font-sans tracking-wide text-sm"
            >
              {{ column.title }}
            </p>
            
            <draggable
              :key="index"
              class="table"
              style="min-height: 300px"
              v-model="column.tasks"
              item-key="id"
              group="tasks"
              ghost-class="ghost-card"
              :empty-insert-threshhold="50"
            >
              <template #item="{ element }">
                <task-card
                  class="mt-3 cursor-move"
                  :task="element"
                  :key="element.id"
                ></task-card>
              </template>
            </draggable>
          </div>
        </div>
      </div>
      
    </div>
  </div>
</template>

<script>
import draggable from "vuedraggable";
import TaskCard from "./TaskCard.vue";
export default {
  name: "HelloWorld",

  components: {
    draggable,
    TaskCard,
  },

  methods: {
    checkTasks(tasks) {
      // IDs der erwarteten Tasks
      const expectedIDsSoll = [1, 2];
      const expectedIDsHaben = [3, 4];

      // IDs in den gegebenen Tasks überprüfen
      const idsSoll = tasks.filter(task => task.id).map(task => task.id);
      const idsHaben = tasks.filter(task => task.id).map(task => task.id);

      // Überprüfung, ob die erwarteten IDs in den gegebenen Tasks enthalten sind
      const isSollCorrect = expectedIDsSoll.every(id => idsSoll.includes(id));
      const isHabenCorrect = expectedIDsHaben.every(id => idsHaben.includes(id));

      // Ausgabe entsprechend der Überprüfung
      if (isSollCorrect && isHabenCorrect) {
        alert('Richtig!');
      } else {
        alert('Falsch!');
      }
    }
  },

  data() {
    return {
      tasks: [
        {
          id: 1,
          title: "Add discount code to checkout page",
          date: "Sep 14",
          type: "Feature Request",
        },
        {
          id: 2,
          title: "Provide documentation on integrations",
          date: "Sep 12",
        },
      ],
      vorrat: [
        {title: "Ordnen Sie die Begriffe zu (Drag & Drop)",
      tasks: [
      {
              id: 1,
              title: "Anlagevermögen",
              date: "Sep 14",
              type: "Feature Request",
            },
            {
              id: 2,
              title: "Umlaufvermögen",
              date: "Sep 12",
            },
            {
              id: 3,
              title: "Eigenkapital",
              date: "Sep 9",
              type: "Design",
            },
            {
              id: 4,
              title: "Fremdkapital",
              date: "Sep 14",
              type: "Feature Request",
            },
      ]}
      ],
      columns: [
        {
          title: "Soll",
          tasks: [],
        },
        {
          title: "Haben",
          tasks: [
            
          ],
        },

      
      ],
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>



.t-table {
  border-top: 2px solid black;
  padding: 10px;
}
.t-table-right {
  border-right: 2px solid black; /* Rechter Rand für das erste Element */
}

.column-width {
  min-width: 320px;
  width: 320px;
}

.ghost-card {
  opacity: 0.5;
  background: #f7fafc;
  border: 1px solid #4299e1;
}

@media (min-width: 768px) {
  .w-96 {
    width: 24rem;
  }
  .mr-4 {
    margin-right: 1rem;
  }
}
@media (min-width: 1024px) {
  .md:w-auto {
    width: auto;
  }
  .md:mr-8 {
    margin-right: 2rem;
  }
}
</style>
