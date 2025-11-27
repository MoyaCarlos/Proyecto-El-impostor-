# 📱 **El Impostor – Juego Social en un Solo Teléfono**

Flutter App – Proyecto colaborativo

Aplicación móvil desarrollada en **Flutter** para jugar al clásico juego social de **roles ocultos** en **juntadas con amigos**, usando **un único teléfono** que se pasa entre los jugadores.

Permite registrar jugadores, guardar sus nombres de forma persistente, elegir modos de juego, repartir roles con animación tipo tarjeta, gestionar votación secreta por turno, revelar roles y determinar el resultado final según reglas fieles al juego de mesa.

---

# 🧩 **Características principales**

### ✔ Juego totalmente local

No requiere internet. Todo ocurre en un único dispositivo.

### ✔ Un solo teléfono

El móvil se va pasando entre jugadores de forma secuencial.

### ✔ Persistencia de jugadores

Los nombres quedan guardados aunque se cierre la app.

### ✔ Modos de juego

* **Impostor clásico**
* **Impostor + Sr. Blanco**
* Preparado para agregar más roles y modos en el futuro.

### ✔ Diccionarios de palabras

Temas como **HOGAR, MÚSICA, CINE, VIAJES, ANIMALES, ETC.**
Cada partida elige un tema y una palabra al azar.

### ✔ Reparto de roles con animación

Tarjeta que se voltea al tocar el botón “Ver rol”.

### ✔ Votación secreta por jugador

Los jugadores votan en el orden en que fueron registrados.

### ✔ Resolución automática de la ronda

Incluye los casos de:

* Impostor encontrado → ganan los civiles
* Ciudadano eliminado → continúa la partida
* Sr. Blanco votado → intenta adivinar la palabra

  * Si acierta → gana él solo
  * Si falla → queda eliminado

### ✔ Temporizador de discusión configurable

Tiempo ajustable desde la pantalla de modos.

### ✔ Botón “Salir”

Cierra la app completamente (útil para teléfonos con multitarea pesada).

### ✔ Foco en UX para juegos sociales

* Interfaz limpia
* Botones grandes
* Indicaciones claras
* Flujo guiado paso a paso

---

# 🏗️ **Objetivo del Proyecto**

Crear un sistema digital confiable, divertido y rápido para jugar “El Impostor” sin necesidad de distribuir tarjetas físicas ni preparar materiales.

Se busca que la app sea:

* **Simple de usar**
* **Estética y entendible**
* **Rápida de navegar**
* **Segura para roles secretos**
* **Extendible en el futuro**

---

# 📚 **Flujo completo del juego**

## 1. Pantalla de Inicio

* **Nueva partida**
* **Modos de juego**
* **Salir** (cierra la aplicación)

---

## 2. Pantalla de Jugadores

* Campo para ingresar nombre
* Botón **Agregar jugador**
* Lista de jugadores agregados, usando SharedPreferences
* Cada jugador tiene botón de **Quitar**
* Botón **Continuar** (mínimo recomendado: 4 jugadores)
* Botón **Volver al inicio**

La lista queda guardada incluso si se cierra la app.

---

## 3. Pantalla de Modo de Juego

* Selección de modo:

  * Impostor clásico
  * Impostor + Sr. Blanco
  * (Extensible)
* Selector de **temporizador** (30/60/90/120 segundos)
* Preparación interna:

  * Elección aleatoria de tema
  * Elección aleatoria de palabra dentro del tema
* **Repartir roles** → genera roles según modo
* Botón **Volver**

---

## 4. Pantalla de Mostrar Rol

Sistema para que cada jugador vea su rol sin ser observado:

* “Turno de: Nombre del jugador”
* Botón **Ver rol** → animación tipo **flip card**
* Rol mostrado:

  * **Ciudadano**: “Sos ciudadano. Palabra: X”
  * **Impostor**: “Sos el Impostor. No conocés la palabra.”
  * **Sr. Blanco**: “Sos el Sr. Blanco. No conocés la palabra. Intentá adivinarla.”
* Botón:

  * **“Listo, pasá el teléfono”**
* Botón de emergencia:

  * **“Reiniciar partida”** (si alguien vio algo mal)

Cuando todos ven su rol → pasa a Resumen.

---

## 5. Pantalla de Resumen (Inicio de discusión)

* Muestra el tiempo configurado
* Inicia temporizador visual
* Texto general:

  * “Discutan entre ustedes. Cuando estén listos, inicien la votación secreta.”
* Botón:

  * **“Iniciar votación secreta”**

---

## 6. Votación Secreta (pantalla por jugador)

Los jugadores votan en el **orden en que se registraron**.

Por cada jugador:

* “Turno de: Nombre”
* Botón **Continuar** (pasar teléfono)
* Lista de jugadores para votar (excepto sí mismo)
* Botón:

  * **Confirmar voto**

Se guarda internamente en un mapa:

```dart
Map<String, String> votos = {
  "Jugador A": "Jugador C",
  "Jugador B": "Jugador C",
  ...
};
```

Cuando todos votan → pasa a resultados.

---

## 7. Pantalla de Resultados de la Votación

* Muestra conteo total de votos
* Detecta al **más votado**
* Si hay empate → se muestra el empate (versión MVP sin desempate automático)
* Botón:

  * **Revelar**

