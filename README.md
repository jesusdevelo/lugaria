# 🅿️ Lugaria — Guía de configuración completa

Aplicación web responsive para detectar y reservar estacionamientos en Guadalajara,
con mapa real (OpenStreetMap, **gratis**) y panel de administrador.

---

## 📁 Archivos del proyecto

```
lugaria/
├── index.html        ← App principal (usuarios)
├── admin.html        ← Panel de administrador
├── firestore.rules   ← Reglas de seguridad de Firestore
└── README.md
```

---

## PASO 1 — Crear proyecto en Firebase

1. Ve a → https://console.firebase.google.com
2. Clic en **"Agregar proyecto"**
3. Nombre: `lugaria-gdl`
4. Google Analytics: opcional
5. Clic en **"Crear proyecto"**

---

## PASO 2 — Registrar la app web

1. En la pantalla principal del proyecto, clic en el ícono **`</>`** (Web)
2. Nombre de la app: `Lugaria Web`
3. **No** marques Firebase Hosting por ahora
4. Clic en **"Registrar app"**
5. Copia el bloque `firebaseConfig` que aparece. Se verá así:

```js
const firebaseConfig = {
  apiKey:            "AIzaSy...",
  authDomain:        "lugaria-gdl.firebaseapp.com",
  projectId:         "lugaria-gdl",
  storageBucket:     "lugaria-gdl.appspot.com",
  messagingSenderId: "1234567890",
  appId:             "1:1234...:web:abc..."
};
```
import { initializeApp } from "firebase/app";
import { getAnalytics } from "firebase/analytics";
// TODO: Add SDKs for Firebase products that you want to use
// https://firebase.google.com/docs/web/setup#available-libraries

// Your web app's Firebase configuration
// For Firebase JS SDK v7.20.0 and later, measurementId is optional
const firebaseConfig = {
  apiKey: "AIzaSyB9b5kGcIAXltYazwKkrA3FW3Z3DwLMDI0",
  authDomain: "lugaria-guadalajara-9505e.firebaseapp.com",
  projectId: "lugaria-guadalajara-9505e",
  storageBucket: "lugaria-guadalajara-9505e.firebasestorage.app",
  messagingSenderId: "456373137134",
  appId: "1:456373137134:web:f7d150c367f3ebbdeb4dff",
  measurementId: "G-K1Z09169FH"
};

// Initialize Firebase
const app = initializeApp(firebaseConfig);
const analytics = getAnalytics(app);

---

## PASO 3 — Pegar la configuración en los archivos

Abre **`index.html`** y busca este bloque (aprox. línea 230):

```js
const firebaseConfig = {
  apiKey:            "TU_API_KEY",
  authDomain:        "TU_PROYECTO.firebaseapp.com",
  ...
};
```

Reemplázalo con tu `firebaseConfig` real.

Haz lo mismo en **`admin.html`** (misma sección, mismos valores).

---

## PASO 4 — Activar Firebase Authentication

1. Firebase Console → menú izquierdo → **Authentication**
2. Clic en **"Comenzar"**
3. Pestaña **"Sign-in method"**
4. Habilita **"Correo electrónico/Contraseña"** → guardar

---

## PASO 5 — Crear la base de datos Firestore

1. Firebase Console → **Firestore Database**
2. Clic en **"Crear base de datos"**
3. Selecciona **"Comenzar en modo de prueba"**
4. Región: `nam5` (Estados Unidos central, la más rápida para México)
5. Clic en **"Listo"**

---

## PASO 6 — Aplicar reglas de seguridad

1. Firebase Console → **Firestore** → pestaña **"Reglas"**
2. Borra el contenido actual
3. Pega el contenido del archivo `firestore.rules`
4. Clic en **"Publicar"**

---

## PASO 7 — Cargar estacionamientos iniciales

Los estacionamientos se agregan desde el **panel de administrador**.
Pero primero necesitas crear una cuenta admin:

### 7a. Crear cuenta de administrador

1. Abre `index.html` en el navegador
2. Clic en **"Registrarse"** → llena el formulario → crea la cuenta
3. Ve a Firebase Console → **Authentication** → **Users**
4. Copia el **UID** de la cuenta que acabas de crear (columna "User UID")

### 7b. Registrar el UID como admin

Abre **`index.html`** y busca:

```js
const ADMIN_UIDS = [];
```

Agrégalo así:

```js
const ADMIN_UIDS = ["PEGA_AQUI_EL_UID"];
```

Haz lo mismo en **`admin.html`**:

```js
const ADMIN_UIDS = ["PEGA_AQUI_EL_UID"];
```

### 7c. Agregar estacionamientos desde el admin

1. Abre `admin.html`
2. Inicia sesión con la cuenta admin
3. Clic en **"Nuevo estac."** en el menú lateral
4. Llena el formulario con:
   - Nombre, dirección
   - Latitud/Longitud (ver instrucciones de coordenadas abajo)
   - Lugares totales y disponibles
   - Precio por hora, horario, tipo

---

## Cómo obtener coordenadas (lat/lng) gratis

### Opción A — OpenStreetMap (recomendado)
1. Ve a https://www.openstreetmap.org
2. Navega hasta el estacionamiento
3. Clic derecho en el punto exacto → **"Mostrar dirección"**
4. Aparece `lat=XX.XXXX, lon=-XXX.XXXX` en la URL

### Opción B — Google Maps
1. Ve a maps.google.com
2. Clic derecho sobre el estacionamiento
3. El primer elemento del menú es la latitud y longitud. Clic para copiar.

