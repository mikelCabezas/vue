# Prueba Técnica para Frontend con habilidades en Backend

**Objetivo:** Evaluar las habilidades del candidato en la creación y manipulación de interfaces web utilizando Vue2, una librería CSS similar a Tailwind, y una librería externa para la gestión de grids, así como la capacidad de almacenar y manipular datos localmente.

---

**Descripción del Proyecto:**

Se necesita desarrollar una aplicación web para la gestión de pedidos. El candidato recibirá un archivo JSON con información sobre: Negocio, Cliente, Pedido y Producto. A partir de esta información, debe crear:

1. **Vista de Gestión de Pedidos:** En esta vista, se deberán mostrar los pedidos en forma de lista con funcionalidades como filtros, contadores de estados, entre otros que el candidato considere útiles.

2. **Vista de Detalle de Pedido:** Debe mostrar información detallada sobre el pedido seleccionado, el negocio y el producto contratados.

El principal desafío de la prueba es la capacidad de editar la vista utilizando un grid flexible. El candidato deberá elegir una librería de componentes internos y ofrecer diferentes tarjetas (cards) que se adapten al tamaño del grid y muestren información relevante sobre el producto.

---

**Requisitos Técnicos:**
1. **Framework:** Vue 2.
2. **CSS Framework:** Tailwind o similar.
3. **Grid Library:** Elección libre por parte del candidato. Debe permitir flexibilidad y adaptabilidad.
4. **Local Storage:** Debe ser capaz de almacenar y recuperar datos localmente.
5. **Dynamic Cards:** Las tarjetas deben ser capaces de adaptar su diseño y contenido en función del tamaño del grid y de la información del producto. Por ejemplo:
    - Producto tipo "web": Deberá mostrar páginas activas, URLs, estadísticas, porcentajes de seguridad/performance, etc.
    - Producto tipo "social": Deberá mostrar cuentas conectadas, URL de perfiles, estadísticas, porcentajes de engagement/likes, etc.
6. **Fake API Data:** Se usarán datos falsos de una API para dotal la app de contenido

---

**Entregables:**
1. **Código fuente:** De la aplicación desarrollada.
2. **Instrucciones:** Para ejecutar y probar la aplicación.
3. **Documentación:** (opcional) Cualquier comentario o decisión de diseño que el candidato considere relevante.

---

**Criterios de Evaluación:**
1. **Funcionalidad:** La aplicación debe cumplir con las especificaciones dadas.
2. **Calidad del Código:** Estructura, limpieza y buenas prácticas.
3. **Adaptabilidad:** Las tarjetas deben adaptarse correctamente al tamaño del grid y mostrar la información relevante.
4. **Diseño y UX:** Facilidad de uso, estética y coherencia del diseño.
5. **Documentación:** Clarity y completitud de cualquier documentación adicional proporcionada.

---

**Tiempo Estimado:** 1 semana

**Recursos mockeados:** 
1. **Users**: https://run.mocky.io/v3/c2c1f79c-3403-4406-be1b-cf9220d23c28
2. **Posts**: https://run.mocky.io/v3/04ecea1d-ea0e-46c8-ba39-fabd53897f8d
2. **Websites**: https://run.mocky.io/v3/c5f76519-3cde-4fe7-9124-a8f1dd91d413
3. **Credits**: https://run.mocky.io/v3/d82d7f1a-57d2-4bcc-a06a-261812ecdee7

---

**Requisitos técnicos escogidos:** 
2. **CSS Framework:** Tailwind CSS.
3. **Grid Library:** Vue Grid Layout: https://github.com/jbaysolutions/vue-grid-layout.
