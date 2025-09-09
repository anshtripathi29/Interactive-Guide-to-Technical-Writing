<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Guide to Technical Writing</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&family=Lora:wght@400;700&display=swap" rel="stylesheet">
    <!-- Chosen Palette: Warm Neutrals -->
    <!-- Application Structure Plan: A tab-based single-page application (SPA) is chosen for its superior user experience in exploring distinct topics. This structure avoids a long, potentially overwhelming scroll, allowing users to directly navigate to their area of interest (e.g., 'Career Tools'). The main sections are 'Overview', 'Document Types', 'Core Processes', and 'Career Tools'. Interaction is central: a dropdown filters chart data, hover effects reveal details in a process flow, and clickable rows highlight comparisons, transforming passive reading into active exploration. This design prioritizes user control and targeted information retrieval over a linear narrative. -->
    <!-- Visualization & Content Choices: 
        - Definition: Goal: Inform -> Presentation: Text block -> Justification: Foundational text.
        - Characteristics: Goal: Compare -> Viz: Radar Chart -> Interaction: Hover tooltips -> Justification: Shows the balance of multiple core skills effectively. (Chart.js, Canvas).
        - Document Types: Goal: Compare -> Viz: Bar Chart -> Interaction: Dropdown filter by industry -> Justification: Adds an interactive layer to show how document needs vary by professional context. (Chart.js, Canvas).
        - Report Writing Process: Goal: Organize -> Viz: HTML/CSS Flowchart -> Interaction: Hover to show details for each step -> Justification: Keeps the diagram clean while offering deeper insights on demand. (HTML/CSS/JS, No SVG/Mermaid).
        - CV vs. Resume: Goal: Compare -> Viz: HTML Table -> Interaction: Clickable rows to highlight corresponding attributes -> Justification: Actively engages the user in comparing the two documents side-by-side. (HTML/CSS/JS, No SVG).
    -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #FDFBF7; /* Warm Neutral Background */
            color: #4A4A4A;
        }
        h1, h2, h3 {
            font-family: 'Lora', serif;
        }
        .tab-btn {
            transition: all 0.3s ease;
            border-bottom: 3px solid transparent;
        }
        .tab-btn.active {
            border-bottom-color: #D5A021; /* Subtle Accent */
            color: #1A1A1A;
        }
        .content-section {
            display: none;
        }
        .content-section.active {
            display: block;
        }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 550px;
            margin-left: auto;
            margin-right: auto;
            height: 350px;
            max-height: 450px;
        }
        .flowchart-node {
            transition: all 0.3s ease;
            cursor: pointer;
        }
        .flowchart-node:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
        }
        .comparison-row {
            transition: background-color 0.3s ease;
            cursor: pointer;
        }
        .comparison-row.highlighted {
            background-color: #FEF3C7; /* Accent Highlight */
        }
    </style>