### Coordenadas de ejemplo en Guadalajara:

| Zona            | Lat        | Lng          |
|-----------------|------------|--------------|
| Centro Histórico| 20.6596    | -103.3496    |
| Providencia     | 20.6831    | -103.3730    |
| Zapopan Centro  | 20.7213    | -103.3884    |
| Tlaquepaque     | 20.6412    | -103.3124    |
| Andares         | 20.7255    | -103.4222    |
| Chapalita       | 20.6679    | -103.4080    |

---

## PASO 8 — Ejecutar el proyecto

> ⚠️ Firebase requiere un servidor HTTP. **No funciona abriendo el archivo directamente.**

### Opción A — VS Code + Live Server (recomendado)
1. Instala la extensión **"Live Server"** en VS Code
2. Clic derecho en `index.html` → **"Open with Live Server"**

### Opción B — Node.js
```bash
npx serve .
# Abre http://localhost:3000
```

### Opción C — Python
```bash
python3 -m http.server 3000
# Abre http://localhost:3000
```

### Opción D — Deploy gratuito con Firebase Hosting
```bash
npm install -g firebase-tools
firebase login
firebase init hosting
# public directory: . (punto)
# single-page app: No
firebase deploy
# Te da una URL pública gratuita tipo https://lugaria-gdl.web.app
```

---

## Estructura de la base de datos (Firestore)

### Colección `parkings`
```
parkings/{auto-id}
  ├─ name:         "Plaza Sol"
  ├─ address:      "Av. Vallarta 1234"
  ├─ city:         "Guadalajara"
  ├─ lat:          20.6764
  ├─ lng:          -103.3820
  ├─ totalSpots:   20
  ├─ availSpots:   14        ← se actualiza al reservar
  ├─ pricePerHour: 20
  ├─ hours:        "6:00–22:00"
  ├─ type:         "Cubierto"
  ├─ active:       true
  └─ updatedAt:    Timestamp
```

### Colección `users`
```
users/{uid}
  ├─ name:       "Juan Díaz"
  ├─ email:      "juan@gmail.com"
  ├─ phone:      "+52 33 1234 5678"
  └─ createdAt:  Timestamp

users/{uid}/vehicles/{auto-id}
  ├─ plate:      "ABC-123"
  ├─ type:       "auto" | "suv" | "moto" | "camion"
  ├─ brand:      "Nissan"
  ├─ model:      "Versa"
  ├─ color:      "Gris"
  └─ createdAt:  Timestamp
```

### Colección `reservations`
```
reservations/{auto-id}
  ├─ userId:        "uid_del_usuario"
  ├─ parkingId:     "id_del_estac"
  ├─ parkingName:   "Plaza Sol"
  ├─ vehicleId:     "id_del_vehiculo"
  ├─ vehiclePlate:  "ABC-123"
  ├─ vehicleType:   "auto"
  ├─ startTime:     Timestamp
  ├─ endTime:       Timestamp
  ├─ durationHours: 2
  ├─ totalPrice:    40
  ├─ confirmCode:   "LUG-AB12CD"
  ├─ status:        "active" | "cancelled" | "completed"
  └─ createdAt:     Timestamp
```

---

## Funciones de la app (resumen)

### index.html (usuarios)
- ✅ Mapa real con OpenStreetMap (Leaflet.js) — gratis, sin API key
- ✅ Pines de colores con disponibilidad en tiempo real
- ✅ Registro e inicio de sesión (Firebase Auth)
- ✅ Registro de vehículos vinculados al usuario
- ✅ Reserva por tiempo (30 min hasta 6 horas)
- ✅ Código de confirmación único (LUG-XXXXXX)
- ✅ Historial de reservas en el perfil
- ✅ **Totalmente responsive** — móvil con barra de navegación inferior y bottom sheet
- ✅ Modo demo funciona sin Firebase (datos de ejemplo)

### admin.html (administrador)
- ✅ Login protegido por lista de UIDs admins
- ✅ Dashboard con estadísticas en tiempo real
- ✅ CRUD completo de estacionamientos
- ✅ Ajuste manual de disponibilidad
- ✅ Listado de todas las reservas
- ✅ Listado de todos los usuarios
- ✅ Responsive para tablet y móvil
- ✅ Actualizaciones en tiempo real (Firestore onSnapshot)

---

## Próximas funciones sugeridas

- [ ] Notificaciones push (Firebase Cloud Messaging) cuando el tiempo de reserva termina
- [ ] Código QR del comprobante de reserva
- [ ] Cancelación de reservas desde la app
- [ ] Módulo de pagos (Stripe o MercadoPago)
- [ ] Foto del estacionamiento subida por el admin
- [ ] Reportes de ocupación por hora (gráficas)
- [ ] App móvil nativa (React Native / Expo)

---

## Tecnologías utilizadas

| Tecnología       | Uso                          | Costo    |
|------------------|------------------------------|----------|
| Firebase Auth    | Registro y login             | Gratis   |
| Firestore        | Base de datos en tiempo real | Gratis*  |
| Leaflet.js       | Mapa interactivo             | Gratis   |
| OpenStreetMap    | Tiles del mapa               | Gratis   |
| Firebase Hosting | Deploy web (opcional)        | Gratis   |

*Firestore en plan Spark (gratuito): 50,000 lecturas/día, 20,000 escrituras/día — más que suficiente para empezar.
