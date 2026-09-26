# Simulador IV/IO — Manual de uso

Simulador educativo de bomba de infusión intravenosa (IV) e intraósea (IO), pensado para practicar programación de fármacos, cálculo de dosis, manejo de alarmas y reconocimiento de distintas interfaces de bombas reales.

> ⚠️ **Uso exclusivamente educativo.** No es un dispositivo médico, no está validado clínicamente y no debe usarse para tomar decisiones de tratamiento real. Los nombres de marcas de bombas mencionados en este documento se citan únicamente con fines de reconocimiento visual/educativo; no existe afiliación, patrocinio ni certificación por parte de esos fabricantes.

---

## 1. Cómo abrir la app

Es un archivo `.html` autocontenido: ábrelo con doble clic o arrástralo a cualquier navegador (Chrome, Edge, Firefox). No necesita instalación ni conexión a internet.

---

## 2. Estructura de la pantalla

| Elemento | Descripción |
|---|---|
| **Selector de modelo de bomba** | Menú desplegable en la parte superior. Cambia el tema visual y la disposición de botones sin borrar lo programado. |
| **Datos del paciente** | Campo de **peso (kg)**, usado para calcular dosis por kg. |
| **Acceso vascular** | Botones para elegir el tipo de acceso (periférico, central, intraóseo, etc.), cada uno con su presión basal simulada. |
| **Fármaco/Solución** | Lista de soluciones y fármacos disponibles; cada uno trae su unidad de dosificación y, si aplica, su concentración real. |
| **Pantalla (CONSOLE)** | Muestra el fármaco seleccionado, estado (STANDBY/RUN/ALARMA), tasa (mL/h), dosis (si aplica), VTBI, volumen infundido y presión de línea. |
| **Teclado numérico** | Para digitar valores directamente. |
| **Flechas ▲ / ENTER / ▼** | Ajustan el valor del campo activo (RATE, DOSIS o VTBI) o confirman lo digitado. |
| **Botones de transporte** | START, STOP, PAUSA, ON/OFF, BOLUS/PURGA, SILENCIO (sus nombres cambian según el modelo de bomba elegido). |
| **Puerta/palanca** | Simula la compuerta del casete de la bomba; debe estar cerrada para poder iniciar la infusión. |
| **Botones de diagnóstico** | Para forzar manualmente alarmas de práctica (Ocluir, Aire en línea, etc.). |

---

## 3. Flujo básico de uso

1. **Enciende la bomba** con el botón `ON/OFF` si está apagada.
2. **Ingresa el peso del paciente** (obligatorio para fármacos dosificados por kg).
3. **Selecciona el acceso vascular** que vas a simular.
4. **Selecciona el fármaco o solución** de la lista.
   - Al cambiar de fármaco, la **tasa y la dosis se reinician a 0** a propósito (medida de seguridad: nunca se debe arrastrar la tasa de un fármaco anterior a uno nuevo).
5. **Programa el valor:**
   - Toca el recuadro `RATE`, `DOSIS` o `VTBI` para seleccionarlo como campo activo.
   - Escribe el número con el teclado y confirma con `ENTER`, o usa `▲`/`▼` para ajustarlo.
6. **Cierra la puerta/palanca** si está abierta.
7. Presiona **START** (o el botón equivalente del modelo elegido) para iniciar la infusión.
8. Usa **PAUSA**, **STOP** o **SILENCIO** según necesites durante la simulación.

---

## 4. Cómo funciona el cálculo de Dosis ↔ Tasa

Para fármacos dosificados por peso (mcg/kg/min, mcg/kg/h, mg/kg/h, UI/h, mEq/h), la app usa la **concentración real de la mezcla** (mostrada en pantalla como `CONC:`) para convertir automáticamente entre dosis y tasa, en cualquier dirección:

- Si programas la **dosis**, la app calcula la tasa (mL/h) necesaria.
- Si programas la **tasa**, la app calcula la dosis resultante.
- Si cambias el **peso del paciente** mientras hay una dosis activa, la tasa se recalcula para mantener la dosis prescrita.

