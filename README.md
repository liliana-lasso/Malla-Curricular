document.addEventListener('DOMContentLoaded', () => {

    // 1. LISTA DE RAMOS Y SUS REQUISITOS
    // Se crea un 'id' único para cada ramo para manejar las dependencias.
    const ramosData = [
        // Semestre 1
        { id: 'calculo', nombre: 'Cálculo', semestre: 1, prerequisitos: [] },
        { id: 'fund-eco-com', nombre: 'Fundamentos de Economía y Comercio', semestre: 1, prerequisitos: [] },
        { id: 'intro-derecho', nombre: 'Introducción al Derecho y Constitución Política', semestre: 1, prerequisitos: [] },
        { id: 'proc-admin', nombre: 'Proceso Administrativo', semestre: 1, prerequisitos: [] },
        { id: 'taller-hab-info', nombre: 'Taller de Habilidades Informáticas para la Gestión', semestre: 1, prerequisitos: [] },
        { id: 'fund-conta', nombre: 'Fundamentos de Contabilidad', semestre: 1, prerequisitos: [] },
        
        // Semestre 2
        { id: 'algebra-lineal', nombre: 'Álgebra Lineal', semestre: 2, prerequisitos: ['calculo'] },
        { id: 'sist-info-ger', nombre: 'Sistemas de Información Gerencial', semestre: 2, prerequisitos: ['taller-hab-info'] },
        { id: 'leg-comercial', nombre: 'Legislación Comercial', semestre: 2, prerequisitos: ['intro-derecho'] },
        { id: 'macro-coyuntura', nombre: 'Macroeconomía y Coyuntura', semestre: 2, prerequisitos: ['fund-eco-com'] },
        { id: 'reg-aduanero', nombre: 'Régimen Aduanero', semestre: 2, prerequisitos: ['intro-derecho'] },
        { id: 'comp-textos', nombre: 'Comprensión y Producción de Textos Académicos Generales', semestre: 2, prerequisitos: [] },
        { id: 'lenguas-1', nombre: 'Lenguas con Fines Generales y Académicos I', semestre: 2, prerequisitos: [] },

        // Semestre 3
        { id: 'est-1', nombre: 'Estadística I para las Ciencias de la Administración', semestre: 3, prerequisitos: ['algebra-lineal'] },
        { id: 'mat-fin', nombre: 'Matemática para Finanzas', semestre: 3, prerequisitos: ['algebra-lineal'] },
        { id: 'leg-laboral', nombre: 'Legislación Laboral', semestre: 3, prerequisitos: ['leg-comercial'] },
        { id: 'eco-empresa', nombre: 'Economía de Empresa', semestre: 3, prerequisitos: ['macro-coyuntura'] },
        { id: 'importaciones', nombre: 'Importaciones', semestre: 3, prerequisitos: ['reg-aduanero'] },
        { id: 'lenguas-2', nombre: 'Lenguas con Fines Generales y Académicos II', semestre: 3, prerequisitos: ['lenguas-1'] },
        { id: 'hatha-yoga', nombre: 'Estilos de Vida Saludable (HATHA YOGA)', semestre: 3, prerequisitos: [] },

        // Semestre 4
        { id: 'est-2', nombre: 'Estadística II para las Ciencias de la Administración', semestre: 4, prerequisitos: ['est-1'] },
        { id: 'ciencias-humanas', nombre: 'Ciencias Humanas', semestre: 4, prerequisitos: [] },
        { id: 'der-int', nombre: 'Derecho Internacional Público y Privado', semestre: 4, prerequisitos: ['importaciones'] },
        { id: 'teo-com-ext', nombre: 'Teoría de Comercio Exterior', semestre: 4, prerequisitos: ['eco-empresa'] },
        { id: 'exportaciones', nombre: 'Exportaciones', semestre: 4, prerequisitos: ['importaciones'] },
        { id: 'lenguas-3', nombre: 'Lenguas con Fines Generales y Académicos III', semestre: 4, prerequisitos: ['lenguas-2'] },
        { id: 'teatros-mundo', nombre: 'Artístico y Humanístico (TEATROS Y TEATRALIDADES DEL MUNDO)', semestre: 4, prerequisitos: [] },

        // Semestre 5
        { id: 'humanismo-catedra', nombre: 'Humanismo, Pensamiento Administrativo y Organizaciones – Cátedra', semestre: 5, prerequisitos: ['ciencias-humanas'] },
        { id: 'inv-op', nombre: 'Investigación y Gestión de Operaciones', semestre: 5, prerequisitos: ['est-2'] },
        { id: 'reg-cambiario', nombre: 'Régimen Cambiario', semestre: 5, prerequisitos: ['exportaciones'] },
        { id: 'mercadeo', nombre: 'Mercadeo', semestre: 5, prerequisitos: ['exportaciones'] },
        { id: 'negociacion-int', nombre: 'Técnicas de Negociación Internacional', semestre: 5, prerequisitos: ['exportaciones'] },
        { id: 'analisis-sist-prod', nombre: 'Análisis de los Sistemas de Producción de Bienes y Servicios', semestre: 5, prerequisitos: ['sist-info-ger', 'est-2'] },
        { id: 'lenguas-4', nombre: 'Lenguas con Fines Generales y Académicos IV', semestre: 5, prerequisitos: ['lenguas-3'] },

        // Semestre 6
        { id: 'costos-presupuestos', nombre: 'Análisis de Costos y Presupuestos', semestre: 6, prerequisitos: ['fund-conta', 'sist-info-ger'] },
        { id: 'estrategias-neg-int', nombre: 'Estrategias y Negocios Internacionales', semestre: 6, prerequisitos: ['macro-coyuntura', 'negociacion-int'] },
        { id: 'logistica', nombre: 'Logística y Distribución Física Nacional e Internacional', semestre: 6, prerequisitos: ['negociacion-int'] },
        { id: 'tratados-acuerdos', nombre: 'Tratados y Acuerdos Comerciales', semestre: 6, prerequisitos: ['reg-cambiario'] },
        { id: 'geoeconomia', nombre: 'Geoeconomía', semestre: 6, prerequisitos: ['macro-coyuntura', 'reg-cambiario'] },
        { id: 'electiva-1', nombre: 'Electiva Profesional (COMPETITIV. Y DESARROLLO ECON. REGIONAL)', semestre: 6, prerequisitos: [] },

        // Semestre 7
        { id: 'inteligencia-negocios', nombre: 'Inteligencia de Negocios y Analítica de Datos para la Gestión', semestre: 7, prerequisitos: ['sist-info-ger'] },
        { id: 'admin-financiera', nombre: 'Administración y Gestión Financiera', semestre: 7, prerequisitos: ['costos-presupuestos'] },
        { id: 'contratacion-int', nombre: 'Contratación Internacional', semestre: 7, prerequisitos: ['geoeconomia'] },
        { id: 'inv-mercados', nombre: 'Investigación de Mercados Internacionales', semestre: 7, prerequisitos: ['tratados-acuerdos', 'mercadeo'] },
        { id: 'formacion-social', nombre: 'Formación Social y Ciudadana', semestre: 7, prerequisitos: [] },
        { id: 'electiva-2', nombre: 'Electiva Profesional', semestre: 7, prerequisitos: [] },

        // Semestre 8
        { id: 'consultorio', nombre: 'Consultorio Empresarial', semestre: 8, prerequisitos: ['contratacion-int'] },
        { id: 'evaluacion-proyectos', nombre: 'Evaluación Financiera de Proyectos', semestre: 8, prerequisitos: ['admin-financiera'] },
        { id: 'finanzas-int', nombre: 'Finanzas Internacionales', semestre: 8, prerequisitos: ['admin-financiera'] },
        { id: 'taller-escritura', nombre: 'Taller de Escritura Científica', semestre: 8, prerequisitos: ['comp-textos'] },
        { id: 'integracion-eco', nombre: 'Integración Económica y Bloques Comerciales', semestre: 8, prerequisitos: ['tratados-acuerdos'] },
        { id: 'electiva-3', nombre: 'Electiva Profesional', semestre: 8, prerequisitos: [] },
        
        // Semestre 9
        { id: 'normas-int', nombre: 'Requisitos y Normas Internacionales para Bienes y Servicios', semestre: 9, prerequisitos: ['tratados-acuerdos'] },
        { id: 'electiva-4', nombre: 'Electiva Profesional', semestre: 9, prerequisitos: [] },
        { id: 'trabajo-grado', nombre: 'Trabajo de Grado', semestre: 9, prerequisitos: ['consultorio'] }
    ];

    const frasesMotivacionales = [
        "¡Una asignatura más superada en el camino hacia mi meta profesional! Cada esfuerzo, cada noche de estudio, cada reto... ¡valió la pena!",
        "Hoy celebro más que una nota: celebro mi disciplina, mi constancia y mis ganas de crecer como profesional.",
        "Cada materia aprobada es un paso más cerca de mi título como Profesional en Comercio Exterior. ¡Vamos por más!",
        "Hoy apruebo una asignatura, mañana estaré negociando con el mundo. ¡Mi futuro se construye paso a paso!",
        "Pasar esta materia no solo representa una calificación, representa todo el esfuerzo silencioso que hay detrás. ¡Estoy haciendo que mi sueño se cumpla!",
        "Otra asignatura que queda atrás, y una versión más fuerte de mí que avanza. ¡Esto apenas comienza!",
        "Cada logro me acerca al profesional que quiero ser. ¡Hoy celebro lo que he logrado y lo que aún voy a conquistar!"
    ];

    // 2. ELEMENTOS DEL DOM
    const mallaContainer = document.getElementById('malla-curricular');
    const notificacionEl = document.getElementById('notificacion');

    // 3. ESTADO DE LA APLICACIÓN
    // Carga los ramos aprobados desde localStorage o inicializa un array vacío.
    let ramosAprobados = JSON.parse(localStorage.getItem('ramosAprobados')) || [];

    // 4. FUNCIONES

    /**
     * Guarda el estado actual de los ramos aprobados en localStorage.
     */
    const guardarProgreso = () => {
        localStorage.setItem('ramosAprobados', JSON.stringify(ramosAprobados));
    };

    /**
     * Muestra una notificación en pantalla.
     * @param {string} mensaje - El texto a mostrar.
     * @param {number} duracion - Duración en milisegundos.
     */
    const mostrarNotificacion = (mensaje, duracion = 3000) => {
        notificacionEl.textContent = mensaje;
        notificacionEl.classList.add('mostrar');
        setTimeout(() => {
            notificacionEl.classList.remove('mostrar');
        }, duracion);
    };
    
    /**
     * Muestra una frase motivacional aleatoria al aprobar un ramo.
     */
    const celebrarLogro = () => {
        const frase = frasesMotivacionales[Math.floor(Math.random() * frasesMotivacionales.length)];
        mostrarNotificacion(frase, 5000);
    };

    /**
     * Actualiza la barra de progreso.
     */
    const actualizarProgreso = () => {
        const totalRamos = ramosData.length;
        const aprobados = ramosAprobados.length;
        const porcentaje = totalRamos > 0 ? (aprobados / totalRamos) * 100 : 0;

        document.getElementById('progreso-porcentaje').textContent = `${porcentaje.toFixed(1)}%`;
        document.getElementById('barra-progreso-relleno').style.width = `${porcentaje}%`;
    };

    /**
     * Actualiza las clases CSS de todos los ramos según su estado (aprobado, bloqueado, disponible).
     */
    const actualizarEstadoVisual = () => {
        const todosLosRamosEl = document.querySelectorAll('.ramo');

        todosLosRamosEl.forEach(ramoEl => {
            const ramoId = ramoEl.dataset.id;
            const ramoInfo = ramosData.find(r => r.id === ramoId);
            
            // Resetear clases
            ramoEl.classList.remove('aprobado', 'bloqueado');
            
            // 1. Verificar si está aprobado
            if (ramosAprobados.includes(ramoId)) {
                ramoEl.classList.add('aprobado');
                return; // Si está aprobado, no puede estar bloqueado
            }

            // 2. Verificar si está bloqueado (si no está aprobado)
            const requisitos = ramoInfo.prerequisitos;
            const requisitosFaltantes = requisitos.filter(reqId => !ramosAprobados.includes(reqId));
            
            if (requisitosFaltantes.length > 0) {
                ramoEl.classList.add('bloqueado');
                // Guardar los requisitos faltantes en el elemento para mostrarlos después
                ramoEl.dataset.requisitosFaltantes = requisitosFaltantes
                    .map(reqId => ramosData.find(r => r.id === reqId).nombre)
                    .join(', ');
            } else {
                 ramoEl.dataset.requisitosFaltantes = '';
            }
        });

        actualizarProgreso();
    };

    /**
     * Maneja el evento de clic en un ramo.
     * @param {Event} e - El objeto del evento.
     */
    const handleRamoClick = (e) => {
        const ramoEl = e.currentTarget;
        const ramoId = ramoEl.dataset.id;
        
        if (ramoEl.classList.contains('bloqueado')) {
            const faltantes = ramoEl.dataset.requisitosFaltantes;
            mostrarNotificacion(`Ramo bloqueado. Debes aprobar: ${faltantes}`);
            return;
        }

        // Alternar estado de aprobación
        if (ramosAprobados.includes(ramoId)) {
            // Desaprobar
            ramosAprobados = ramosAprobados.filter(id => id !== ramoId);
        } else {
            // Aprobar
            ramosAprobados.push(ramoId);
            celebrarLogro();
        }
        
        guardarProgreso();
        actualizarEstadoVisual();
    };

    /**
     * Renderiza la malla curricular completa en el DOM.
     */
    const renderizarMalla = () => {
        const numSemestres = Math.max(...ramosData.map(r => r.semestre));
        
        for (let i = 1; i <= numSemestres; i++) {
            // Crear contenedor del semestre
            const semestreContainer = document.createElement('div');
            semestreContainer.className = 'semestre';
            semestreContainer.id = `semestre-${i}`;
            
            // Crear título del semestre
            const tituloSemestre = document.createElement('h2');
            tituloSemestre.className = 'semestre-titulo';
            tituloSemestre.textContent = `Semestre ${i}`;
            semestreContainer.appendChild(tituloSemestre);

            // Obtener y agregar los ramos de este semestre
            const ramosDelSemestre = ramosData.filter(r => r.semestre === i);
            ramosDelSemestre.forEach(ramoInfo => {
                const ramoEl = document.createElement('div');
                ramoEl.className = 'ramo';
                ramoEl.textContent = ramoInfo.nombre;
                ramoEl.dataset.id = ramoInfo.id;
                
                ramoEl.addEventListener('click', handleRamoClick);
                
                semestreContainer.appendChild(ramoEl);
            });
            
            mallaContainer.appendChild(semestreContainer);
        }
    };

    // 5. INICIALIZACIÓN
    renderizarMalla();
    actualizarEstadoVisual();

});# Malla-Curricular
