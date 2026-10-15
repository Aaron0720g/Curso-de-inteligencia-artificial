# Ejercicio 2 — Descripciones PEAS

### 1\. Asistente virtual de voz

* **Performance:** tasa de reconocimiento correcto de comandos, tiempo de respuesta, tareas completadas sin repetir la orden, satisfacción del usuario.
* **Environment:** hogar u oficina con ruido variable y varios hablantes; parcialmente observable, estocástico, secuencial, dinámico, continuo.
* **Actuators:** síntesis de voz por la bocina, llamadas a servicios web y apps, alarmas y recordatorios, luces indicadoras.
* **Sensors:** micrófonos, reconocedor de voz, reloj, ubicación.

### 2\. Robot aspirador doméstico

* **Performance:** superficie limpiada por carga, suciedad recogida, tiempo por ciclo, colisiones y caídas evitadas, consumo de batería.
* **Environment:** departamento con muebles, mascotas y puertas que cambian de posición; parcialmente observable, estocástico, secuencial, dinámico, continuo.
* **Actuators:** motores de ruedas, cepillos, turbina de succión, regreso a la base de carga, avisos sonoros y en la app.
* **Sensors:** sensores de choque, acantilado y proximidad, giroscopio, detector de suciedad, cámara, nivel de batería.

### 3\. Sistema de recomendación de streaming

* **Performance:** clics en lo recomendado, minutos reproducidos, tasa de abandono, diversidad del catálogo sugerido, retención de suscriptores.
* **Environment:** plataforma con millones de usuarios y catálogo que cambia, estocástico, secuencial, dinámico, discreto.
* **Actuators:** ordenar filas y carruseles de la interfaz, armar listas automáticas, enviar notificaciones y correos, elegir la carátula mostrada.
* **Sensors:** historial de reproducción, valoraciones, búsquedas, hora y dispositivo, metadatos del catálogo, comportamiento de usuarios similares.

### 4\. Vehículo autónomo en ciudad

* **Performance:** llegar al destino sin accidentes, respeto a reglas de tránsito, tiempo de viaje, suavidad de frenado y giro, consumo de combustible.
* **Environment:** calles urbanas con tráfico, peatones, ciclistas, clima y obras, estocástico, secuencial, dinámico, continuo, multiagente.
* **Actuators:** acelerador, freno, volante, cambios, direccionales, claxon, luces, pantalla y voz para el pasajero.
* **Sensors:** cámaras, LIDAR, radar, ultrasonido, GPS, odómetro, acelerómetro, mapas digitales, sensores del motor.

### 5\. Agente de trading algorítmico

* **Performance:** rendimiento neto de comisiones, riesgo asumido, pérdida máxima, latencia de ejecución, cumplimiento de límites regulatorios.
* **Environment:** mercados financieros con otros operadores humanos y automáticos, estocástico, secuencial, dinámico, multiagente.
* **Actuators:** órdenes de compra y venta, órdenes límite y stop, cancelación de órdenes, ajuste del tamaño de posición, cobertura con derivados.
* **Sensors:** cotizaciones en tiempo real, libro de órdenes, volumen, indicadores macroeconómicos, noticias y reportes financieros por API, estado del portafolio.

### 6\. Diagnóstico médico asistido por IA

* **Performance:** sensibilidad y especificidad, falsos negativos evitados, tiempo hasta el diagnóstico, utilidad de la explicación para el médico.
* **Environment:** hospital o clínica con pacientes y personal médico; parcialmente observable, estocástico, secuencial, dinámico, discreto y continuo.
* **Actuators:** lista de diagnósticos probables con su confianza, marcado de regiones sospechosas en la imagen, sugerencia de estudios adicionales, alertas de urgencia, notas en el expediente.
* **Sensors:** imágenes clínicas, resultados de laboratorio, signos vitales, historia clínica electrónica, síntomas capturados, bases de datos médicas.

### 7\. Dron de inspección de infraestructura

* **Performance:** defectos detectados frente a los existentes, cobertura de la estructura, precisión de localización del daño, tiempo de vuelo, integridad del dron.
* **Environment:** puentes, tuberías o líneas eléctricas al aire libre, con viento y señal intermitente, estocástico, secuencial, dinámico, continuo.
* **Actuators:** motores y hélices, orientación de la cámara en el gimbal, disparo de foto y video, encendido de luces, regreso automático, marcado de coordenadas del hallazgo.
* **Sensors:** cámara visual y térmica, GPS, IMU, altímetro, sensor de distancia, anemómetro, nivel de batería.

### 8\. Agente jugador de ajedrez

* **Performance:** partidas ganadas, puntaje Elo, tiempo usado por jugada, calidad de las jugadas frente al análisis posterior.
* **Environment:** tablero de 64 casillas con un solo oponente; totalmente observable, determinista, secuencial, estático, discreto, multiagente.
* **Actuators:** mover una pieza, capturar, enrocar, coronar un peón, ofrecer tablas, rendirse, registrar la jugada en notación.
* **Sensors:** posición actual del tablero, jugada del oponente, historial de la partida, reloj de la partida, libro de aperturas y tablas de finales.

