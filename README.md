# FlowPay 
El objetivo es buscar la alternativa para digitalizar gratuitamente el pago electronico en pequeños negocios. 

## ¿Cual es el problema?
Hoy en día muchos pequeños negocios o sucursales no cuentan con una forma digital sencilla y gratuita de cobrar electrónicamente. Los clientes deben usar métodos tradicionales (efectivo, transferencias manuales), lo que dificulta la rapidez, control y trazabilidad de los pagos.

## Tecnologias usadas:
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

## Solucion:
Crear una plataforma web ligera en la que los usuarios inicien sesión o se registren, el negocio pueda generar enlaces de pago seguros con el uso de Interledger. los clientes puedan escanear un QR para ir directamente al link de pago, así se digitaliza el cobro sin necesidad de hardware adicional ni apps externas complejas.

## Beneficios:
-Pagos rápidos y universales con Interledger.
-Accesibilidad: solo se necesita un navegador (no apps pesadas).
-Escaneo QR integrado: evita que el usuario escriba URLs.
-Simplicidad de uso: interfaz clara y minimalista.
-Escalabilidad: se puede extender para reportes, historial de pagos o integración con bancos.

## Arquitectura:
Cliente (Frontend)
 └── HTML + CSS + JS
 └── html5-qrcode (escaneo QR)
 └── UI (login, botones, etc.)

Servidor de Pagos (Backend)
 └── Interledger Payment Pointer
 └── Generación de URL de pago

Conexión
 └── El frontend redirige al usuario al servidor de pagos (Interledger).

## Funciones indispensables:
 Login / Registro básico (aunque en el código aún no está implementado).
 Generar enlace de pago Interledger.
 Escaneo de QR y redirección automática al enlace.
 Notificación de éxito / error en el pago.

## Construccion:
Frontend (interfaz + QR) → Eric Valente De La Cruz Ortiz.
Backend → Brian Fernando Balderas Santiz.
Diseño UI/UX → Sebastian Garduño Suarez.
Gestión del proyecto → Diego Rangel Silva.

# Codigo: 

```
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Login y Pago</title>

  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&display=swap" rel="stylesheet">

  <style>
    body {
      font-family: 'Poppins', sans-serif;
      background: linear-gradient(135deg, #1e3c72, #2a5298);
      height: 100vh;
      margin: 0;
      display: flex;
      justify-content: center;
      align-items: center;
    }

    .container {
      background: rgba(255, 255, 255, 0.15);
      backdrop-filter: blur(12px);
      -webkit-backdrop-filter: blur(12px);
      padding: 40px 30px;
      border-radius: 20px;
      box-shadow: 0 8px 25px rgba(0,0,0,0.15);
      text-align: center;
      width: 360px;
      display: none;
      animation: fadeIn 1s ease-in-out;
      color: #fff;
    }

    h2 {
      margin-bottom: 25px;
      font-size: 26px;
      font-weight: 700;
      letter-spacing: 0.5px;
    }

    button {
      display: block;
      width: 100%;
      padding: 14px;
      margin: 14px 0;
      border: none;
      border-radius: 12px;
      font-size: 16px;
      font-weight: 600;
      color: white;
      cursor: pointer;
      transition: all 0.3s ease;
      background: linear-gradient(135deg, #4a90e2, #357ABD);
    }

    button:hover {
      transform: translateY(-3px) scale(1.02);
      box-shadow: 0 6px 18px rgba(0,0,0,0.25);
    }

    .btn-register {
      background: linear-gradient(135deg, #50c878, #3da35a);
    }

    .btn-payment {
      background: linear-gradient(135deg, #f39c12, #d68910);
    }

    .btn-scan {
      background: linear-gradient(135deg, #8e44ad, #6c3483);
    }

    #reader {
      margin: 20px auto;
      border: 2px solid rgba(255, 255, 255, 0.3);
      border-radius: 15px;
      padding: 12px;
      background: rgba(255, 255, 255, 0.08);
      display: none;
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(30px); }
      to { opacity: 1; transform: translateY(0); }
    }

    @media (max-width: 420px) {
      .container {
        width: 90%;
        padding: 25px;
      }
    }
  </style>

  <script src="https://unpkg.com/html5-qrcode"></script>
</head>
<body>

  <div class="container" id="login-container" style="display:block;">
    <h2>🌍 Bienvenido a la Sucursal</h2>
    <button onclick="login()">🔑 Iniciar Sesión</button>
    <button class="btn-register" onclick="alert('Aquí iría la página de registro')">📝 Registrarse</button>
  </div>

  <div class="container" id="action-container">
    <h2>📲 Opciones de Pago</h2>
    <button class="btn-payment" onclick="goToPayment()">💳 Generar Pago</button>
    <button class="btn-scan" onclick="scanQR()">📷 Escanear QR</button>
    <div id="reader"></div>
  </div>

  <script>
    let reader = null;

    function login() {
      document.getElementById("login-container").style.display = "none";
      document.getElementById("action-container").style.display = "block";
    }

    function goToPayment() {
      const paymentURL = "https://pay.interledger-test.dev/payment-choice?receiver=https://ilp.interledger-test.dev/tienda";
      window.open(paymentURL, "_blank");
    }

    function scanQR() {
      document.getElementById("reader").style.display = "block";

      if(reader) {
        reader.stop().catch(err => console.error(err));
      }

      reader = new Html5Qrcode("reader");
      reader.start(
        { facingMode: "user" },
        { fps: 10, qrbox: 250 },
        qrCodeMessage => {
          alert(`✅ QR detectado: ${qrCodeMessage}`);
          reader.stop();
          window.location.href = qrCodeMessage;
        },
        errorMessage => {
          console.log("Escaneando...");
        }
      ).catch(err => console.error(err));
    }
  </script>

</body>
</html>
```
