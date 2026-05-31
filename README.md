# MD2-Pineapple-Ripening-Optimization
MD2 Pineapple Ripening Optimization
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Agro-Tech Derivative Tool: MD2 Ripening</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        canvas { cursor: crosshair; }
    </style>
</head>
<body class="bg-gray-50 font-sans text-gray-800">

    <div class="max-w-4xl mx-auto p-6">
        <header class="text-center mb-8">
            <h1 class="text-3xl font-bold text-green-700">MD2 Pineapple Ripening Analysis</h1>
            <p class="text-lg text-gray-600">Calculus in Malaysian Agro-Product Tech</p>
        </header>

        <section class="bg-white p-6 rounded-xl shadow-md mb-6">
            <h2 class="text-xl font-semibold mb-3 border-b pb-2">The Bioscience Problem</h2>
            <p class="mb-4">
                Determining the exact harvest window is critical for export. We model the Brix level (sugar content) of the MD2 variety over the final 20 days before harvest. 
                Using data trends typical of <strong>MARDI (Malaysian Agricultural Research and Development Institute)</strong> studies, we define the sweetness function as:
            </p>
            <div class="bg-gray-100 p-4 rounded-lg text-center mb-4">
                <p class="font-mono text-blue-700">f(x) = -0.04x² + 1.2x + 5</p>
                <p class="text-sm text-gray-500 mt-2">Where <strong>x</strong> = Days before harvest, <strong>f(x)</strong> = Brix percentage (%)</p>
            </div>
            <p class="mb-2">To find the <strong>Rate of Ripening</strong>, we calculate the first derivative:</p>
            <div class="bg-gray-100 p-4 rounded-lg text-center">
                <p class="font-mono text-red-700">f'(x) = -0.08x + 1.2</p>
                <p class="text-sm text-gray-500 mt-2">This tells us how many Brix units the sugar increases per day.</p>
            </div>
        </section>

        <section class="bg-white p-6 rounded-xl shadow-md">
            <h3 class="text-center text-gray-500 mb-4 italic italic">Click on the graph line to calculate the ripening rate at that specific day!</h3>
            <div class="relative h-80 w-full">
                <canvas id="growthChart"></canvas>
            </div>
            
            <div id="resultBox" class="mt-6 p-4 border-2 border-dashed border-green-300 rounded-lg hidden">
                <h4 class="font-bold text-green-800">Analysis for Day <span id="dayVal"></span>:</h4>
                <p id="calcResult" class="text-lg"></p>
                <p id="laymanResult" class="mt-2 text-gray-600 italic font-medium"></p>
            </div>
        </section>

        <footer class="mt-8 text-center text-sm text-gray-400">
            <p>Data Model based on simulated MD2 maturation profiles. | Applied Calculus for SFP Agro-Product Technology.</p>
        </footer>
    </div>

    <script>
        const ctx = document.getElementById('growthChart').getContext('2d');
        
        // Data generation
        const labels = Array.from({length: 21}, (_, i) => i);
        const dataPoints = labels.map(x => (-0.04 * Math.pow(x, 2)) + (1.2 * x) + 5);

        const growthChart = new Chart(ctx, {
            type: 'line',
            data: {
                labels: labels,
                datasets: [{
                    label: 'Brix Content (%)',
                    data: dataPoints,
                    borderColor: '#10b981',
                    backgroundColor: 'rgba(16, 185, 129, 0.1)',
                    borderWidth: 3,
                    fill: true,
                    tension: 0.4,
                    pointRadius: 5,
                    pointHoverRadius: 8
                }]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                onClick: (e) => {
                    const points = growthChart.getElementsAtEventForMode(e, 'nearest', { intersect: true }, true);
                    if (points.length) {
                        const index = points[0].index;
                        const x = labels[index];
                        calculateDerivative(x);
                    }
                },
                scales: {
                    y: { 
                        beginAtZero: true,
                        title: { display: true, text: 'Brix (%)' }
                    },
                    x: { 
                        title: { display: true, text: 'Days of Observation' }
                    }
                }
            }
        });

        function calculateDerivative(x) {
            // f'(x) = -0.08x + 1.2
            const rate = (-0.08 * x) + 1.2;
            const brix = (-0.04 * Math.pow(x, 2)) + (1.2 * x) + 5;
            
            document.getElementById('resultBox').classList.remove('hidden');
            document.getElementById('dayVal').innerText = x;
            document.getElementById('calcResult').innerHTML = `Current Sweetness: <strong>${brix.toFixed(2)}% Brix</strong> | Rate of Change (f'): <strong>${rate.toFixed(2)} Brix/day</strong>`;
            
            let explanation = "";
            if (rate > 0.8) {
                explanation = "The pineapple is in a 'rapid ripening' phase. Sugar is accumulating fast! Not ready for harvest yet.";
            } else if (rate > 0.2) {
                explanation = "Ripening is slowing down. We are approaching peak sweetness. Prepare logistics for harvest soon.";
            } else if (rate <= 0.2 && rate >= -0.1) {
                explanation = "Peak Maturity! The rate of change is near zero. Harvest now for maximum quality.";
            } else {
                explanation = "The fruit is over-ripening. Sugars may begin to ferment. Quality is declining.";
            }
            document.getElementById('laymanResult').innerText = "Expert Insight: " + explanation;
        }
    </script>
</body>
</html>
