#FlowPay 
El objetivo es buscar la alternativa para digitalizar gratuitamente el pago electronico en pequeños negocios. 

#¿Cual es el problema?
Hoy en día muchos pequeños negocios o sucursales no cuentan con una forma digital sencilla y gratuita de cobrar electrónicamente. Los clientes deben usar métodos tradicionales (efectivo, transferencias manuales), lo que dificulta la rapidez, control y trazabilidad de los pagos.

#Tecnologias usadas:
-Frontend:
  HTML, CSS, JS (interfaz básica).
  html5-qrcode (para escanear códigos QR desde la cámara).
  Google Fonts y estilos con gradientes para el diseño.

-Backend / Pago:
  Interledger Protocol (ILP) → manejo de pagos abiertos e interoperables.
  Servidor de prueba de Interledger (interledger-test.dev).

-Infraestructura (stack simple):
  Navegador del usuario (ejecuta la UI + escaneo QR).
  Servidor de Interledger (procesa pagos).

#Solucion:
Crear una plataforma web ligera en la que los usuarios inicien sesión o se registren, el negocio pueda generar enlaces de pago seguros con el uso de Interledger. los clientes puedan escanear un QR para ir directamente al link de pago, así se digitaliza el cobro sin necesidad de hardware adicional ni apps externas complejas.

#Beneficios:
-Pagos rápidos y universales con Interledger.
-Accesibilidad: solo se necesita un navegador (no apps pesadas).
-Escaneo QR integrado: evita que el usuario escriba URLs.
-Simplicidad de uso: interfaz clara y minimalista.
-Escalabilidad: se puede extender para reportes, historial de pagos o integración con bancos.

#Arquitectura:
Cliente (Frontend)
 └── HTML + CSS + JS
 └── html5-qrcode (escaneo QR)
 └── UI (login, botones, etc.)

Servidor de Pagos (Backend)
 └── Interledger Payment Pointer
 └── Generación de URL de pago

Conexión
 └── El frontend redirige al usuario al servidor de pagos (Interledger).

#Funciones indispensables:
 Login / Registro básico (aunque en el código aún no está implementado).
 Generar enlace de pago Interledger.
 Escaneo de QR y redirección automática al enlace.
 Notificación de éxito / error en el pago.

#Construccion:
Frontend (interfaz + QR) → Eric Valente De La Cruz Ortiz.
Backend → Brian Fernando Balderas Santiz.
Diseño UI/UX → Sebastian Garduño Suarez.
Gestión del proyecto → Diego Rangel Silva.
