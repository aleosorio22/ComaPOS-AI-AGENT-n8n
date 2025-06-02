# 🧠 Agente de Reportes — ComaPOS | n8n + MCP Server/Client

Este repositorio contiene un **agente inteligente de reportes** desarrollado en **n8n**, diseñado específicamente para integrarse con el sistema de punto de venta **ComaPOS**. Utiliza una arquitectura basada en **MCP Server y MCP Client** para automatizar la generación, consulta y envío de reportes clave del negocio.

---

## 📦 ¿Qué hace este agente?

El asistente permite realizar **consultas automáticas al backend de ComaPOS** para obtener datos como:

- Ventas del día o por rango de fechas.
- Productos con stock bajo.
- Movimientos de caja.
- Reportes personalizados.

Posteriormente, envía los resultados al canal configurado (correo electrónico, Telegram, etc.) sin intervención humana, todo de forma orquestada a través de flujos de n8n.

---

## 🔁 Arquitectura de los Flujos

El sistema está compuesto por **tres flujos principales**, con funciones bien definidas:

### 1. `MCP Server - Reports`
Encargado de **realizar las peticiones HTTP al backend** de ComaPOS para obtener la información solicitada. 
- Utiliza nodos `HTTP Request` para consultar endpoints protegidos o públicos del backend (por ejemplo: `/api/reportes/ventas-del-dia`).
- Puede recibir parámetros dinámicos desde otros flujos (como fechas o filtros personalizados).

### 2. `MCP Server - Mails`
Encargado de **enviar reportes por correo electrónico** (u otros canales).
- Utiliza nodos de `Gmail`, `SMTP`, o `Webhook` para integraciones con plataformas externas.
- Admite personalización del mensaje y adjuntos (PDFs, Excel, JSON).

### 3. `MCP Client`
Es el **agente maestro** que conecta ambos MCP Servers:
- Recibe comandos u órdenes desde otro sistema (por ejemplo: "enviar reporte de ventas de hoy").
- Llama a los flujos `MCP Server Reports` para obtener la información.
- Luego, invoca `MCP Server Mails` para enviar la respuesta al usuario.

Esta estructura permite **modularidad, escalabilidad y reutilización** de componentes para otros agentes o flujos.

---

## 🔌 Requisitos Técnicos

- Instancia de **n8n** operativa (self-hosted o en la nube).
- Sistema ComaPOS en producción, con sus **APIs REST expuestas** y accesibles desde n8n.
- Conexión configurada a servicios externos de mensajería (como Gmail, Outlook, SendGrid, etc.).
- Acceso autenticado a los endpoints del backend de ComaPOS (usualmente con token o credenciales JWT).
- Reglas MCP definidas (como "bajo stock", "ventas hoy", "reporte mensual") según la lógica de negocio.

---

## ⚙️ Tecnologías Utilizadas

- [n8n](https://n8n.io/) (Flujos de automatización sin código)
- ComaPOS API (Backend desarrollado en Node.js + MySQL)
- HTTP Request Nodes
- Gmail Nodes (o cualquier otro sistema de envío de emails)
- Webhooks internos para integración A2A (Agent to Agent)

---

## ✏️ Ejemplo de Flujo

1. El MCP Client recibe un trigger para "reporte de ventas de hoy".
2. Llama al flujo `MCP Server Reports`, el cual hace un `HTTP Request` a:  
   `https://api.comapos.com/api/reportes/ventas-hoy`
3. La respuesta se transforma en un formato legible (JSON, tabla, PDF).
4. Luego, `MCP Server Mails` se activa y envía un correo con el reporte al destinatario.

---

## 📬 Contacto

¿Tienes dudas, sugerencias o quieres colaborar?

**Alejandro Osorio**  
Desarrollador del sistema ComaPOS y de la arquitectura de automatización.  
📧 Email personal: [alejandroosorio022@gmail.com](mailto:alejandroosorio022@gmail.com)  
📧 Email empresarial: [comatecgt@gmail.com](mailto:comatecgt@gmail.com)  
🌐 GitHub: [@aleosorio22](https://github.com/aleosorio22)

**Karen Jiménez**  
Colaboradora en la creación de los flujos automatizados de n8n.  
📧 Email: [kjimenezg6@miumg.edu.gt](mailto:kjimenezg6@miumg.edu.gt)

---

## 📄 Licencia

Este proyecto está bajo la licencia MIT. Puedes modificar, reutilizar y adaptar los flujos según tus necesidades, siempre y cuando se mantenga el crédito correspondiente.

---

## 💡 Nota Final

Este asistente puede ser adaptado fácilmente a **otros sistemas** con APIs REST, no únicamente ComaPOS. Basta con ajustar las URLs y parámetros en los nodos HTTP para integrarlo en cualquier backend moderno.
