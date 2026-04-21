# MyAppWithCamera

Aplicación Android educativa que demuestra el uso de la cámara con Jetpack Compose, solicitud de permisos en tiempo de ejecución y renderizado de imágenes capturadas.

## 📋 Descripción

Este proyecto es una **práctica pequeña de Android** que implementa:

- **CameraScreen**: Pantalla que integra la funcionalidad de cámara
- **Solicitud de permisos**: Implementación de permisos en tiempo de ejecución para acceso a la cámara
- **Captura de fotos**: Uso de `ActivityResultContracts.TakePicture()` para capturar fotos
- **Guardado en MediaStore**: Las fotos se guardan en la galería del dispositivo usando MediaStore
- **Renderizado con Coil**: Visualización de imágenes capturadas usando la librería Coil

## 🛠️ Tecnologías

- **Lenguaje**: Kotlin
- **UI Framework**: Jetpack Compose
- **Navegación**: Navigation Compose
- **Carga de imágenes**: Coil (coil-compose)
- **API Mínima**: Android 5.0 (API 21)
- **API Objetivo**: Android 14 (API 34)

## 📁 Estructura del Proyecto

```
MyAppWithCamera/
├── app/
│   ├── src/main/
│   │   ├── java/com/example/myappwithcamera/
│   │   │   └── MainActivity.kt          # Pantalla principal, navegación y cámara
│   │   ├── AndroidManifest.xml          # Configuración de la app y permisos
│   │   └── res/                         # Recursos
│   └── build.gradle.kts
├── build.gradle.kts
└── gradle.properties
```

## ⚙️ Configuración Requerida

### Permisos en AndroidManifest.xml

Asegúrate de tener el siguiente permiso en `app/src/main/AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.CAMERA" />
```

### Dependencias en build.gradle.kts

La aplicación requiere las siguientes dependencias:

```kotlin
dependencies {
    // Compose
    implementation("androidx.compose.ui:ui")
    implementation("androidx.compose.material3:material3")
    implementation("androidx.navigation:navigation-compose")
    
    // Camera (Activity Result Contracts)
    implementation("androidx.activity:activity-compose")
    
    // Coil para cargar imágenes
    implementation("io.coil-kt:coil-compose:2.2.2")
    
    // Core
    implementation("androidx.core:core:1.10.1")
}
```

## 🚀 Funcionalidades

### HomeScreen
- Botón para navegar a la pantalla de cámara
- Interfaz simple y limpia

### CameraScreen
1. **Verificación de permisos**: Comprueba si el permiso de cámara está otorgado
2. **Solicitud de permiso**: Pide el permiso al usuario si no está otorgado
3. **Captura de foto**: 
   - Crea una URI en MediaStore
   - Lanza el intento de captura con `TakePicture()`
   - La foto se guarda automáticamente en la galería
4. **Visualización**: Muestra la foto capturada usando `AsyncImage` de Coil

## 💡 Conceptos Clave Implementados

### 1. Solicitud de Permisos en Tiempo de Ejecución
```kotlin
val launcherPermiso = rememberLauncherForActivityResult(
    contract = ActivityResultContracts.RequestPermission()
) { concedido ->
    if (concedido) {
        // Permiso otorgado
    }
}
```

### 2. Captura de Foto
```kotlin
val launcherCamara = rememberLauncherForActivityResult(
    contract = ActivityResultContracts.TakePicture()
) { success ->
    if (!success) {
        // Limpiar si la captura falla
    }
}
```

### 3. MediaStore
Las fotos se guardan en:
- Ubicación: `Pictures/MiApp`
- Formato: JPEG
- Nombre: `foto_<timestamp>.jpg`

### 4. Coil AsyncImage
Carga y renderiza imágenes desde URIs:
```kotlin
AsyncImage(
    model = imagenUri,
    contentDescription = "Foto tomada",
    modifier = Modifier.size(300.dp)
)
```

## 🔄 Flujo de la Aplicación

```
HomeScreen (Botón "Ir a Cámara")
    ↓
CameraScreen
    ├─ Si permiso no otorgado → Solicitar permiso
    │   └─ Si usuario acepta → Proceder
    │   └─ Si usuario rechaza → Finalizar
    ├─ Crear URI en MediaStore
    ├─ Lanzar cámara (TakePicture)
    ├─ Usuario captura foto (o cancela)
    │   └─ Si captura exitosa → Renderizar con Coil
    │   └─ Si cancela → Limpiar URI
    └─ Mostrar foto capturada
```

## 🧪 Pruebas Recomendadas

1. **Dispositivo físico con Android 10+** (API 29+): Soporte completo para MediaStore.RELATIVE_PATH
2. **Emulador**: Usar emulador con cámara virtual habilitada
3. **Casos de prueba**:
   - ✓ Otorgar permiso y capturar foto exitosamente
   - ✓ Rechazar permiso y verificar que no se lance la cámara
   - ✓ Cancelar captura y verificar que no queden registros vacíos
   - ✓ Capturar múltiples fotos consecutivas

## 📝 Notas

- La aplicación utiliza `MediaStore` para guardar fotos, compatible con Android 10+ (Q y superiores)
- Para compatibilidad con Android < 10, considera implementar `FileProvider`
- Las fotos se guardan en la galería del dispositivo en `Pictures/MiApp`
- Se utiliza Compose completamente para la UI

## 👤 Autor

Práctica educativa de Android

## 📄 Licencia

Este proyecto es de carácter educativo

