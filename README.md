# Study
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Explorador Celular Interativo</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    <!-- Chosen Palette: Warm Neutrals (Beige, Soft Browns, Subtle Accents) -->
    <!-- Application Structure Plan: A "Virtual Cell Explorer" model. The main view is a large, interactive diagram of a cell made with HTML/CSS. Clicking on an organelle updates a dedicated information panel with details from the report. Navigation tabs allow users to switch between this "Cell Diagram" view, a "Comparative Overview" view with charts, and a "Genetic Flow" view explaining the central dogma. This structure was chosen to transform passive reading into active exploration, making complex biological relationships more intuitive and engaging than a linear text format. -->
    <!-- Visualization & Content Choices: 
        1. Cell Diagram (Inform/Explore): Report Info -> Organelle descriptions. Goal -> Allow free exploration. Viz/Method -> Interactive clickable HTML divs representing organelles. Interaction -> On-click updates the info panel. Justification -> Fosters discovery-based learning. Library -> Vanilla JS.
        2. Cytoskeleton Comparison (Compare): Report Info -> Stability of cytoskeleton filaments. Goal -> Compare characteristics. Viz/Method -> Bar Chart. Interaction -> Static visual reference. Justification -> Clearly quantifies qualitative descriptions. Library -> Chart.js.
        3. Genetic Flow Diagram (Organize/Show Process): Report Info -> Central Dogma (Replication, Transcription, Translation). Goal -> Illustrate a core process. Viz/Method -> Styled HTML/CSS flow diagram. Interaction -> Clicking a step reveals detailed text. Justification -> Breaks down a complex sequence into understandable steps. Library -> Vanilla JS.
    -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <style>
        body { font-family: 'Inter', sans-serif; }
        .active-tab { border-color: #a16207; color: #a16207; background-color: #fefce8; }
        .cell-bg { background-color: #fef3c7; }
        .nucleus-bg { background-color: #fde68a; }
        .organelle { transition: transform 0.2s ease-in-out, box-shadow 0.2s ease-in-out; cursor: pointer; }
        .organelle:hover { transform: scale(1.05); box-shadow: 0 0 15px rgba(0,0,0,0.2); }
        .content-fade-in { animation: fadeIn 0.5s ease-in-out; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .chart-container { position: relative; width: 100%; max-width: 600px; margin-left: auto; margin-right: auto; height: 350px; max-height: 400px; }
        @media (min-width: 768px) { .chart-container { height: 400px; } }
    </style>
</head>
<body class="bg-[#FDFBF6] text-gray-800">

    <div class="container mx-auto p-4 md:p-8">
        <header class="text-center mb-8">
            <h1 class="text-4xl md:text-5xl font-bold text-gray-900">🔬 Explorador Celular Interativo</h1>
            <p class="mt-2 text-lg text-gray-600">Uma viagem ao interior da unidade fundamental da vida, com base em "Histologia Básica" de Junqueira & Carneiro.</p>
        </header>

        <nav class="flex justify-center mb-8 border-b border-gray-200">
            <button onclick="showView('view-cell')" id="tab-cell" class="py-4 px-6 font-semibold text-gray-600 border-b-2 border-transparent hover:border-yellow-600 hover:text-yellow-800 focus:outline-none active-tab">A Célula</button>
            <button onclick="showView('view-overview')" id="tab-overview" class="py-4 px-6 font-semibold text-gray-600 border-b-2 border-transparent hover:border-yellow-600 hover:text-yellow-800 focus:outline-none">Visão Geral Comparativa</button>
            <button onclick="showView('view-flow')" id="tab-flow" class="py-4 px-6 font-semibold text-gray-600 border-b-2 border-transparent hover:border-yellow-600 hover:text-yellow-800 focus:outline-none">Fluxo Genético</button>
        </nav>

        <main>
            <!-- View 1: Interactive Cell -->
            <div id="view-cell">
                <p class="text-center text-gray-600 mb-6 max-w-3xl mx-auto">Esta seção apresenta um modelo interativo da célula eucariótica. Clique em qualquer organela no diagrama para explorar sua estrutura, função e importância. As informações detalhadas aparecerão ao lado, permitindo que você navegue pela complexa maquinaria celular de forma intuitiva e focada.</p>
                <div class="flex flex-col lg:flex-row gap-8">
                    <div class="w-full lg:w-1/2 h-[500px] md:h-[600px] relative cell-bg rounded-full border-4 border-amber-300 flex items-center justify-center p-4">
                        <!-- Nucleus -->
                        <div onclick="showInfo('nucleus')" class="organelle w-40 h-40 md:w-48 md:h-48 nucleus-bg rounded-full border-4 border-amber-500 flex items-center justify-center text-center p-2 font-bold text-amber-900">Núcleo</div>

                        <!-- Organelles -->
                        <div onclick="showInfo('mitochondria')" class="organelle absolute top-10 left-10 w-24 h-12 bg-red-300 border-2 border-red-500 rounded-full flex items-center justify-center text-xs font-semibold text-red-900">Mitocôndria</div>
                        <div onclick="showInfo('rer')" class="organelle absolute top-1/3 right-5 w-28 h-16 bg-blue-300 border-2 border-blue-500 rounded-lg flex items-center justify-center text-xs font-semibold text-blue-900 p-1 text-center">R.E. Rugoso</div>
                        <div onclick="showInfo('rel')" class="organelle absolute bottom-1/4 right-8 w-24 h-12 bg-sky-300 border-2 border-sky-500 rounded-lg flex items-center justify-center text-xs font-semibold text-sky-900 p-1 text-center">R.E. Liso</div>
                        <div onclick="showInfo('golgi')" class="organelle absolute bottom-10 left-12 w-24 h-16 bg-purple-300 border-2 border-purple-500 rounded-lg flex items-center justify-center text-xs font-semibold text-purple-900">Complexo de Golgi</div>
                        <div onclick="showInfo('lysosome')" class="organelle absolute top-12 right-12 w-10 h-10 bg-orange-300 border-2 border-orange-500 rounded-full flex items-center justify-center text-xs font-semibold text-orange-900">Lisossomo</div>
                         <div onclick="showInfo('cytoskeleton')" class="organelle absolute bottom-10 right-1/3 w-20 h-10 bg-gray-300 border-2 border-gray-500 rounded-md flex items-center justify-center text-xs font-semibold text-gray-900">Citoesqueleto</div>
                    </div>

                    <div id="info-panel" class="w-full lg:w-1/2 p-6 bg-white rounded-2xl shadow-lg border border-gray-200">
                        <div id="info-content" class="content-fade-in">
                            <h2 id="info-title" class="text-3xl font-bold text-gray-800 mb-4">Bem-vindo!</h2>
                            <p id="info-text" class="text-gray-600 leading-relaxed">Clique em uma organela para começar sua exploração. Cada componente da célula possui uma estrutura e função únicas que são essenciais para a vida. Este painel fornecerá todos os detalhes que você precisa saber.</p>
                        </div>
                    </div>
                </div>
            </div>

            <!-- View 2: Overview -->
            <div id="view-overview" class="hidden">
                 <p class="text-center text-gray-600 mb-8 max-w-3xl mx-auto">Nesta seção, oferecemos uma visão geral e comparativa dos componentes celulares. O gráfico abaixo ilustra uma característica fundamental do citoesqueleto: a estabilidade de seus filamentos. Observe como os filamentos intermediários fornecem suporte estrutural duradouro, enquanto microtúbulos e microfilamentos são altamente dinâmicos, permitindo movimento e mudanças na forma celular.</p>
                <div class="bg-white p-6 rounded-2xl shadow-lg border border-gray-200">
                    <h2 class="text-2xl font-bold text-center mb-4">Comparativo do Citoesqueleto: Estabilidade Estrutural</h2>
                    <div class="chart-container">
                        <canvas id="cytoskeletonChart"></canvas>
                    </div>
                </div>
            </div>

            <!-- View 3: Genetic Flow -->
            <div id="view-flow" class="hidden">
                 <p class="text-center text-gray-600 mb-8 max-w-3xl mx-auto">Esta seção desvenda o Dogma Central da Biologia Molecular, o fluxo de informação genética que governa a célula. O diagrama ilustra os três processos cruciais: Replicação (cópia do DNA), Transcrição (DNA para RNA) e Tradução (RNA para proteína). Clique em cada etapa para ver uma explicação detalhada sobre como a célula armazena, acessa e utiliza suas instruções genéticas para funcionar.</p>
                <div class="bg-white p-6 rounded-2xl shadow-lg border border-gray-200">
                    <h2 class="text-2xl font-bold text-center mb-4">O Fluxo da Informação Genética</h2>
                    <div class="grid md:grid-cols-3 gap-4 text-center items-start">
                        <!-- Replication -->
                        <div class="flex flex-col items-center p-4 rounded-lg bg-blue-50 border border-blue-200">
                            <h3 class="font-bold text-lg text-blue-800 mb-2">Replicação</h3>
                            <div class="text-3xl mb-2">🧬 ➡️ 🧬</div>
                            <p class="text-sm text-blue-700 mb-4">Local: Núcleo</p>
                            <button onclick="showFlowInfo('replication')" class="bg-blue-600 text-white px-4 py-2 rounded-lg text-sm font-semibold hover:bg-blue-700">Detalhes</button>
                        </div>
                        <!-- Transcription -->
                        <div class="flex flex-col items-center p-4 rounded-lg bg-purple-50 border border-purple-200">
                            <h3 class="font-bold text-lg text-purple-800 mb-2">Transcrição</h3>
                            <div class="text-3xl mb-2">🧬 ➡️ 📜</div>
                            <p class="text-sm text-purple-700 mb-4">Local: Núcleo</p>
                            <button onclick="showFlowInfo('transcription')" class="bg-purple-600 text-white px-4 py-2 rounded-lg text-sm font-semibold hover:bg-purple-700">Detalhes</button>
                        </div>
                        <!-- Translation -->
                        <div class="flex flex-col items-center p-4 rounded-lg bg-green-50 border border-green-200">
                            <h3 class="font-bold text-lg text-green-800 mb-2">Tradução</h3>
                            <div class="text-3xl mb-2">📜 ➡️ 💪</div>
                            <p class="text-sm text-green-700 mb-4">Local: Citoplasma (Ribossomos)</p>
                            <button onclick="showFlowInfo('translation')" class="bg-green-600 text-white px-4 py-2 rounded-lg text-sm font-semibold hover:bg-green-700">Detalhes</button>
                        </div>
                    </div>
                    <div id="flow-info-panel" class="mt-6 p-4 bg-gray-50 rounded-lg border border-gray-200 min-h-[100px]">
                        <p id="flow-info-text" class="text-gray-600 leading-relaxed">Clique em "Detalhes" para ver a explicação de cada processo.</p>
                    </div>
                </div>
            </div>
        </main>
    </div>

    <script>
        const infoData = {
            nucleus: {
                title: "O Núcleo Celular",
                text: "O núcleo abriga o material genético (DNA) e atua como o centro de controle da célula, coordenando a síntese de proteínas, a divisão celular e o crescimento. É delimitado por um envelope nuclear com poros que regulam o transporte de moléculas. Dentro dele, a cromatina se condensa em cromossomos durante a divisão celular, e o nucléolo sintetiza os componentes dos ribossomos."
            },
            mitochondria: {
                title: "Mitocôndrias",
                text: "Conhecidas como as 'centrais energéticas', as mitocôndrias geram a maior parte do ATP (energia) da célula através da respiração celular. Possuem duas membranas, sendo a interna dobrada em cristas, onde ocorrem reações cruciais. Células com alta demanda energética, como as musculares, têm um número elevado de mitocôndrias."
            },
            rer: {
                title: "Retículo Endoplasmático Rugoso (RER)",
                text: "Caracterizado pela presença de ribossomos aderidos, o RER é o principal local de síntese e modificação de proteínas que serão secretadas, inseridas em membranas ou enviadas para outras organelas. Sua estrutura de cisternas achatadas é fundamental para o correto dobramento proteico."
            },
            rel: {
                title: "Retículo Endoplasmático Liso (REL)",
                text: "O REL não possui ribossomos e suas funções são diversas: síntese de lipídios e hormônios esteroides, detoxificação de drogas no fígado e armazenamento de íons cálcio, essencial para a contração muscular. É formado por uma rede de túbulos interconectados."
            },
            golgi: {
                title: "Complexo de Golgi",
                text: "Funciona como um 'centro de distribuição'. Recebe proteínas e lipídios do RE, realiza modificações pós-traducionais (como glicosilação), e os 'empacota' em vesículas para seus destinos finais, seja para secreção ou para outras partes da célula. É composto por cisternas empilhadas."
            },
            lysosome: {
                title: "Lisossomos",
                text: "São as 'usinas de reciclagem' da célula. Contêm enzimas digestivas ácidas capazes de degradar materiais externos (fagocitose) e organelas velhas (autofagia). São cruciais para a renovação celular e defesa contra patógenos. São mais abundantes em células fagocíticas, como macrófagos."
            },
            cytoskeleton: {
                title: "Citoesqueleto",
                text: "É uma rede complexa de filamentos proteicos (microtúbulos, microfilamentos de actina e filamentos intermediários) que dá forma e suporte mecânico à célula. Também participa do movimento celular, transporte intracelular e divisão celular, atuando como o 'esqueleto e músculos' da célula."
            }
        };

        const flowData = {
            replication: {
                text: "A replicação é o processo de duplicação do DNA, garantindo que cada célula-filha receba uma cópia idêntica do genoma antes da divisão celular. Ocorre no núcleo e é semiconservativa, ou seja, cada nova molécula de DNA contém uma fita antiga e uma fita nova."
            },
            transcription: {
                text: "A transcrição é a síntese de uma molécula de RNA a partir de um molde de DNA. Este processo, também no núcleo, 'copia' a informação de um gene para uma molécula de RNA mensageiro (mRNA), que então é processada e preparada para sair do núcleo."
            },
            translation: {
                text: "A tradução ocorre no citoplasma, nos ribossomos. Aqui, a sequência de informações do mRNA é 'lida' e convertida em uma sequência de aminoácidos, formando uma proteína funcional. É a etapa final onde a instrução genética se torna uma molécula ativa."
            }
        };

        const infoPanel = document.getElementById('info-panel');
        const infoTitle = document.getElementById('info-title');
        const infoText = document.getElementById('info-text');

        function showInfo(organelle) {
            const data = infoData[organelle];
            if (data) {
                infoPanel.innerHTML = `<div id="info-content" class="content-fade-in">
                    <h2 id="info-title" class="text-3xl font-bold text-yellow-800 mb-4">${data.title}</h2>
                    <p id="info-text" class="text-gray-700 leading-relaxed">${data.text}</p>
                </div>`;
            }
        }

        const views = ['view-cell', 'view-overview', 'view-flow'];
        const tabs = ['tab-cell', 'tab-overview', 'tab-flow'];

        function showView(viewId) {
            views.forEach(id => {
                document.getElementById(id).classList.add('hidden');
            });
            tabs.forEach(id => {
                document.getElementById(id).classList.remove('active-tab');
            });

            document.getElementById(viewId).classList.remove('hidden');
            document.getElementById(`tab-${viewId.split('-')[1]}`).classList.add('active-tab');
        }
        
        const flowInfoText = document.getElementById('flow-info-text');
        
        function showFlowInfo(process) {
            const data = flowData[process];
            if (data) {
                flowInfoText.textContent = data.text;
                flowInfoText.parentElement.classList.add('content-fade-in');
                flowInfoText.parentElement.addEventListener('animationend', () => {
                    flowInfoText.parentElement.classList.remove('content-fade-in');
                });
            }
        }

        document.addEventListener('DOMContentLoaded', () => {
            const ctx = document.getElementById('cytoskeletonChart').getContext('2d');
            new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: ['Filamentos Intermediários', 'Microtúbulos', 'Microfilamentos (Actina)'],
                    datasets: [{
                        label: 'Nível de Estabilidade (Arbitrário)',
                        data: [90, 50, 40],
                        backgroundColor: [
                            'rgba(161, 98, 7, 0.6)', // amber-700
                            'rgba(29, 78, 216, 0.6)', // blue-700
                            'rgba(192, 38, 211, 0.6)' // fuchsia-600
                        ],
                        borderColor: [
                            'rgba(161, 98, 7, 1)',
                            'rgba(29, 78, 216, 1)',
                            'rgba(192, 38, 211, 1)'
                        ],
                        borderWidth: 1
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    indexAxis: 'y',
                    scales: {
                        x: {
                            beginAtZero: true,
                            max: 100,
                             grid: { display: false }
                        },
                        y: {
                            grid: { color: '#e5e7eb' }
                        }
                    },
                    plugins: {
                        legend: {
                            display: false
                        },
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    let label = context.dataset.label || '';
                                    if (label) {
                                        label += ': ';
                                    }
                                    if (context.parsed.x !== null) {
                                        label += context.parsed.x + ' (Alta Estabilidade)';
                                        if(context.label.includes('Micro')) {
                                          label = label.replace('Alta Estabilidade', 'Dinâmico');
                                        }
                                    }
                                    return label;
                                }
                            }
                        }
                    }
                }
            });

            showInfo('nucleus');
        });
    </script>
</body>
</html>