</head>
<body class="antialiased">

    <div class="min-h-screen">
        <header class="bg-white shadow-sm sticky top-0 z-10">
            <div class="container mx-auto px-4 sm:px-6 lg:px-8">
                <div class="flex justify-between items-center py-4">
                    <h1 class="text-2xl font-bold text-gray-800">Technical Writing Explorer</h1>
                    <nav id="tabs" class="hidden md:flex space-x-6">
                        <button data-tab="overview" class="tab-btn active font-medium pb-2">Overview</button>
                        <button data-tab="doctypes" class="tab-btn font-medium pb-2">Document Types</button>
                        <button data-tab="processes" class="tab-btn font-medium pb-2">Core Processes</button>
                        <button data-tab="career" class="tab-btn font-medium pb-2">Career Tools</button>
                    </nav>
                </div>
            </div>
        </header>
        
        <main class="container mx-auto p-4 sm:p-6 lg:p-8">
            <div id="content">
                <!-- Overview Section -->
                <section id="overview" class="content-section active">
                    <div class="text-center mb-8">
                        <h2 class="text-4xl font-bold text-gray-900">The Art of Clarity and Precision</h2>
                        <p class="mt-2 text-lg text-gray-600">Technical writing bridges the gap between complex information and user understanding.</p>
                    </div>
                    <div class="grid grid-cols-1 lg:grid-cols-2 gap-8 items-center">
                        <div class="bg-white p-6 rounded-lg shadow-md">
                            <h3 class="text-2xl font-bold mb-4 text-[#AF8C51]">Definition</h3>
                            <p class="leading-relaxed">Technical writing is a specialized form of communication designed to convey technical or complex information to a specific audience in a clear, concise, and effective manner. Its primary goal isn't just to inform, but to enable the audience to perform a task, make a decision, or understand a concept. It translates the language of experts—engineers, scientists, and programmers—into language that is accessible to the intended users.</p>
                        </div>
                        <div class="bg-white p-6 rounded-lg shadow-md">
                            <h3 class="text-2xl font-bold mb-4 text-center text-[#AF8C51]">Core Characteristics</h3>
                            <p class="text-center mb-4">Effective technical documents are a balance of several key qualities. Hover over the points on the chart to see how each contributes to the goal of clear communication.</p>
                            <div class="chart-container">
                                <canvas id="characteristicsChart"></canvas>
                            </div>
                        </div>
                    </div>
                </section>

                <!-- Document Types Section -->
                <section id="doctypes" class="content-section">
                    <div class="text-center mb-8">
                        <h2 class="text-4xl font-bold text-gray-900">A Spectrum of Documents</h2>
                        <p class="mt-2 text-lg text-gray-600">The output of technical writing varies greatly depending on the industry and audience.</p>
                    </div>
                    <div class="bg-white p-6 rounded-lg shadow-md">
                        <p class="text-center mb-4">The demand for different technical documents changes across industries. Select an industry below to see which document types are most commonly produced. This helps illustrate how a technical writer's focus might shift based on their field.</p>
                        <div class="max-w-xs mx-auto mb-6">
                            <select id="industryFilter" class="w-full p-2 border border-gray-300 rounded-md focus:ring-2 focus:ring-[#D5A021]">
                                <option value="software" selected>Software Development</option>
                                <option value="medical">Medical & Scientific</option>
                                <option value="engineering">Engineering & Manufacturing</option>
                            </select>
                        </div>
                        <div class="chart-container h-96 max-h-[500px]">
                            <canvas id="docTypesChart"></canvas>
                        </div>
                    </div>
                </section>

                <!-- Processes Section -->
                <section id="processes" class="content-section">
                     <div class="text-center mb-8">
                        <h2 class="text-4xl font-bold text-gray-900">Structured for Success</h2>
                        <p class="mt-2 text-lg text-gray-600">Great technical documentation is the result of a deliberate, methodical process.</p>
                    </div>
                     <div class="bg-white p-6 rounded-lg shadow-md">
                        <h3 class="text-2xl text-center font-bold mb-4 text-[#AF8C51]">The Report Writing Lifecycle</h3>
                        <p class="text-center max-w-3xl mx-auto mb-8">Writing a technical report is a multi-stage process that ensures accuracy, clarity, and completeness. Hover over each step below to learn more about the key activities involved.</p>
                        <div class="relative">
                            <div class="flex flex-col md:flex-row justify-between items-center space-y-4 md:space-y-0 md:space-x-4">
                                <!-- Step 1 -->
                                <div id="step-1" class="flowchart-node w-full md:w-1/5 bg-[#E1D4C0] p-4 rounded-lg text-center">
                                    <h4 class="font-bold">1. Plan & Research</h4>
                                </div>
                                <div class="text-2xl font-bold text-[#D5A021] hidden md:block">&rarr;</div>
                                <!-- Step 2 -->
                                <div id="step-2" class="flowchart-node w-full md:w-1/5 bg-[#E1D4C0] p-4 rounded-lg text-center">
                                    <h4 class="font-bold">2. Outline & Structure</h4>
                                </div>
                                <div class="text-2xl font-bold text-[#D5A021] hidden md:block">&rarr;</div>
                                <!-- Step 3 -->
                                <div id="step-3" class="flowchart-node w-full md:w-1/5 bg-[#E1D4C0] p-4 rounded-lg text-center">
                                    <h4 class="font-bold">3. Draft Content</h4>
                                </div>
                                <div class="text-2xl font-bold text-[#D5A021] hidden md:block">&rarr;</div>
                                <!-- Step 4 -->
                                <div id="step-4" class="flowchart-node w-full md:w-1/5 bg-[#E1D4C0] p-4 rounded-lg text-center">
                                    <h4 class="font-bold">4. Review & Edit</h4>
                                </div>
                            </div>
                            <!-- Tooltip -->
                            <div id="flowchart-tooltip" class="absolute hidden bg-gray-800 text-white text-sm rounded-lg p-3 shadow-lg z-20 max-w-xs"></div>
                        </div>
                    </div>
                </section>

                <!-- Career Tools Section -->
                <section id="career" class="content-section">
                    <div class="text-center mb-8">
                        <h2 class="text-4xl font-bold text-gray-900">Your Professional Toolkit</h2>
                        <p class="mt-2 text-lg text-gray-600">Understanding key career documents is essential for professional advancement.</p>
                    </div>
                    <div class="bg-white p-6 rounded-lg shadow-md">
                        <h3 class="text-2xl text-center font-bold mb-4 text-[#AF8C51]">Résumé vs. Curriculum Vitae (CV)</h3>
                        <p class="text-center max-w-3xl mx-auto mb-8">While often used interchangeably, these two documents have distinct purposes, formats, and audiences. Click on any attribute in the table below to highlight and directly compare the differences.</p>
                        <div class="overflow-x-auto">
                            <table class="w-full border-collapse">
                                <thead>
                                    <tr class="border-b-2 border-gray-300">
                                        <th class="p-3 text-left font-bold text-lg">Attribute</th>
                                        <th class="p-3 text-left font-bold text-lg">Résumé</th>
                                        <th class="p-3 text-left font-bold text-lg">Curriculum Vitae (CV)</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr class="comparison-row border-b border-gray-200">
                                        <td class="p-3 font-semibold">Purpose</td>
                                        <td class="p-3">A concise, targeted marketing document for a specific job application. Goal is to secure an interview.</td>
                                        <td class="p-3">A comprehensive, detailed history of one's academic and professional career.</td>
                                    </tr>
                                    <tr class="comparison-row border-b border-gray-200">
                                        <td class="p-3 font-semibold">Length</td>
                                        <td class="p-3">Strictly 1-2 pages. Brevity is key.</td>
                                        <td class="p-3">No length limit. Often multiple pages, growing over time.</td>
                                    </tr>
                                    <tr class="comparison-row border-b border-gray-200">
                                        <td class="p-3 font-semibold">Content</td>
                                        <td class="p-3">Focuses on relevant skills, experience, and achievements tailored to the job description.</td>
                                        <td class="p-3">Includes publications, research, presentations, grants, affiliations, and teaching experience.</td>
                                    </tr>
                                    <tr class="comparison-row">
                                        <td class="p-3 font-semibold">Audience</td>
                                        <td class="p-3">Primarily for corporate recruiters and hiring managers.</td>
                                        <td class="p-3">Primarily for academic, scientific, or research positions.</td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                    </div>
                </section>
            </div>
        </main>

    </div>

    <script>
    document.addEventListener('DOMContentLoaded', function () {
        const tabs = document.querySelectorAll('.tab-btn');
        const contentSections = document.querySelectorAll('.content-section');
        let characteristicsChart, docTypesChart;

        const dataStore = {
            docTypes: {
                software: {
                    labels: ['API Documentation', 'User Guides', 'Release Notes', 'Knowledge Base Articles', 'Developer Tutorials'],
                    data: [95, 85, 80, 75, 70]
                },
                medical: {
                    labels: ['Clinical Study Reports', 'Regulatory Submissions', 'Patient Information Leaflets', 'Research Papers', 'Lab Manuals'],
                    data: [90, 88, 82, 78, 70]
                },
                engineering: {
                    labels: ['Standard Operating Procedures', 'Technical Specifications', 'Assembly Manuals', 'Safety Data Sheets', 'Project Proposals'],
                    data: [92, 89, 85, 75, 72]
                }
            },
            flowchartDetails: {
                'step-1': '<strong>Plan & Research:</strong> Define the report\'s purpose, identify the target audience, and determine the scope. Gather all necessary data, sources, and information through research and interviews.',
                'step-2': '<strong>Outline & Structure:</strong> Create a logical framework for the report. This includes a table of contents, section headings, and subheadings that will guide the flow of information.',
                'step-3': '<strong>Draft Content:</strong> Write the body of the report, focusing on presenting information clearly and accurately. Adhere to the planned structure and prioritize substance over style in this initial phase.',
                'step-4': '<strong>Review & Edit:</strong> Meticulously check for clarity, conciseness, grammar, and technical accuracy. This phase often involves peer reviews and multiple revision cycles to refine the document.'
            }
        };

        function handleTabClick(tab) {
            tabs.forEach(t => t.classList.remove('active'));
            tab.classList.add('active');
            
            const targetId = tab.dataset.tab;
            contentSections.forEach(section => {
                section.classList.remove('active');
                if (section.id === targetId) {
                    section.classList.add('active');
                }
            });
            
            if (targetId === 'overview' && !characteristicsChart) {
                initCharacteristicsChart();
            } else if (targetId === 'doctypes' && !docTypesChart) {
                initDocTypesChart();
            }
        }
        
        tabs.forEach(tab => {
            tab.addEventListener('click', () => handleTabClick(tab));
        });

        function initCharacteristicsChart() {
            const ctx = document.getElementById('characteristicsChart').getContext('2d');
            characteristicsChart = new Chart(ctx, {
                type: 'radar',
                data: {
                    labels: ['Clarity', 'Accuracy', 'Conciseness', 'Audience-Aware', 'Objectivity', 'Accessibility'],
                    datasets: [{
                        label: 'Core Quality',
                        data: [10, 10, 8, 9, 8, 9],
                        backgroundColor: 'rgba(175, 140, 81, 0.2)',
                        borderColor: '#AF8C51',
                        pointBackgroundColor: '#AF8C51',
                        pointBorderColor: '#fff',
                        pointHoverBackgroundColor: '#fff',
                        pointHoverBorderColor: '#AF8C51'
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: { display: false }
                    },
                    scales: {
                        r: {
                            suggestedMin: 0,
                            suggestedMax: 10,
                            ticks: { stepSize: 2 }
                        }
                    }
                }
            });
        }
        
        function initDocTypesChart() {
            const ctx = document.getElementById('docTypesChart').getContext('2d');
            const initialData = dataStore.docTypes.software;
            docTypesChart = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: initialData.labels,
                    datasets: [{
                        label: 'Commonality Score',
                        data: initialData.data,
                        backgroundColor: ['#AF8C51', '#C3A97A', '#D7C6A3', '#E1D4C0', '#F0EBE3'],
                        borderRadius: 4
                    }]
                },
                options: {
                    indexAxis: 'y',
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: { display: false }
                    },
                    scales: {
                        x: { grid: { display: false } }
                    }
                }
            });
        }

        document.getElementById('industryFilter').addEventListener('change', function(e) {
            const selectedIndustry = e.target.value;
            const newData = dataStore.docTypes[selectedIndustry];
            docTypesChart.data.labels = newData.labels;
            docTypesChart.data.datasets[0].data = newData.data;
            docTypesChart.update();
        });
        
        const flowchartNodes = document.querySelectorAll('.flowchart-node');
        const flowchartTooltip = document.getElementById('flowchart-tooltip');
        
        flowchartNodes.forEach(node => {
            node.addEventListener('mouseenter', (e) => {
                const details = dataStore.flowchartDetails[node.id];
                flowchartTooltip.innerHTML = details;
                flowchartTooltip.classList.remove('hidden');

                const nodeRect = node.getBoundingClientRect();
                const containerRect = node.parentElement.parentElement.getBoundingClientRect();
                
                flowchartTooltip.style.left = `${nodeRect.left - containerRect.left}px`;
                flowchartTooltip.style.top = `${nodeRect.bottom - containerRect.top + 10}px`;
            });
            node.addEventListener('mouseleave', () => {
                flowchartTooltip.classList.add('hidden');
            });
        });

        const comparisonRows = document.querySelectorAll('.comparison-row');
        comparisonRows.forEach(row => {
            row.addEventListener('click', () => {
                const isHighlighted = row.classList.contains('highlighted');
                comparisonRows.forEach(r => r.classList.remove('highlighted'));
                if (!isHighlighted) {
                    row.classList.add('highlighted');
                }
            });
        });

        handleTabClick(document.querySelector('.tab-btn.active'));
    });
    </script>

</body>
</html>
