# Demo: respuesta de leads con IA

Página para mostrar en reuniones y en contenido. El visitante escoge un negocio, escribe como si fuera un cliente y en segundos ve cómo la IA califica su mensaje (caliente, tibio o frío) y le responde. La respuesta también le llega a su correo.

**Pruébala:** https://jfedrmon-spec.github.io/demo-leads

## Cómo está armada

| Parte | Dónde vive | Qué hace |
|---|---|---|
| `index.html` | GitHub Pages | La página que ve el visitante |
| `backend/Codigo.gs` | Google Apps Script | Llama a la IA, envía el correo y guarda cada prueba en un Google Sheet |

La clave de la API **nunca** está en GitHub: queda guardada en las propiedades privadas del script de Google.

## Instalación del motor (unos 15 minutos)

### 1. Hoja para registrar las pruebas
1. Crea un Google Sheet nuevo, por ejemplo "Demo leads IA - registros".
2. Copia su ID: es la parte de la URL que va entre `/d/` y `/edit`.

### 2. Crea el script
1. Entra a [script.google.com](https://script.google.com) y crea un **Proyecto nuevo**. Llámalo "Motor demo leads".
2. Borra lo que aparece y pega todo el contenido de [`backend/Codigo.gs`](backend/Codigo.gs).
3. En `CONFIG` pon tu WhatsApp en `TU_WHATSAPP` (ej: `573001234567`). Guarda.

### 3. Guarda la clave y el ID de la hoja
1. Ve a **Configuración del proyecto** (engranaje a la izquierda) y baja hasta **Propiedades del script**.
2. Agrega dos propiedades:
   - `ANTHROPIC_API_KEY`: tu clave de [console.anthropic.com](https://console.anthropic.com) (empieza por `sk-ant-`).
   - `SHEET_ID`: el ID que copiaste en el paso 1.

### 4. Prueba el motor
1. Vuelve al editor, escoge la función `probarMotor` arriba y dale **Ejecutar**.
2. Acepta los permisos. Si sale "Google no verificó esta app": *Configuración avanzada → Ir a Motor demo leads*. Es normal porque el script es tuyo.
3. En el registro de ejecución debe aparecer la calificación y el correo que escribió la IA.

### 5. Publica el motor
1. Arriba a la derecha: **Implementar → Nueva implementación**.
2. En el engranaje de "Seleccionar tipo" escoge **Aplicación web**.
3. **Ejecutar como:** Yo. **Quién tiene acceso:** Cualquier usuario.
4. Dale **Implementar** y copia la **URL de la aplicación web** (termina en `/exec`).
5. Pega esa URL en `index.html`, en `CONFIG.API_URL`. Esa URL sí puede ser pública: la clave sigue escondida.

> Si después cambias el código del motor: **Implementar → Gestionar implementaciones → Editar (lápiz) → Versión: Nueva versión → Implementar**. Si no lo haces, la URL sigue usando el código viejo.

## Protección contra abuso

La página es pública, así que el motor tiene límites (se cambian en `CONFIG`):
- Máximo 100 pruebas al día en total.
- Máximo 3 pruebas por correo cada hora.
- Mensajes de máximo 1.200 caracteres.
- Un campo invisible que atrapa bots.

Gmail gratis permite unos 100 correos al día enviados por scripts.

## Costo

Cada prueba cuesta alrededor de 1 a 2 centavos de dólar en la API de Claude.
