<template>
    <div class="grid grid-flow-col auto-cols-max">
        <div style="min-width: 900px;  padding: 20px;">
            <canvas  id="financeChart"></canvas>
        </div>
        <div style="padding: 20px;">div 2</div>
    </div>
  </template>
  
  <script>
  import Chart from 'chart.js/auto';
  
  export default {
    mounted() {

     // Verschuldungsgrade in %
      // Beispielwerte für die Berechnungen
      const earnings = 100000;
      
      const zins = 0.01 
      const eigenkapital = 1200000;
      const fk = 100000;
      
      const interestExpense = 200000;
      const debt = 3000000;
      const zinsaufwand = fk * zins;
      const gewinn = earnings - zinsaufwand
  
      // Berechnung der Kennzahlen
      const debtRatio = ((fk/eigenkapital) * 100).toFixed(2);
      const ekr = ((gewinn / eigenkapital) * 100).toFixed(2);
      const fkr = ((zinsaufwand / fk) * 100).toFixed(2);
      const gkr = (((gewinn + zinsaufwand)/ (eigenkapital + fk)) * 100).toFixed(2);
  
      // Chart erstellen
      const ctx = document.getElementById('financeChart').getContext('2d');
      new Chart(ctx, {
        type: 'bar',
        data: {
    
          datasets: [
          {
              label: 'Eigenkapitalrentabilität (EKR)',
              data: [{x: debtRatio, y: ekr}], // Verwende den berechneten ROA-Wert
              backgroundColor: 'rgba(255, 99, 132, 0.5)',
          borderColor: 'rgba(255, 99, 132, 1)',
          borderWidth: 1
            },
            {
              label: 'Gesamtkapitalrentabilität (GKR)',
              data: Array(10).fill(gkr), // Verwende den berechneten ROA-Wert
              backgroundColor: 'rgba(54, 162, 235, 0.5)',
              borderColor: 'rgba(54, 162, 235, 1)',
              borderWidth: 1
            },
            {
              label: 'Fremdkapitalrentabilität = Fremdkapitalzins',
              data: Array(10).fill(fkr), // Verwende den berechneten Cost of Debt-Wert
              backgroundColor: 'rgba(75, 192, 192, 0.5)',
              borderColor: 'rgba(75, 192, 192, 1)',
              borderWidth: 1
            }
          ]
        },
        options: {
          scales: {
            x: {
              title: {
                display: true,
                text: 'Verschuldungsgrad in %'
              }
            },
            y: {
              title: {
                display: true,
                text: 'Rendite in %'
              },
              min: 0, 
            
            }
          }
        }
      });
    }
  };
  </script>
  
  <style>
  /* Stile für das Canvas-Element können hier angewendet werden */
  </style>
  