Fórmulas usadas:

| Unidad de dosis | Fórmula (tasa en mL/h) |
|---|---|
| mcg/kg/min | `tasa = dosis × peso × 60 / concentración` |
| mcg/kg/h | `tasa = dosis × peso / concentración` |
| mg/kg/h | `tasa = dosis × peso / concentración` |
| UI/h | `tasa = dosis / concentración` |
| mEq/h | `tasa = dosis / concentración` |

Para soluciones simples (Salina, Hartmann, Dextrosa), no existe "dosis": solo se programa la tasa en mL/h directamente.

También incluye una **calculadora de goteo por gravedad** independiente: `tasa (mL/h) = (gotas/min × 60) / factor de goteo (gtt/mL)`.

---

## 5. Medidas de seguridad simuladas

- **Bloqueo por vía incompatible:** no permite iniciar un fármaco vesicante (p. ej. Norepinefrina, Dextrosa 50%, KCl) por un acceso periférico.
- **Bloqueo por dosis no programada:** no permite iniciar un fármaco dosificado por kg si la dosis sigue en 0.
- **Reinicio de tasa/dosis al cambiar de fármaco:** evita administrar por error la tasa de un fármaco distinto.
- **Alarma de puerta abierta:** no inicia si la palanca/casete no está cerrada.
- **Alarma de oclusión:** se dispara si la presión de línea simulada supera el umbral (~220 mmHg); la presión oscila de forma realista alrededor del valor basal del acceso elegido.
- **Alarma de VTBI alcanzado:** al llegar a 0 mL, avisa y detiene la progresión del volumen.

---

## 6. Modelos de bomba disponibles

El selector superior permite cambiar entre 8 interfaces con estructura y botones distintos (mismo motor de cálculo por debajo):

| Modelo | Particularidad de interfaz |
|---|---|
| **Genérico** | Pantalla oscura tipo OLED, botones Start/Stop/Pausa independientes. |
| **BD Alaris™ System (estilo)** | Pantalla clara, botones `STOP/NO` y `STANDBY`, alarma `SILENCE X`. |
| **B. Braun Infusomat/Perfusor Space (estilo)** | Una sola tecla combinada **START/STOP** (sin pausa independiente), pantalla monocroma verdosa, botón `PURGA`. |
| **Baxter Sigma Spectrum (estilo)** | Estética de pantalla táctil, teclado numérico "plano", botones `RUN`/`MUTE`. |
| **Fresenius Kabi Agilia (estilo)** | Carcasa blanco/gris, botones `MARCHA`/`PARO`. |
| **ICU Medical Plum 360/Plum A+ (estilo)** | Acento lavanda/morado, pausa como `HOLD`. |
| **CME/McKinley T34 — Jeringa (estilo)** | Pantalla pequeña ámbar, **sin Pausa ni Bolo**, con **bloqueo de teclado físico** (ícono 🔒 en pantalla) que impide reprogramar mientras está activo. |
| **Smiths Medical Medfusion (estilo)** | Pantalla táctil grande, teclado "plano", acentos teal/naranja. |

Cambiar de modelo **no reinicia** el paciente ni la infusión en curso; solo cambia el tema visual y la disposición de botones.

---

## 7. Botones de diagnóstico (práctica de alarmas)

Ubicados aparte de los controles de transporte, permiten forzar manualmente:
- **Ocluir** → simula alarma de oclusión de línea.
- **Aire** → simula alarma de aire en línea.

Útiles para practicar la respuesta ante alarmas sin esperar a que ocurran de forma espontánea.

---

## 8. Limitaciones conocidas

- Los valores de concentración de cada fármaco son ejemplos de preparación estándar (indicados en el nombre del fármaco); en la práctica real siempre debe verificarse la concentración de la mezcla preparada.
- El simulador no reemplaza el entrenamiento ni la certificación en el uso de bombas de infusión reales.
- Las interfaces de marcas comerciales son representaciones aproximadas con fines de reconocimiento visual, no réplicas exactas certificadas.
