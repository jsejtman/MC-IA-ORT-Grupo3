A.1 — El caso
Tener una IA que reemplace al asistente (persona) de un profesional de la salud independiente. que permita gestionar la agenda (abrir agenda, asigar turnos y recordar turnos todo via whastapp), que pueda resolver dudas frecuentes de los pacientes que se contactan


## Mapa inicial — [Nombre del caso]
1. Usuario
El sistema interactúa con tres perfiles principales:

Paciente: Interactúa vía WhatsApp para agendar, cancelar, reasignar turnos o consultar dudas frecuentes.

Profesional de la Salud (Médico/Terapeuta): Configura su disponibilidad, supervisa la agenda, define los protocolos de atención y puede intervenir el chat si la IA detecta un caso complejo o una urgencia.

Administrador del Sistema / Desarrollador: Mantiene la infraestructura, monitorea los logs de las conversaciones y ajusta las integraciones.

2. Datos
¿Qué información necesita entrar al sistema para funcionar correctamente?

Disponibilidad del Profesional: Días y horarios de atención, duración estándar de las consultas (ej. 30 minutos), feriados, vacaciones y excepciones.

Base de Datos de Pacientes: Nombre, teléfono (WhatsApp ID), DNI/Historia Clínica (si aplica) y un registro de sus turnos pasados y futuros.

Base de Conocimiento (FAQs del Consultorio):

Preguntas frecuentes sobre preparaciones previas a estudios (ej. "venir en ayunas").

Ubicación exacta del consultorio, medios de pago aceptados, obras sociales o prepagas que atiende, y política de cancelación (ej. avisar con 24 horas de anticipación).

Contexto de la Conversación: Historial reciente del chat de WhatsApp para mantener el hilo lógico de la interacción.

3. Modelo (LLM)
Recomendación: GPT-4o (o Claude 3.5 Sonnet) mediante API.

Por qué: Los modelos más avanzados son indispensables aquí por tres motivos clave:

Comprensión de lenguaje natural coloquial: Los pacientes escriben de forma muy variada (audios transcritos, faltas de ortografía, expresiones locales).

Uso de Herramientas (Function Calling): El LLM debe ser capaz de entender con precisión cuándo extraer parámetros (como fecha y hora) para ejecutar una función en la base de datos.

Control de Tono: Permiten ajustar el system prompt con mucha rigurosidad para mantener un tono empático, profesional, cálido y estrictamente prudente (evitando dar diagnósticos médicos).

4. Herramientas (Tools / Function Calling)
El LLM no opera solo; actúa como el "cerebro" que decide qué herramientas ejecutar según la intención del paciente:

Gestor de Agenda (API de Calendario o BD):

consultar_horarios_disponibles(fecha)

reservar_turno(paciente_id, fecha_hora, tipo_consulta)

cancelar_o_reagendar_turno(turno_id)

Motor de Mensajería (WhatsApp Business API / Meta Cloud):

enviar_mensaje_whatsapp(telefono, texto)

programar_recordatorio(turno_id, tiempo_antes) (para enviar alertas 24h y 2h antes del turno).

Buscador de Conocimiento (RAG):

buscar_en_faqs(query) para extraer información validada sobre preparaciones, ubicación o aranceles.

Sistema de Derivación a Humano:

escalar_a_asistente_humano(motivo, conversacion_id) para pausar la IA y notificar al profesional si el paciente reporta síntomas urgentes o pide hablar con una persona.

5. Reglas (Límites y Validaciones)
Para un entorno de salud, las restricciones son críticas:

No Diagnóstico / No Prescripción: La regla de oro en el system prompt debe prohibir terminantemente emitir opiniones médicas, sugerir medicamentos o interpretar síntomas como si fuera un profesional. Ante consultas clínicas, la IA debe responder con un mensaje estándar: "No puedo evaluar síntomas por este medio. Si es una urgencia, acérquese a una guardia médica o comuníquese con emergencias".

Validación de Identidad / Datos Obligatorios: Antes de confirmar un turno, el sistema debe asegurarse de tener el nombre completo y datos de contacto del paciente.

Ventana de Mensajería de WhatsApp: Respetar las políticas de Meta sobre plantillas aprobadas (template messages) para iniciar conversaciones fuera de la ventana de 24 horas.

Seguridad y Privacidad (Ley de Protección de Datos Personales): Encriptación de datos en tránsito y en reposo, y asegurar que la API del LLM utilizada no entrene modelos públicos con información privada de pacientes.

6. Salida
¿Qué recibe finalmente el usuario (paciente y profesional)?

Para el Paciente (en su WhatsApp):

Respuestas inmediatas, naturales y amables a sus dudas.

Opciones claras de horarios en formato de texto legible.

Confirmación formal del turno con los datos clave (fecha, hora, dirección o link de videollamada).

Recordatorios automáticos en su celular.

Para el Profesional de la Salud:

Una agenda sincronizada y ordenada sin fricciones operativas.

Notificaciones en su panel o WhatsApp cuando un paciente agenda, cancela o requiere atención personalizada urgente.

----
SDD -> ODD
Harness Agents

