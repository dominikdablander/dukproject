<template>
  <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
    <div class="md:min-h-[500px] md: md:p-4">
      <canvas id="financeChart" ref="chartCanvas"></canvas>
    </div>
    <div class="p-10">
      <div class="rounded-lg p-4 shadow-md" style="max-width: 300px">
        <div class="price-range p-4">
          <p>Fremdkapitalzins</p>
          <span class="text-sm">{{ currentInterest }}</span>
          <span class="text-sm">%</span>
          <input
            class="w-full"
            type="range"
            min="0"
            max="30"
            step="0.1"
            v-model="currentInterest"
          />
          <div class="-mt-2 flex w-full justify-between">
            <span class="text-sm text-gray-600">0%</span>
            <span class="text-sm text-gray-600">30%</span>
          </div>
        </div>
        <div class="price-range p-4">
          <p>Fremdkapital in EUR</p>
          <span class="text-sm">{{ currentDebtString }}</span>
          <span class="text-sm"> EUR</span>
          <input
            class="w-full"
            type="range"
            min="0"
            max="1000000"
            step="10000"
            v-model="currentDebt"
          />
          <div class="-mt-2 flex w-full justify-between">
            <span class="text-sm text-gray-600">0,00 EUR </span>
            <span class="text-sm text-gray-600">1.000.000,00 EUR</span>
          </div>
        </div>
        <button
        @click="createChart(this.currentInterstRate, this.currentDebt)"
          type="button"
          class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800"
        >
          Berechnen
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import Chart from "chart.js/auto";

export default {
  data() {
    return {
      chartInstance: null,
      currentInterest: 7,
      currentDebt: 100000,
    };
  },

  computed: {
    currentDebtString: {
      get() {
        return this.currentDebt.toLocaleString("de-DE");
      },
      set(newValue) {
        this.currentDebt = parseFloat(
          newValue.replace(/\./g, "").replace(",", ".")
        );
      },
    },
    currentInterstRate() {
        return this.currentInterest / 100
    }

  },
  watch: {
    currentDebt(newValue) {
      this.currentDebtString = newValue.toLocaleString("de-DE");
    },
  },

  mounted() {
    this.createChart(this.currentInterstRate, this.currentDebt);
  },
  beforeUnmount() {},

  methods: {
    createChart(zins, fk) {

        let chartStatus = Chart.getChart('financeChart')
        if (chartStatus != undefined) {
            chartStatus.destroy()
        }
      // Verschuldungsgrade in %
      // Beispielwerte für die Berechnungen
      const earnings = 100000;

      
      const eigenkapital = 1200000;
      

      const interestExpense = 200000;
      const debt = 3000000;
      const zinsaufwand = fk * zins;
      const gewinn = earnings - zinsaufwand;

      // Berechnung der Kennzahlen
      const debtRatio = ((fk / eigenkapital) * 100).toFixed(2);
      const ekr = ((gewinn / eigenkapital) * 100).toFixed(2);
      const fkr = ((zinsaufwand / fk) * 100).toFixed(2);
      const gkr = (
        ((gewinn + zinsaufwand) / (eigenkapital + fk)) *
        100
      ).toFixed(2);

      // Chart erstellen
      const canvas = this.$refs.chartCanvas;
      canvas.parentNode.style.width = "100%";
      canvas.width = canvas.offsetWidth;

      new Chart(canvas, {
        type: "bar",
        data: {
          datasets: [
            {
              label: "Eigenkapitalrentabilität (EKR)",
              data: [{ x: debtRatio, y: ekr }], // Verwende den berechneten ROA-Wert
              backgroundColor: "rgba(255, 99, 132, 0.5)",
              borderColor: "rgba(255, 99, 132, 1)",
              borderWidth: 1,
            },
            {
              label: "Gesamtkapitalrentabilität (GKR)",
              data: Array(10).fill(gkr), // Verwende den berechneten ROA-Wert
              backgroundColor: "rgba(54, 162, 235, 0.5)",
              borderColor: "rgba(54, 162, 235, 1)",
              borderWidth: 1,
            },
            {
              label: "Fremdkapitalrentabilität = Fremdkapitalzins",
              data: Array(10).fill(fkr), // Verwende den berechneten Cost of Debt-Wert
              backgroundColor: "rgba(75, 192, 192, 0.5)",
              borderColor: "rgba(75, 192, 192, 1)",
              borderWidth: 1,
            },
          ],
        },
        options: {
          maintainAspectRatio: false,
          scales: {
            x: {
              title: {
                display: true,
                text: "Verschuldungsgrad in %",
              },
            },
            y: {
              title: {
                display: true,
                text: "Rendite in %",
              },
              min: 0,
            },
          },
        },
      });
    },
  },
};
</script>

<style>
/* Stile für das Canvas-Element können hier angewendet werden */
</style>
