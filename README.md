# 🔔 Rally de la Independencia AR — Dolores Hidalgo

Juego de **realidad aumentada por ubicación** hecho en Flutter con **TDD** (Desarrollo Guiado por Pruebas).
El jugador recorre Dolores Hidalgo buscando a 6 personajes de la Independencia con un **radar**, un indicador de **"frío o caliente"** y la **cámara**. Al acercarse a 15 m o menos, responde una **pregunta de historia** para atraparlo y guardarlo en su **álbum**.

- **Materia:** Desarrollo Móvil Integral · UTNG
- **Profesor:** Anastacio Rodríguez García
- **Alumna:** Yaneli Sánchez Cárdenas · Grupo GTID244

---

## 📋 ¿Qué se hizo? (resumen por ronda)

| Ronda | Qué se construyó | Archivo(s) |
|---|---|---|
| 0 | Proyecto, paquetes (`camera`, `geolocator`, `flutter_compass`, `shared_preferences`) y permisos de Android | `pubspec.yaml`, `AndroidManifest.xml` |
| 1 | Mini documento de diseño: reglas del juego (15 m, 2 intentos, 100/50 puntos, 60–500 m) | — |
| 2 | Matemáticas GPS: distancia (Haversine), rumbo, ángulos y la función NUEVA `destino` | `geo.dart` |
| 3 | Modelo de datos y catálogo de 6 personajes con su pregunta | `personaje.dart`, `catalogo.dart` |
| 4 | Esconder personajes al azar con `Random` inyectado y raíz cuadrada (reparto parejo) | `aparicion.dart` |
| 5 | Radar (polares → cartesianas) y "frío o caliente" | `radar.dart` |
| 6 | Motor de RA: qué personajes ve la cámara y en qué parte de la pantalla | `motor_ar.dart` |
| 7 | Reglas del juego con `sealed class` (Capturado / Fallo / SeEscapo) | `partida.dart`, `excepciones.dart` |
| 8 | Guardar el progreso (álbum) con un contrato y dos implementaciones | `almacen.dart`, `almacen_prefs.dart` |
| 9 | Fuentes de GPS y brújula: reales y simuladas (modo demo para "caminar" con botones) | `fuentes*.dart` |
| 10 | ViewModel que une todo | `rally_viewmodel.dart` |
| 11 | Pantallas: cámara, radar con `CustomPainter`, juego, álbum e inicio | `presentacion/` |
| 12 | Playtesting, seguridad física y privacidad | — |

### Ciclo TDD usado
🔴 **ROJO:** escribir la prueba primero (falla) → 🟢 **VERDE:** código mínimo para que pase → 🔵 **REFACTOR:** ordenar sin romper nada.

### Estructura
```
lib/
  dominio/       <- reglas del juego (Dart puro, sin Flutter)
  datos/         <- GPS, brújula, simuladores y almacenamiento
  presentacion/  <- ViewModel y pantallas
  main.dart
test/            <- 43 pruebas (geo, aparición, radar, motor AR, partida, viewmodel)
```

---

## ▶️ Cómo ejecutarlo

```bash
flutter pub get
flutter test        # deben pasar todas las pruebas
flutter run         # con el emulador abierto
```

En el emulador usa el botón **"Modo demo (salón de clases)"**: no necesita GPS ni brújula.

---

## 📸 Evidencias

### Código

**C1 · Paquetes del proyecto (`pubspec.yaml`)**
![C1](capturas/C1_pubspec.png)

**C2 · Permisos de Android (`AndroidManifest.xml`)**
![C2](capturas/C2_permisos.png)

**C3 · Función `destino` (Ronda 2)** — si camino X metros con rumbo R, ¿a dónde llego?
![C3](capturas/C3_destino.png)

**C4 · Azar con raíz cuadrada (Ronda 4)** — el `Random` se recibe como parámetro para poder probarlo con semilla.
![C4](capturas/C4_aparicion.png)

**C5 · Radar y frío o caliente (Ronda 5)**
![C5](capturas/C5_radar.png)

**C6 · Motor de realidad aumentada (Ronda 6)**
![C6](capturas/C6_motor_ar.png)

**C7 · Sealed class de resultados (Ronda 7)**
![C7](capturas/C7_sealed.png)

**C8 · Reglas de captura (Ronda 7)** — 15 m, 100/50 puntos, huida tras 2 fallos.
![C8](capturas/C8_capturar.png)

**C9 · Contrato de almacenamiento (Ronda 8)** — solo se guardan ids y puntaje, nunca coordenadas.
![C9](capturas/C9_almacen.png)

**C10 · Caminar en modo demo (Ronda 9)** — reutiliza `destino()`.
![C10](capturas/C10_demo.png)

**C11 · ViewModel `capturar()` (Ronda 10)**
![C11](capturas/C11_viewmodel.png)

**C12 · Radar con CustomPainter (Ronda 11)**
![C12](capturas/C12_custompainter.png)

**C13 · Switch exhaustivo (Ronda 11)**
![C13](capturas/C13_switch.png)

### Pruebas

**P1 · Prueba "ida y vuelta" de `destino` en 9 direcciones**
![P1](capturas/P1_test_geo.png)

**P2 · Prueba de reglas: con 2 fallos el personaje se escapa**
![P2](capturas/P2_test_partida.png)

**P3 · Azar reproducible: misma semilla = mismo resultado**
![P3](capturas/P3_test_aparicion.png)

**T1 · `flutter test`: todas las pruebas en verde**
![T1](capturas/T1_flutter_test.png)

### App en el emulador

**E1 · Pantalla de inicio con aviso de seguridad**
![E1](capturas/E1_inicio.png)

**E2 · Modo demo: radar con los 6 personajes y letrero de temperatura**
![E2](capturas/E2_radar.png)

**E3 · Personaje visible en la "cámara" con su distancia**
![E3](capturas/E3_personaje.png)

**E4 · A 15 m o menos: círculo verde "¡Tócame!"**
![E4](capturas/E4_tocame.png)

**E5 · Pregunta de historia**
![E5](capturas/E5_pregunta.png)

**E6 · Personaje atrapado (+100 puntos)**
![E6](capturas/E6_atrapado.png)

**E7 · Dos fallos: el personaje se escapa**
![E7](capturas/E7_escapo.png)

**E8 · Mi álbum**
![E8](capturas/E8_album.png)

---

## 🔒 Privacidad y seguridad
- La ubicación nunca sale del teléfono; el progreso guardado solo tiene ids de personajes y el puntaje.
- Se pide ubicación solo "mientras se usa la app".
- La pantalla de inicio muestra un aviso para cuidar el paso y no cruzar la calle viendo el teléfono.

> ⚠️ Los datos históricos y las coordenadas son de práctica; deben verificarse con fuentes locales.
