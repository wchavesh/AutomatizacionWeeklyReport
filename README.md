🤖 Agente IA — Reporte Semanal de Proyectos

Workflow n8n que genera y envía automáticamente reportes de avance de proyectos cada viernes a las 4pm, usando GPT-4o y Gmail.

Lee los datos del proyecto desde Excel → analiza con IA → envía dos versiones del reporte: una interna para el equipo y otra ejecutiva para el cliente.


¿Qué hace?

Excel con tareas → GPT-4o analiza → Email interno (equipo) + Email cliente

Cada viernes a las 4pm el workflow:


Lee las tareas, responsables, avances y fechas del proyecto
Llama a GPT-4o para analizar riesgos y redactar los reportes
Genera reporte interno con semáforos 🟢🟡🔴, alertas críticas y nivel de riesgo
Genera reporte cliente en tono ejecutivo, sin detalles internos
Envía ambos emails automáticamente por Gmail



Contenido del repositorio

├── reporte-semanal-n8n.json              # Workflow para importar en n8n
├── ClienteABC-MigracionPlataforma.xlsx   # Ejemplo de proyecto con datos demo
└── sistema de prompts/
    └── sistema-prompts-gestion-proyectos.md  # Sistema de 5 prompts para gestionar proyectos con IA


Requisitos


n8n (self-hosted o cloud) — v2.4 o superior
Cuenta de OpenAI con acceso a GPT-4o
Cuenta de Gmail conectada en n8n



Cómo usar

1. Importar el workflow

En n8n: Workflows → Import from file → selecciona reporte-semanal-n8n.json

2. Conectar las credenciales

El workflow requiere dos credenciales configuradas en n8n:

NodoCredencialOpenAI Chat ModelAPI Key de OpenAI (nómbrala "OpenAI account")Email Interno / Email ClienteCuenta Gmail

3. Reemplazar los datos del proyecto

Abre el nodo "Leer Proyectos Excel" y reemplaza el array projects con los datos reales de tu proyecto. La forma más fácil es:

bash# 1. Completa tu Excel en la carpeta SeguimientoProyectos/
# 2. Ejecuta el script para generar el JSON
python analizar_proyectos.py

# 3. Copia el output y pégalo en el nodo "Leer Proyectos Excel"

La estructura del Excel se explica más abajo.

4. Publicar y probar


Haz clic en "Execute workflow" para una prueba manual
Si todo funciona, haz clic en "Publish"
El workflow correrá solo cada viernes a las 4pm



Estructura del Excel

El archivo ClienteABC-MigracionPlataforma.xlsx incluye tres hojas:

Hoja Tareas

ColumnaDescripciónB2Nombre del proyectoD2Nombre del clienteF2PMH2Fecha de inicioJ2Fecha de cierre estimadaFila 6+Tareas: ID, Fase, Categoría, Tarea, Responsable, Fechas, Prioridad, Estado, % Avance, Notas

Hoja Contactos


Sección CLIENTE (filas 5–9): a quién se envía el reporte del cliente
Sección EQUIPO INTERNO (filas 13–20): a quién se envía el reporte interno


Los emails en estas secciones son los destinatarios de los correos automáticos.


Sistema de prompts

La carpeta sistema de prompts/ contiene un sistema de 5 prompts encadenados para gestionar proyectos completos con IA:

PromptFunción1 — InicializaciónCaptura datos del proyecto y genera el Excel base2 — Carga de tareasAgrega o modifica tareas desde texto libre3 — Standup diarioProcesa el resumen del standup y actualiza el Excel4 — Análisis de saludCalcula avance, semáforos y nivel de riesgo5 — NotificacionesGenera los reportes interno y cliente

El workflow n8n automatiza los prompts 4 y 5 cada viernes.


Personalización

Cambiar el horario: edita el nodo "Cada Viernes 4pm" → expresión cron 0 16 * * 5

Agregar más proyectos: agrega más objetos al array projects en el nodo "Leer Proyectos Excel". El workflow genera un reporte por cada proyecto.

Cambiar el modelo: en el nodo "OpenAI Chat Model" puedes cambiar gpt-4o por cualquier modelo compatible.
