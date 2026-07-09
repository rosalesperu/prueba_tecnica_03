# prueba_tecnica_03
---

# Examen Técnico – RPA

## Objetivo

Desarrollar un bot de automatización que procese un conjunto de proveedores, valide la información y registre únicamente los datos válidos en un portal web utilizando un navegador real (UiPath, Selenium o Playwright).

---

# Etapa 1 – Automatización del Portal

El bot debe:

1. Abrir el portal web.
2. Leer el archivo de proveedores.
3. Validar la información.
4. Registrar únicamente los proveedores válidos.
5. Adjuntar el archivo PDF solicitado.
6. Enviar el formulario.
7. Reabrir el portal para cada nuevo registro (el token es de un solo uso).

---

# Archivos proporcionados

* Dataset en Excel (.xlsx)
* Dataset en CSV (.csv)
* Archivo PDF para adjuntar en cada registro

---

# Reglas de validación

El bot debe validar que:

### Email

* Tenga un formato válido.
* Ejemplo:

  ```
  usuario@dominio.tld
  ```

---

### Fecha de constitución

* Debe ser una fecha válida.
* Formato:

```
YYYY-MM-DD
```

* No puede ser una fecha futura.

---

### Campos obligatorios

No pueden estar vacíos:

* Razón Social
* Categoría
* Régimen

---

### Categorías permitidas

Únicamente:

* Tecnología
* Servicios
* Manufactura
* Logística

---

### Regímenes permitidos

Únicamente:

* Común
* Simplificado
* Gran Contribuyente

---

### Servicios

Debe existir al menos un servicio válido.

Valores permitidos:

* Hosting
* Soporte
* Consultoría
* Desarrollo
* Capacitación

En el archivo vienen separados mediante:

```
|
```

En el formulario corresponden a checkboxes.

---

### NIT

* No se permiten duplicados.
* Si un NIT aparece repetido, solamente se debe registrar la primera ocurrencia.

---

### Adjuntos

En cada registro debe adjuntarse el PDF proporcionado.

---

### Reintentos

El portal puede responder temporalmente con:

```
HTTP 503
```

El bot debe implementar reintentos automáticos antes de considerar el envío como fallido.

---

# Criterios de evaluación

Se evalúa:

* Registro correcto de proveedores.
* Validación adecuada de datos.
* Evitar registros duplicados.
* Robustez del proceso mediante reintentos ante errores temporales.

---

# Etapa Avanzada (Bonus)

Si se completa la etapa principal, se debe procesar un segundo conjunto de proveedores.

## Transformación de datos

La fecha viene con formato:

```
DD/MM/YYYY
```

Debe convertirse a:

```
YYYY-MM-DD
```

antes de registrar el proveedor.

---

## Enriquecimiento mediante API

El archivo no contiene la categoría.

En su lugar contiene un código.

El bot debe consultar:

```
GET /api/catalogo/{codigo}
```

Ejemplo:

```
TEC
```

y utilizar la categoría devuelta por la API para completar el formulario.

---

## Control de Rate Limit

El portal puede responder:

```
HTTP 429
```

El bot debe:

* esperar el tiempo indicado por `Retry-After`,
* volver a intentar automáticamente,
* completar los cuatro registros.

---

## Adjuntos

Debe adjuntarse el mismo PDF utilizado en la etapa anterior.

---

# Batería de Misiones

Además del ejercicio principal, el examen propone seis automatizaciones independientes.

## Misión 1 – Alta de proveedores

Registrar correctamente los proveedores utilizando el formulario web y completar la etapa principal y el bonus.

---

## Misión 2 – Tabla web paginada

* Navegar por una tabla con múltiples páginas.
* Contar las facturas con estado **VENCIDA**.
* Sumar sus montos.
* Enviar el resultado en formato JSON.

---

## Misión 3 – Lectura de PDF

* Descargar un PDF.
* Extraer:

  * Número de orden.
  * Total.
* Enviar ambos valores en formato JSON.

---

## Misión 4 – API con cursor

Consumir una API paginada utilizando el campo:

```
nextCursor
```

Filtrar:

* Región = Norte.
* Saldo > 3000.

Calcular:

* Cantidad de clientes.
* Suma de saldos.

Enviar el resultado en JSON.

---

## Misión 5 – Correlación de múltiples fuentes

Consumir:

* Un archivo CSV de pagos.
* Una API de clientes.

Calcular:

* Total pagado por clientes activos.
* Nombre del cliente activo con mayor pago.

Responder en formato JSON.

---

## Misión 6 – Formulario dinámico

* Abrir un formulario web.
* Inspeccionar el DOM.
* Identificar controles cuyos nombres cambian dinámicamente.
* Completar el formulario.
* Enviar la información desde la página.

---

## Consideraciones generales

* Las respuestas a las misiones deben enviarse en formato **JSON** (`Content-Type: application/json`).
* Cada misión puede reintentarse hasta obtener una respuesta correcta.
* El examen evalúa conocimientos de:

  * Automatización web.
  * Validación de datos.
  * Manejo de archivos Excel y CSV.
  * Lectura de PDF.
  * Consumo de APIs REST.
  * Procesamiento de JSON.
  * Paginación.
  * Manejo de errores HTTP (503 y 429).
  * Reintentos automáticos.
  * Automatización de formularios dinámicos.
  * Integración de múltiples fuentes de información.