---

## 8. Revelación del Rol (Tarjeta final)

Se muestra una tarjeta con el nombre del más votado y el botón **“Revelar”**.

Al tocar:

* Animación flip
* Muestra el rol real

### ❗ Resolución según rol

---

### ✔ Caso 1: El más votado es **Impostor**

* Mensaje: “Era el impostor”
* **Fin de la partida**
* **Ganan los ciudadanos**
* Botón:

  * Volver al inicio / Nueva partida

---

### ✔ Caso 2: El más votado es **Ciudadano**

* Mensaje: “Han votado a un inocente”
* Ciudadano queda **eliminado**
* **La partida continúa sin él**

---

### ✔ Caso 3: El más votado es **Sr. Blanco**

* Mensaje: “Es el Sr. Blanco. Debe intentar adivinar la palabra.”
* Campo para ingresar la palabra que cree correcta
* Botón **Confirmar**

#### Si acierta:

* “El Sr. Blanco adivinó la palabra”
* **Gana él solo**
* Todos los demás pierden
* Fin de partida

#### Si falla:

* “El Sr. Blanco no acertó”
* Sr. Blanco queda **eliminado**
* La partida continúa

---

# 🏆 **Condiciones de victoria (MVP)**

| Situación                     | Resultado        |
| ----------------------------- | ---------------- |
| Impostor revelado             | Ganan Ciudadanos |
| Sr. Blanco revelado y acierta | Gana Sr. Blanco  |
| Sr. Blanco revelado y falla   | Continúa partida |
| Ciudadano revelado            | Continúa partida |
| Partida reiniciada            | Estado inicial   |

A futuro se pueden agregar:

* Reglas de victoria por cantidad de jugadores vivos
* Rondas múltiples
* Varios impostores

---

# 📚 **Diccionarios de palabras (Temas)**

Ubicados en `lib/data/temas.dart` o en un archivo JSON dentro de `assets/`.

Ejemplo:

```dart
final Map<String, List<String>> temas = {
  "HOGAR": ["cama", "cocina", "almohada", "silla"],
  "MUSICA": ["guitarra", "micrófono", "batería"],
  "CINE": ["director", "cámara", "butaca"],
  "ANIMALES": ["gato", "delfín", "tortuga"],
};
```

La palabra para la ronda se elige así:

1. Tema aleatorio
2. Palabra aleatoria dentro del tema

---

# 🗃️ **Persistencia local – SharedPreferences**

Se guardan:

* Lista de jugadores (`List<String>`)
* Configuración opcional:

  * Último modo de juego
  * Último temporizador elegido

No se guardan:

* Votos
* Ganadores
* Estadísticas

---

# 🧱 **Arquitectura del Proyecto**

```
lib/
 ├─ main.dart
 ├─ screens/
 │   ├─ home_screen.dart               // Pantalla 1
 │   ├─ players_screen.dart            // Pantalla 2
 │   ├─ game_mode_screen.dart          // Pantalla 3
 │   ├─ reveal_role_screen.dart        // Pantalla 4
 │   ├─ round_summary_screen.dart      // Pantalla 5
 │   ├─ voting_screen.dart             // Pantalla 6 (subpantallas)
 │   ├─ voting_results_screen.dart     // Resultados
 │   └─ final_reveal_screen.dart       // Tarjeta final
 ├─ data/
 │   └─ temas.dart
 ├─ models/
 │   ├─ jugador.dart
 │   └─ partida.dart
 ├─ services/
 │   ├─ jugadores_service.dart         // SharedPreferences
 │   └─ partida_service.dart           // Reparto de roles, palabras
 └─ widgets/
     └─ flip_card.dart                // Animación
```

---

# 🛠️ **Instalación del entorno de desarrollo**

### 1. Instalar Flutter (canal stable)

[https://flutter.dev/docs/get-started/install](https://flutter.dev/docs/get-started/install)

```bash
flutter doctor
```

---

### 2. Instalar Android Studio (solo SDK + emulador)

---

### 3. Instalar VS Code + extensiones

* Flutter
* Dart

---

### 4. Probar la app en distintos dispositivos

#### ✔ Linux desktop (recomendado para desarrollo rápido)

```bash
flutter config --enable-linux-desktop
flutter run -d linux
```

#### ✔ Navegador (Flutter Web)

```bash
flutter run -d chrome
```

#### ✔ Emulador Android

```bash
flutter run -d emulator
```

#### ✔ Celular físico (cada tanto)

```bash
flutter run -d <id>
```

---

# 🧪 **Metodología de Trabajo – SCRUM liviano**

* Equipo de **3 personas**
* Roles fluidos:

  * Product Owner (visión del juego)
  * Scrum Master (organización)
  * Equipo dev (todos)
* Sprints de **1 semana**
* Reuniones:

  * Planning
  * Daily breve
  * Review
  * Retro
* Herramientas:

  * Trello / GitHub Projects / Notion
* Prototipos:

  * **Figma** para todas las pantallas antes de implementarlas

---

# 🤝 **Contribución**

* Usar ramas por feature
* Commits claros
* Pull requests revisados por al menos 1 integrante

---

# 📜 **Licencia**

MIT (recomendada) o la que el equipo prefiera.

---
