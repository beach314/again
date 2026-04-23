index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quadratic Function Graph</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        body { background-color: #f8fafc; }
        .container { max-width: 900px; margin: 2rem auto; padding: 1rem; background: white; border-radius: 12px; box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1); }
    </style>
</head>
<body>

<div class="container">
    <h1 class="text-2xl font-bold text-center text-slate-800 mb-4">Graph of $y = -16t^2 + 96t + 80$</h1>
    
    <div class="bg-blue-50 p-4 rounded-lg mb-6 text-sm text-slate-700">
        <p><strong>Key Properties:</strong></p>
        <ul class="list-disc ml-5 mt-2">
            <li><strong>Vertex:</strong> (3, 224) - The maximum height.</li>
            <li><strong>y-intercept:</strong> (0, 80)</li>
            <li><strong>x-intercept:</strong> <span class="text-red-600 font-bold">~6.74</span> (calculated via quadratic formula: $\frac{-96 \pm \sqrt{96^2 - 4(-16)(80)}}{2(-16)}$)</li>
        </ul>
    </div>

    <div class="relative h-[500px] w-full">
        <canvas id="quadraticChart"></canvas>
    </div>
</div>

<script>
    const ctx = document.getElementById('quadraticChart').getContext('2d');
    
    // Generate data points from t=0 to t=8
    const dataPoints = [];
    for (let t = 0; t <= 8; t += 0.1) {
        const y = -16 * Math.pow(t, 2) + (96 * t) + 80;
        dataPoints.push({ x: t.toFixed(2), y: y.toFixed(2) });
    }

    // Specific calculation for x-intercept
    // Using quadratic formula: t = [-b - sqrt(b^2 - 4ac)] / 2a
    const a = -16, b = 96, c = 80;
    const discriminant = Math.sqrt(b*b - 4*a*c);
    const xIntercept = (-b - discriminant) / (2 * a); // 6.7416...

    new Chart(ctx, {
        type: 'line',
        data: {
            datasets: [{
                label: 'y = -16t² + 96t + 80',
                data: dataPoints,
                borderColor: '#2563eb',
                borderWidth: 3,
                pointRadius: 0,
                fill: false,
                tension: 0.4
            },
            {
                label: 'x-intercept',
                data: [{x: xIntercept, y: 0}],
                pointBackgroundColor: '#ef4444',
                pointRadius: 6,
                pointHoverRadius: 8,
                showLine: false
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            scales: {
                x: {
                    type: 'linear',
                    title: { display: true, text: 'Time (t)', font: { size: 14, weight: 'bold' } },
                    min: 0,
                    max: 8,
                    ticks: { stepSize: 1 }
                },
                y: {
                    title: { display: true, text: 'Height (y)', font: { size: 14, weight: 'bold' } },
                    min: -100,
                    max: 300,
                    ticks: { stepSize: 50 },
                    grid: {
                        color: (context) => context.tick.value === 0 ? '#000' : '#e2e8f0',
                        lineWidth: (context) => context.tick.value === 0 ? 2 : 1
                    }
                }
            },
            plugins: {
                tooltip: {
                    callbacks: {
                        label: function(context) {
                            return `(t: ${context.parsed.x}, y: ${context.parsed.y})`;
                        }
                    }
                },
                annotation: { // Manual label for intercept if needed, but ChartJS standard is sufficient
                }
            }
        }
    });
</script>

</body>
</html>
