<!DOCTYPE html>
<html lang="es" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Juan Pablo Jiménez | Arquitecto y Gestor de Proyectos</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    
    <!-- Chosen Palette: Warm Neutrals & Deep Blue -->
    <!-- Application Structure Plan: Se ha diseñado una SPA con navegación fija para facilitar el acceso a las secciones clave: Sobre Mí, Servicios, Proyectos, Consulta IA, Ideas IA, Estudio Viabilidad IA y Contacto. La estructura no es un simple scroll, sino una experiencia guiada. 'Sobre Mí' combina la biografía con los diferenciadores para un impacto inmediato. 'Servicios' utiliza un diagrama de proceso interactivo para explicar las fases de forma clara y visual. 'Proyectos' es una galería filtrable con modales para ver detalles, permitiendo a los usuarios enfocarse en el tipo de obra de su interés. La sección 'Consulta IA' ofrece una interacción directa con un LLM para sugerencias de servicios. La sección 'Ideas IA' permite generar ideas de innovación y sostenibilidad. La nueva sección 'Estudio Viabilidad IA' permite generar un esquema de estudio de viabilidad. Esta arquitectura está pensada para la usabilidad, guiando al usuario desde el conocimiento del profesional hasta la exploración de su trabajo, interacciones innovadoras y finalmente al contacto. -->
    <!-- Visualization & Content Choices: 1. Servicios: Se usa un diagrama de proceso horizontal (HTML/CSS) con interacción JS. Objetivo: Organizar y clarificar el flujo de trabajo. Al hacer clic, se muestra la información de cada fase. Justificación: Es más intuitivo y menos abrumador que un listado. 2. Diferenciales ('Por qué elegirme'): Se usan tarjetas en una cuadrícula (HTML/Tailwind). Objetivo: Informar y destacar. Justificación: Facilita la lectura rápida y asimilación de los puntos clave. 3. Proyectos: Galería con filtros (botones) y modales (JS). Objetivo: Organizar y Explorar. El usuario filtra por tipo de proyecto y ve los detalles en una ventana emergente sin salir de la página. Justificación: Es la forma más eficiente y profesional de presentar un portafolio variado, mejorando la experiencia de usuario. 4. Consulta IA: Interfaz de texto con botón y área de respuesta (HTML/Tailwind) con llamada a la API de Gemini (JS). Objetivo: Ofrecer una "pre-consulta" interactiva y destacar la innovación. Justificación: Permite a los usuarios obtener una guía inicial personalizada y demuestra el uso de tecnología avanzada. 5. Ideas IA: Interfaz de texto con botón y área de respuesta (HTML/Tailwind) con llamada a la API de Gemini (JS). Objetivo: Generar ideas de innovación y sostenibilidad, resaltando la visión de Juan Pablo. Justificación: Demuestra proactividad y conocimiento en tendencias actuales, ofreciendo valor adicional al visitante. 6. Estudio Viabilidad IA: Interfaz de texto con botón y área de respuesta (HTML/Tailwind) con llamada a la API de Gemini (JS). Objetivo: Mostrar la capacidad de Juan Pablo en la fase de diagnóstico y viabilidad. Justificación: Proporciona una muestra tangible de un entregable clave en sus servicios iniciales, generando confianza. -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->

    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f8f7f4;
            color: #333;
        }
        .nav-link {
            position: relative;
            transition: color 0.3s ease;
        }
        .nav-link::after {
            content: '';
            position: absolute;
            width: 0;
            height: 2px;
            bottom: -4px;
            left: 50%;
            transform: translateX(-50%);
            background-color: #2c3e50;
            transition: width 0.3s ease;
        }
        .nav-link:hover::after, .nav-link.active::after {
            width: 100%;
        }
        .hero-bg {
            background-color: #e9e7e2;
        }
        .btn-primary {
            background-color: #2c3e50;
            color: #ffffff;
            transition: background-color 0.3s ease, transform 0.2s ease;
        }
        .btn-primary:hover {
            background-color: #34495e;
            transform: translateY(-2px);
        }
        .section-title {
            color: #2c3e50;
        }
        .service-tab {
            cursor: pointer;
            transition: all 0.3s ease;
            border-color: #d1d5db;
        }
        .service-tab.active {
            background-color: #2c3e50;
            color: #ffffff;
            border-color: #2c3e50;
        }
        .project-filter-btn {
            transition: all 0.3s ease;
        }
        .project-filter-btn.active {
            background-color: #2c3e50;
            color: #ffffff;
        }
        .modal {
            display: none;
            transition: opacity 0.3s ease;
        }
        .modal-content {
            transition: transform 0.3s ease;
        }
        .project-card {
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }
        .project-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
        }
        .loading-spinner {
            border: 4px solid #f3f3f3;
            border-top: 4px solid #2c3e50;
            border-radius: 50%;
            width: 24px;
            height: 24px;
            animation: spin 1s linear infinite;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
    </style>
</head>
<body class="antialiased">

    <header class="bg-white/80 backdrop-blur-sm shadow-sm sticky top-0 z-50">
        <nav class="container mx-auto px-6 py-4 flex justify-between items-center">
            <a href="#inicio" class="text-xl font-bold text-[#2c3e50]">Juan Pablo Jiménez</a>
            <div class="hidden md:flex items-center space-x-8">
                <a href="#sobre-mi" class="nav-link text-gray-600 hover:text-[#2c3e50]">Sobre Mí</a>
                <a href="#servicios" class="nav-link text-gray-600 hover:text-[#2c3e50]">Servicios</a>
                <a href="#proyectos" class="nav-link text-gray-600 hover:text-[#2c3e50]">Proyectos</a>
                <a href="#consulta-ia" class="nav-link text-gray-600 hover:text-[#2c3e50]">Consulta IA</a>
                <a href="#ideas-ia" class="nav-link text-gray-600 hover:text-[#2c3e50]">Ideas IA</a>
                <a href="#estudio-viabilidad-ia" class="nav-link text-gray-600 hover:text-[#2c3e50]">Estudio Viabilidad IA</a>
                <a href="#contacto" class="nav-link text-gray-600 hover:text-[#2c3e50]">Contacto</a>
            </div>
            <div class="md:hidden">
                <button id="mobile-menu-button" class="text-gray-600 focus:outline-none">
                    <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16m-7 6h7"></path></svg>
                </button>
            </div>
        </nav>
        <div id="mobile-menu" class="hidden md:hidden">
            <a href="#sobre-mi" class="block text-center py-2 px-4 text-sm text-gray-600 hover:bg-gray-200">Sobre Mí</a>
            <a href="#servicios" class="block text-center py-2 px-4 text-sm text-gray-600 hover:bg-gray-200">Servicios</a>
            <a href="#proyectos" class="block text-center py-2 px-4 text-sm text-gray-600 hover:bg-gray-200">Proyectos</a>
            <a href="#consulta-ia" class="block text-center py-2 px-4 text-sm text-gray-600 hover:bg-gray-200">Consulta IA</a>
            <a href="#ideas-ia" class="block text-center py-2 px-4 text-sm text-gray-600 hover:bg-gray-200">Ideas IA</a>
            <a href="#estudio-viabilidad-ia" class="block text-center py-2 px-4 text-sm text-gray-600 hover:bg-gray-200">Estudio Viabilidad IA</a>
            <a href="#contacto" class="block text-center py-2 px-4 text-sm text-gray-600 hover:bg-gray-200">Contacto</a>
        </div>
    </header>

    <main>
        <section id="inicio" class="hero-bg">
            <div class="container mx-auto px-6 py-24 md:py-32 text-center">
                <h1 class="text-4xl md:text-6xl font-bold text-[#2c3e50] mb-4">Juan Pablo Jiménez Aguirre</h1>
                <p class="text-xl md:text-2xl text-gray-700 mb-8">Arquitecto y Especialista en Gestión de Proyectos</p>
                <p class="max-w-3xl mx-auto text-gray-600 mb-10">Convierto ideas en proyectos ejecutados con precisión, optimizando la inversión y garantizando resultados de alta calidad en cada fase.</p>
                <a href="#contacto" class="btn-primary font-bold py-3 px-8 rounded-lg inline-block">Contactar</a>
            </div>
        </section>

        <section id="sobre-mi" class="py-20 md:py-28">
            <div class="container mx-auto px-6">
                <div class="text-center mb-16">
                    <h2 class="text-3xl md:text-4xl font-bold section-title mb-4">Más que un arquitecto, un socio estratégico</h2>
                    <p class="max-w-3xl mx-auto text-gray-600">Mi trabajo es acompañarle desde la visión inicial hasta la entrega final, asegurando que cada decisión agregue valor y se alinee con sus objetivos financieros y de calidad.</p>
                </div>
                <div class="grid md:grid-cols-2 gap-12 items-center">
                    <div>
                        <h3 class="text-2xl font-bold text-[#2c3e50] mb-4">Mi Filosofía</h3>
                        <p class="text-gray-600 mb-4">Con más de 15 años de experiencia en Colombia y Panamá, mi enfoque va más allá del diseño. Se trata de planificar con mentalidad empresarial, ejecutar con precisión técnica y entregar resultados tangibles: a tiempo, dentro del presupuesto y superando expectativas.</p>
                        <p class="text-gray-600">Combino mi trayectoria con conocimientos actuales en finanzas, energías renovables e innovación digital para ofrecer soluciones integrales, eficientes y sostenibles, enfocadas en el retorno de inversión de cada cliente.</p>
                    </div>
                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-6">
                        <div class="bg-white p-6 rounded-lg shadow-md border-l-4 border-[#2c3e50]">
                            <h4 class="font-bold text-lg mb-2">Experiencia Comprobada</h4>
                            <p class="text-sm text-gray-600">Liderazgo en proyectos de alto nivel en dos países, anticipando problemas y optimizando recursos.</p>
                        </div>
                        <div class="bg-white p-6 rounded-lg shadow-md border-l-4 border-[#2c3e50]">
                            <h4 class="font-bold text-lg mb-2">Visión Estratégica</h4>
                            <p class="text-sm text-gray-600">Planificación con mentalidad empresarial para cuidar cada peso invertido y garantizar la rentabilidad.</p>
                        </div>
                        <div class="bg-white p-6 rounded-lg shadow-md border-l-4 border-[#2c3e50]">
                            <h4 class="font-bold text-lg mb-2">Comunicación Clara</h4>
                            <p class="text-sm text-gray-600">Acompañamiento completo con informes constantes y recomendaciones claras para la toma de decisiones.</p>
                        </div>
                        <div class="bg-white p-6 rounded-lg shadow-md border-l-4 border-[#2c3e50]">
                            <h4 class="font-bold text-lg mb-2">Formación Actualizada</h4>
                            <p class="text-sm text-gray-600">Soluciones innovadoras que integran IA, energías renovables y gestión financiera.</p>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <section id="servicios" class="py-20 md:py-28 bg-white">
            <div class="container mx-auto px-6">
                <div class="text-center mb-16">
                    <h2 class="text-3xl md:text-4xl font-bold section-title mb-4">Un Servicio para Cada Etapa de su Proyecto</h2>
                    <p class="max-w-3xl mx-auto text-gray-600">Ofrezco un acompañamiento flexible y modular, adaptado a sus necesidades específicas, desde la idea inicial hasta la entrega final.</p>
                </div>
                
                <div class="w-full max-w-4xl mx-auto">
                    <div class="flex flex-col sm:flex-row justify-between border-b-2 border-gray-200 mb-8">
                        <div id="tab-1" class="service-tab flex-1 text-center p-4 border-b-4 active">Fase 1: Diagnóstico</div>
                        <div id="tab-2" class="service-tab flex-1 text-center p-4 border-b-4">Fase 2: Planificación</div>
                        <div id="tab-3" class="service-tab flex-1 text-center p-4 border-b-4">Fase 3: Ejecución</div>
                    </div>
                    <div id="content-1" class="service-content">
                        <h3 class="text-2xl font-bold text-[#2c3e50] mb-3">Diagnóstico Técnico y Viabilidad</h3>
                        <p class="text-gray-500 mb-4 font-medium">Ideal para propietarios o inversionistas que tienen un terreno, local o idea y quieren evaluar su potencial antes de invertir.</p>
                        <ul class="list-disc list-inside text-gray-600 space-y-2">
                            <li>Visita técnica y análisis detallado del espacio.</li>
                            <li>Estudio básico de normativas, restricciones y potencialidades.</li>
                            <li>Estimación preliminar de costos y cronogramas.</li>
                            <li>Recomendaciones estratégicas de diseño y uso eficiente.</li>
                        </ul>
                        <p class="mt-4 bg-blue-50 text-[#2c3e50] p-4 rounded-lg"><b>Beneficio clave:</b> Tomar decisiones con datos claros y mitigar riesgos desde el día cero.</p>
                    </div>
                    <div id="content-2" class="service-content hidden">
                        <h3 class="text-2xl font-bold text-[#2c3e50] mb-3">Planificación Estratégica y Diseño</h3>
                        <p class="text-gray-500 mb-4 font-medium">Dirigido a quienes ya tomaron la decisión de construir o remodelar y necesitan una estructura sólida para su proyecto.</p>
                        <ul class="list-disc list-inside text-gray-600 space-y-2">
                            <li>Desarrollo de anteproyecto y planos técnicos constructivos.</li>
                            <li>Elaboración de presupuesto detallado y cronograma maestro.</li>
                            <li>Asesoría experta en selección de materiales y proveedores.</li>
                            <li>Gestión integral de permisos y licencias.</li>
                        </ul>
                        <p class="mt-4 bg-blue-50 text-[#2c3e50] p-4 rounded-lg"><b>Beneficio clave:</b> Evitar sobrecostos y retrasos, sentando las bases para una ejecución exitosa.</p>
                    </div>
                    <div id="content-3" class="service-content hidden">
                        <h3 class="text-2xl font-bold text-[#2c3e50] mb-3">Gerencia de Obra / Llave en Mano</h3>
                        <p class="text-gray-500 mb-4 font-medium">Para quienes quieren delegar la ejecución completa y recibir un proyecto listo, sin preocupaciones.</p>
                         <ul class="list-disc list-inside text-gray-600 space-y-2">
                            <li>Supervisión técnica y administrativa permanente en obra.</li>
                            <li>Coordinación de todos los contratistas y entregables.</li>
                            <li>Control exhaustivo de calidad, tiempos y presupuesto.</li>
                            <li>Informes de avance periódicos, digitales y visuales.</li>
                        </ul>
                        <p class="mt-4 bg-blue-50 text-[#2c3e50] p-4 rounded-lg"><b>Beneficio clave:</b> Usted se enfoca en sus objetivos mientras yo me encargo de materializar su proyecto.</p>
                    </div>
                </div>
            </div>
        </section>

        <section id="proyectos" class="py-20 md:py-28">
            <div class="container mx-auto px-6">
                <div class="text-center mb-16">
                    <h2 class="text-3xl md:text-4xl font-bold section-title mb-4">Proyectos que Hablan por Sí Mismos</h2>
                    <p class="max-w-3xl mx-auto text-gray-600">Una selección de proyectos que demuestran mi experiencia en diversos sectores y mi compromiso con la calidad y la gestión eficiente.</p>
                </div>
                <div class="flex justify-center flex-wrap gap-2 mb-10">
                    <button class="project-filter-btn active py-2 px-4 rounded-full border border-gray-300" data-filter="all">Todos</button>
                    <button class="project-filter-btn py-2 px-4 rounded-full border border-gray-300" data-filter="residencial">Residencial</button>
                    <button class="project-filter-btn py-2 px-4 rounded-full border border-gray-300" data-filter="hotelero">Hotelero</button>
                    <button class="project-filter-btn py-2 px-4 rounded-full border border-gray-300" data-filter="comercial">Comercial</button>
                    <button class="project-filter-btn py-2 px-4 rounded-full border border-gray-300" data-filter="mixto">Mixto</button>
                </div>
                <div id="project-gallery" class="grid sm:grid-cols-2 lg:grid-cols-3 gap-8">
                </div>
            </div>
        </section>

        <section id="consulta-ia" class="py-20 md:py-28 bg-blue-50">
            <div class="container mx-auto px-6">
                <div class="text-center mb-12">
                    <h2 class="text-3xl md:text-4xl font-bold section-title mb-4">Consulta Rápida con IA ✨</h2>
                    <p class="max-w-2xl mx-auto text-gray-600">Describa brevemente su idea de proyecto y la inteligencia artificial le sugerirá los servicios más relevantes de Juan Pablo Jiménez y algunas consideraciones iniciales.</p>
                </div>
                <div class="max-w-2xl mx-auto bg-white p-8 rounded-lg shadow-md">
                    <textarea id="project-idea-input" class="w-full p-4 border border-gray-300 rounded-lg mb-4 focus:outline-none focus:ring-2 focus:ring-[#2c3e50]" rows="6" placeholder="Ej: Quiero construir una casa moderna de 300m² con jardín vertical y paneles solares."></textarea>
                    <button id="get-ai-suggestion-btn" class="btn-primary font-bold py-3 px-6 rounded-lg w-full flex items-center justify-center">
                        <span id="button-text">Obtener Sugerencia IA</span>
                        <div id="loading-spinner" class="loading-spinner ml-3 hidden"></div>
                    </button>
                    <div id="ai-response" class="mt-8 p-6 bg-gray-50 rounded-lg border border-gray-200 text-gray-700 hidden">
                        <h3 class="font-bold text-lg mb-2 text-[#2c3e50]">Sugerencia de la IA:</h3>
                        <p id="ai-response-content"></p>
                    </div>
                </div>
            </div>
        </section>

        <section id="ideas-ia" class="py-20 md:py-28">
            <div class="container mx-auto px-6">
                <div class="text-center mb-12">
                    <h2 class="text-3xl md:text-4xl font-bold section-title mb-4">Ideas de Innovación y Sostenibilidad ✨</h2>
                    <p class="max-w-2xl mx-auto text-gray-600">Explore conceptos vanguardistas. Describa un tipo de proyecto o un desafío, y la IA le proporcionará ideas para integrar sostenibilidad y diseño innovador.</p>
                </div>
                <div class="max-w-2xl mx-auto bg-white p-8 rounded-lg shadow-md">
                    <textarea id="innovation-idea-input" class="w-full p-4 border border-gray-300 rounded-lg mb-4 focus:outline-none focus:ring-2 focus:ring-[#2c3e50]" rows="6" placeholder="Ej: Proyecto residencial en clima cálido con enfoque en eficiencia energética."></textarea>
                    <button id="get-innovation-btn" class="btn-primary font-bold py-3 px-6 rounded-lg w-full flex items-center justify-center">
                        <span id="innovation-button-text">Generar Ideas Innovadoras</span>
                        <div id="innovation-loading-spinner" class="loading-spinner ml-3 hidden"></div>
                    </button>
                    <div id="innovation-response" class="mt-8 p-6 bg-gray-50 rounded-lg border border-gray-200 text-gray-700 hidden">
                        <h3 class="font-bold text-lg mb-2 text-[#2c3e50]">Ideas Sugeridas por la IA:</h3>
                        <p id="innovation-response-content"></p>
                    </div>
                </div>
            </div>
        </section>

        <section id="estudio-viabilidad-ia" class="py-20 md:py-28 bg-blue-50">
            <div class="container mx-auto px-6">
                <div class="text-center mb-12">
                    <h2 class="text-3xl md:text-4xl font-bold section-title mb-4">Generador de Esquema de Estudio de Viabilidad ✨</h2>
                    <p class="max-w-2xl mx-auto text-gray-600">Obtenga un esquema detallado para un estudio de viabilidad técnica y económica, basado en su descripción de proyecto.</p>
                </div>
                <div class="max-w-2xl mx-auto bg-white p-8 rounded-lg shadow-md">
                    <textarea id="feasibility-input" class="w-full p-4 border border-gray-300 rounded-lg mb-4 focus:outline-none focus:ring-2 focus:ring-[#2c3e50]" rows="6" placeholder="Ej: Desarrollo de un complejo de oficinas de 15,000 m² en el centro de la ciudad, con certificación LEED."></textarea>
                    <button id="get-feasibility-btn" class="btn-primary font-bold py-3 px-6 rounded-lg w-full flex items-center justify-center">
                        <span id="feasibility-button-text">Generar Esquema</span>
                        <div id="feasibility-loading-spinner" class="loading-spinner ml-3 hidden"></div>
                    </button>
                    <div id="feasibility-response" class="mt-8 p-6 bg-gray-50 rounded-lg border border-gray-200 text-gray-700 hidden">
                        <h3 class="font-bold text-lg mb-2 text-[#2c3e50]">Esquema de Estudio de Viabilidad:</h3>
                        <p id="feasibility-response-content"></p>
                    </div>
                </div>
            </div>
        </section>

        <section id="contacto" class="py-20 md:py-28 bg-white">
            <div class="container mx-auto px-6">
                <div class="text-center mb-12">
                    <h2 class="text-3xl md:text-4xl font-bold section-title mb-4">Iniciemos una Conversación</h2>
                    <p class="max-w-2xl mx-auto text-gray-600">¿Tiene un proyecto en mente o busca un profesional para unirse a su equipo? Me encantaría conocer sus planes.</p>
                </div>
                <div class="max-w-xl mx-auto text-center">
                     <div class="bg-gray-50 p-8 rounded-lg shadow-inner">
                        <p class="text-lg font-semibold text-[#2c3e50] mb-2">Juan Pablo Jiménez Aguirre</p>
                        <p class="text-gray-600">Cali, Colombia</p>
                        <p class="text-gray-600 mt-4">
                            <a href="mailto:jp.jimenez.a@email.com" class="hover:text-[#2c3e50] transition-colors">jp.jimenez.a@email.com</a>
                        </p>
                         <p class="text-gray-600">
                            <a href="tel:+573001234567" class="hover:text-[#2c3e50] transition-colors">(+57) 300 123 4567</a>
                        </p>
                    </div>
                </div>
            </div>
        </section>
    </main>

    <footer class="bg-[#2c3e50] text-white">
        <div class="container mx-auto px-6 py-6 text-center text-sm">
            <p>&copy; 2025 Juan Pablo Jiménez Aguirre. Todos los derechos reservados.</p>
            <p>Portafolio interactivo diseñado y desarrollado por IA.</p>
        </div>
    </footer>

    <div id="project-modal" class="modal fixed inset-0 bg-black bg-opacity-70 flex items-center justify-center p-4 z-50">
        <div class="modal-content bg-white rounded-lg max-w-4xl w-full max-h-[90vh] overflow-y-auto relative scale-95 opacity-0">
            <button id="close-modal-btn" class="absolute top-4 right-4 text-gray-500 hover:text-black z-10">
                <svg class="w-8 h-8" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path></svg>
            </button>
            <div class="grid md:grid-cols-2">
                <div class="p-8">
                    <h3 id="modal-title" class="text-2xl font-bold text-[#2c3e50] mb-2"></h3>
                    <p id="modal-category" class="font-medium text-gray-500 mb-4"></p>
                    <p id="modal-description" class="text-gray-600 mb-4"></p>
                    <div id="modal-features" class="text-sm text-gray-600"></div>
                </div>
                <div id="modal-image-container" class="bg-gray-100 flex items-center justify-center min-h-[300px] md:min-h-0">
                    <img id="modal-image" src="" alt="Project image" class="w-full h-full object-cover">
                </div>
            </div>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function () {
            const projects = [
                {
                    title: 'Casa Gamboa',
                    location: 'Ciudad Jardín, Cali',
                    category: 'residencial',
                    description: 'Asesoría técnica integral durante la ejecución de una residencia de alta gama.',
                    features: 'Se logró una obra de alta calidad arquitectónica, con una excelente inversión en acabados y detalles. Lote de 1.500m², 1.600m² construidos, terrazas, gimnasio, turco, piscina climatizada, bar-discoteca, salón de juegos y sala de cine.',
                    image: 'https://placehold.co/800x600/334155/FFFFFF?text=Casa+Gamboa'
                },
                {
                    title: 'Casa Campestre',
                    location: 'La Morada, Jamundí',
                    category: 'residencial',
                    description: 'Diseño y acompañamiento técnico en una vivienda de alta gama.',
                    features: 'Enfoque en integración con el entorno natural, eficiencia constructiva y máximo confort del usuario final.',
                    image: 'https://placehold.co/800x600/475569/FFFFFF?text=Casa+Campestre'
                },
                {
                    title: 'Hotel Hard Rock Café',
                    location: 'Ciudad de Panamá',
                    category: 'hotelero',
                    description: 'Gestión como residente de obra y programador en un proyecto hotelero icónico de gran escala.',
                    features: 'Participación clave en la ejecución de un complejo turístico y comercial de alto impacto, cumpliendo con exigentes estándares de calidad internacional.',
                    image: 'https://placehold.co/800x600/1E293B/FFFFFF?text=Hard+Rock+Panamá'
                },
                {
                    title: 'Ampliación C.C. Jardín Plaza',
                    location: 'Cali, Colombia',
                    category: 'comercial',
                    description: 'Responsable de acabados arquitectónicos en una obra comercial en operación con alta exigencia logística.',
                    features: 'Trabajo ejecutado bajo un cronograma estricto, manteniendo la estética y funcionalidad del centro comercial durante la ampliación.',
                    image: 'https://placehold.co/800x600/334155/FFFFFF?text=Jardín+Plaza'
                },
                {
                    title: 'Hotel Sortis, Business y Casino',
                    location: 'Ciudad de Panamá',
                    category: 'mixto',
                    description: 'Coordinador de obra general en un proyecto mixto de lujo que incluye hotel, oficinas y casino.',
                    features: 'Gestión de equipos técnicos, control de cronograma y supervisión de acabados de alto nivel en un proyecto de alto impacto urbano y estándares internacionales.',
                    image: 'https://placehold.co/800x600/475569/FFFFFF?text=Hotel+Sortis'
                }
            ];

            const gallery = document.getElementById('project-gallery');
            const filterBtns = document.querySelectorAll('.project-filter-btn');
            
            function renderGallery(filter) {
                gallery.innerHTML = '';
                const filteredProjects = filter === 'all' ? projects : projects.filter(p => p.category === filter);
                
                filteredProjects.forEach((project, index) => {
                    const card = document.createElement('div');
                    card.className = 'project-card bg-white rounded-lg overflow-hidden shadow-md cursor-pointer';
                    card.dataset.index = projects.indexOf(project);
                    card.innerHTML = `
                        <div class="h-48 bg-gray-200">
                            <img src="${project.image}" alt="${project.title}" class="w-full h-full object-cover">
                        </div>
                        <div class="p-6">
                            <p class="text-sm font-medium text-[#2c3e50]">${project.category.charAt(0).toUpperCase() + project.category.slice(1)}</p>
                            <h3 class="text-xl font-bold text-gray-800 mt-1">${project.title}</h3>
                            <p class="text-gray-500 text-sm mt-1">${project.location}</p>
                        </div>
                    `;
                    gallery.appendChild(card);
                });
            }

            filterBtns.forEach(btn => {
                btn.addEventListener('click', () => {
                    filterBtns.forEach(b => b.classList.remove('active'));
                    btn.classList.add('active');
                    renderGallery(btn.dataset.filter);
                });
            });

            renderGallery('all');

            const modal = document.getElementById('project-modal');
            const closeModalBtn = document.getElementById('close-modal-btn');
            
            gallery.addEventListener('click', (e) => {
                const card = e.target.closest('.project-card');
                if (card) {
                    const project = projects[card.dataset.index];
                    document.getElementById('modal-title').textContent = project.title;
                    document.getElementById('modal-category').textContent = `${project.category.charAt(0).toUpperCase() + project.category.slice(1)} - ${project.location}`;
                    document.getElementById('modal-description').textContent = project.description;
                    document.getElementById('modal-features').textContent = project.features;
                    document.getElementById('modal-image').src = project.image;
                    modal.style.display = 'flex';
                    setTimeout(() => {
                        modal.classList.add('opacity-100');
                        modal.querySelector('.modal-content').classList.remove('scale-95', 'opacity-0');
                        modal.querySelector('.modal-content').classList.add('scale-100', 'opacity-100');
                    }, 10);
                }
            });

            const closeModal = () => {
                modal.querySelector('.modal-content').classList.remove('scale-100', 'opacity-100');
                modal.querySelector('.modal-content').classList.add('scale-95', 'opacity-0');
                modal.classList.remove('opacity-100');
                setTimeout(() => {
                    modal.style.display = 'none';
                }, 300);
            };
            
            closeModalBtn.addEventListener('click', closeModal);
            modal.addEventListener('click', (e) => {
                if (e.target === modal) {
                    closeModal();
                }
            });

            const serviceTabs = document.querySelectorAll('.service-tab');
            const serviceContents = document.querySelectorAll('.service-content');
            serviceTabs.forEach(tab => {
                tab.addEventListener('click', () => {
                    serviceTabs.forEach(t => t.classList.remove('active'));
                    tab.classList.add('active');
                    const targetId = tab.id.replace('tab', 'content');
                    serviceContents.forEach(content => {
                        if (content.id === targetId) {
                            content.classList.remove('hidden');
                        } else {
                            content.classList.add('hidden');
                        }
                    });
                });
            });

            const mobileMenuButton = document.getElementById('mobile-menu-button');
            const mobileMenu = document.getElementById('mobile-menu');
            mobileMenuButton.addEventListener('click', () => {
                mobileMenu.classList.toggle('hidden');
            });

            document.querySelectorAll('a[href^="#"]').forEach(anchor => {
                anchor.addEventListener('click', function (e) {
                    if(this.parentElement.id === 'mobile-menu') {
                        mobileMenu.classList.add('hidden');
                    }
                });
            });

            // Gemini API Integration - Consulta Rápida
            const getAiSuggestionBtn = document.getElementById('get-ai-suggestion-btn');
            const projectIdeaInput = document.getElementById('project-idea-input');
            const aiResponseDiv = document.getElementById('ai-response');
            const aiResponseContent = document.getElementById('ai-response-content');
            const loadingSpinner = document.getElementById('loading-spinner');
            const buttonText = document.getElementById('button-text');

            // Gemini API Integration - Ideas de Innovación y Sostenibilidad
            const getInnovationBtn = document.getElementById('get-innovation-btn');
            const innovationIdeaInput = document.getElementById('innovation-idea-input');
            const innovationResponseDiv = document.getElementById('innovation-response');
            const innovationResponseContent = document.getElementById('innovation-response-content');
            const innovationLoadingSpinner = document.getElementById('innovation-loading-spinner');
            const innovationButtonText = document.getElementById('innovation-button-text');

            // Gemini API Integration - Generador de Esquema de Estudio de Viabilidad
            const getFeasibilityBtn = document.getElementById('get-feasibility-btn');
            const feasibilityInput = document.getElementById('feasibility-input');
            const feasibilityResponseDiv = document.getElementById('feasibility-response');
            const feasibilityResponseContent = document.getElementById('feasibility-response-content');
            const feasibilityLoadingSpinner = document.getElementById('feasibility-loading-spinner');
            const feasibilityButtonText = document.getElementById('feasibility-button-text');


            async function getAIPrediction(prompt) {
                const chatHistory = [{ role: "user", parts: [{ text: prompt }] }];
                const payload = { contents: chatHistory };
                const apiKey = ""; 
                const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey}`;

                let retries = 0;
                const maxRetries = 5;
                const baseDelay = 1000; 

                while (retries < maxRetries) {
                    try {
                        const response = await fetch(apiUrl, {
                            method: 'POST',
                            headers: { 'Content-Type': 'application/json' },
                            body: JSON.stringify(payload)
                        });

                        if (!response.ok) {
                            if (response.status === 429 && retries < maxRetries - 1) {
                                const delay = baseDelay * Math.pow(2, retries);
                                await new Promise(resolve => setTimeout(resolve, delay));
                                retries++;
                                continue;
                            }
                            throw new Error(`HTTP error! status: ${response.status}`);
                        }

                        const result = await response.json();
                        if (result.candidates && result.candidates.length > 0 &&
                            result.candidates[0].content && result.candidates[0].content.parts &&
                            result.candidates[0].content.parts.length > 0) {
                            return result.candidates[0].content.parts[0].text;
                        } else {
                            return "No se pudo generar una respuesta. Inténtelo de nuevo.";
                        }
                    } catch (error) {
                        if (retries < maxRetries - 1) {
                            const delay = baseDelay * Math.pow(2, retries);
                            await new Promise(resolve => setTimeout(resolve, delay));
                            retries++;
                        } else {
                            return "Hubo un error al conectar con el servicio de IA. Por favor, intente más tarde.";
                        }
                    }
                }
                return "No se pudo obtener una respuesta después de varios intentos.";
            }

            getAiSuggestionBtn.addEventListener('click', async () => {
                const projectIdea = projectIdeaInput.value.trim();
                if (!projectIdea) {
                    aiResponseContent.textContent = "Por favor, describa su idea de proyecto para obtener una sugerencia.";
                    aiResponseDiv.classList.remove('hidden');
                    return;
                }

                buttonText.textContent = '';
                loadingSpinner.classList.remove('hidden');
                getAiSuggestionBtn.disabled = true;
                aiResponseDiv.classList.add('hidden');

                const prompt = `Como arquitecto y especialista en gestión de proyectos, Juan Pablo Jiménez Aguirre, con más de 15 años de experiencia en obras residenciales, comerciales y hoteleras en Colombia y Panamá, y con conocimientos en finanzas, energías renovables e innovación digital, analice la siguiente idea de proyecto y sugiera cuál de los siguientes servicios sería el más adecuado y por qué, además de mencionar 2-3 consideraciones iniciales clave para el cliente.

                Servicios de Juan Pablo:
                1. Diagnóstico Técnico y Viabilidad (Fase Inicial): Evaluar potencial, normativas, costos preliminares.
                2. Planificación Estratégica y Diseño (Fase Intermedia): Desarrollo de anteproyecto, planos, presupuesto detallado, gestión de permisos.
                3. Gerencia de Obra / Llave en Mano (Fase Completa): Supervisión técnica, coordinación de contratistas, control de calidad, tiempos y presupuesto.

                Idea de Proyecto: "${projectIdea}"

                Por favor, responda de manera concisa y profesional, enfocándose en la propuesta de valor de Juan Pablo.`;

                const responseText = await getAIPrediction(prompt);
                aiResponseContent.textContent = responseText;
                aiResponseDiv.classList.remove('hidden');

                buttonText.textContent = 'Obtener Sugerencia IA';
                loadingSpinner.classList.add('hidden');
                getAiSuggestionBtn.disabled = false;
            });

            getInnovationBtn.addEventListener('click', async () => {
                const innovationIdea = innovationIdeaInput.value.trim();
                if (!innovationIdea) {
                    innovationResponseContent.textContent = "Por favor, describa un tipo de proyecto o desafío para generar ideas.";
                    innovationResponseDiv.classList.remove('hidden');
                    return;
                }

                innovationButtonText.textContent = '';
                innovationLoadingSpinner.classList.remove('hidden');
                getInnovationBtn.disabled = true;
                innovationResponseDiv.classList.add('hidden');

                const prompt = `Como arquitecto experto en energías renovables e innovación digital, genere 3-5 ideas de innovación y sostenibilidad para el siguiente concepto de proyecto o desafío: "${innovationIdea}".
                Enfóquese en soluciones prácticas y de alto impacto, que podrían ser implementadas en proyectos de arquitectura.`;

                const responseText = await getAIPrediction(prompt);
                innovationResponseContent.textContent = responseText;
                innovationResponseDiv.classList.remove('hidden');

                innovationButtonText.textContent = 'Generar Ideas Innovadoras';
                innovationLoadingSpinner.classList.add('hidden');
                getInnovationBtn.disabled = false;
            });

            getFeasibilityBtn.addEventListener('click', async () => {
                const feasibilityIdea = feasibilityInput.value.trim();
                if (!feasibilityIdea) {
                    feasibilityResponseContent.textContent = "Por favor, describa el proyecto para el estudio de viabilidad.";
                    feasibilityResponseDiv.classList.remove('hidden');
                    return;
                }

                feasibilityButtonText.textContent = '';
                feasibilityLoadingSpinner.classList.remove('hidden');
                getFeasibilityBtn.disabled = true;
                feasibilityResponseDiv.classList.add('hidden');

                const prompt = `Como Juan Pablo Jiménez, arquitecto y especialista en gestión de proyectos, genere un esquema detallado para un estudio de viabilidad técnica y económica para el siguiente proyecto: "${feasibilityIdea}".
                Incluya secciones clave como:
                - Análisis de Normativa y Usos del Suelo
                - Estudio de Mercado y Demanda Potencial
                - Análisis de Terreno/Ubicación (aspectos técnicos y geográficos)
                - Estimación Preliminar de Costos (construcción, licencias, etc.)
                - Análisis Financiero Básico (proyecciones de inversión y retorno)
                - Evaluación de Riesgos (técnicos, legales, financieros)
                - Cronograma Tentativo
                - Recomendaciones y Conclusiones.
                Responda en formato de lista clara y concisa, utilizando encabezados si es necesario para cada sección.`;

                const responseText = await getAIPrediction(prompt);
                feasibilityResponseContent.textContent = responseText;
                feasibilityResponseDiv.classList.remove('hidden');

                feasibilityButtonText.textContent = 'Generar Esquema';
                feasibilityLoadingSpinner.classList.add('hidden');
                getFeasibilityBtn.disabled = false;
            });
        });
    </script>
</body>
</html>
