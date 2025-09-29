# Filter-jobs-linkedin

Este workflow de **n8n** automatiza la búsqueda de ofertas de empleo en LinkedIn, las filtra según palabras clave, resume la descripción de cada vacante usando **OpenAI** y finalmente envía los resultados a un canal de **Slack**.

---

## 🚀 Flujo de trabajo

### 🔹 Triggers

- **Manual Trigger**: permite ejecutar el workflow manualmente.
- **Schedule Trigger**: programa la ejecución automática (ejemplo: todos los días a las 23:00).

### 🔹 Obtención de datos

- **HTTP Request**: obtiene el HTML de la página de búsqueda de empleos en LinkedIn.
- **HTML**: extrae los links de cada vacante usando el selector `.base-card__full-link`.
- **Code in JavaScript**: convierte la lista de links en un arreglo de objetos `{ link }`.

### 🔹 Procesamiento de vacantes

- **HTTP Request1**: obtiene el HTML de cada oferta.
- **HTML1**: extrae los datos clave de cada oferta:

  - Título
  - Compañía
  - Tipo de solicitud (Request type)
  - Link
  - Descripción (HTML)

- **filter-jobs**: filtra las vacantes cuyo título contenga alguna de las palabras clave:

  - `react`
  - `frontend`
  - `frontend junior`

- **Limit**: reduce el resultado a máximo **15 ofertas**.

### 🔹 Procesamiento con IA

- **Message a model (OpenAI GPT-4.1-mini)**:  
  Limpia la descripción (elimina etiquetas HTML) y la resume en máximo **4 líneas** con los siguientes datos:
  - Salario
  - Tipo de contrato
  - Requisitos principales
  - Responsabilidades
  - Beneficios / Perks
  - Fecha de publicación / vigencia

### 🔹 Salida

- **Send a message (Slack)**: envía el resultado al canal configurado (ejemplo: `#general-jhosuacode`) con este formato:

- Título: {title}
- Link: {link}
- Compañía: {company}
- Descripción: {resumen generado por IA}

yaml
Copiar código

---

## ⚙️ Requisitos

- Una cuenta de **n8n** (local o en servidor).
- Credenciales configuradas para:
  - **Slack API** (para enviar mensajes al canal).
  - **OpenAI API** (para resumir y limpiar las descripciones).

---

## 📂 Estructura de nodos principales

1. Manual Trigger / Schedule Trigger
2. HTTP Request → HTML → Code in JavaScript
3. HTTP Request1 → HTML1 → filter-jobs → Limit
4. Message a model (OpenAI) → Send a message (Slack)

---

## 📌 Uso

1. Importar el workflow en tu instancia de **n8n**.
2. Configurar las credenciales de **Slack** y **OpenAI**.
3. Ajustar el link de búsqueda de LinkedIn en el nodo **HTTP Request**.
4. Ejecutar manualmente o esperar a la ejecución programada.
5. Revisar los resultados en el canal de **Slack** configurado.
