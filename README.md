## 🗺️ KrossMap

**KrossMap** is a lightweight, cross-platform Maps library designed for **Kotlin Multiplatform (KMP)**. It provides an easy and consistent API for working with maps, markers, polylines, and camera movements across Android and iOS — all using **Jetpack Compose** and **SwiftUI Compose Interop**.

Whether you're building a delivery app, ride tracker, or location-based feature, **KrossMap** simplifies the map experience with powerful abstractions and built-in utilities.

### 🚀 Features

- 🧭 Marker rendering & animation  
- 📍 Current location tracking  
- 📷 Camera control & animation  
- 🛣️ Polyline (route) support  
- 💡 Jetpack Compose friendly  
- 🌍 Kotlin Multiplatform Ready (Android & iOS)

### 📦 Dependency

To add **KrossMap** to your project, include the following in your **shared module's `build.gradle.kts`**:

```kotlin
dependencies {
    implementation("io.github.farimarwat:krossmap:1.1")
}

```

### 🛠️ Usage Guide

Follow these simple steps to get started with `KrossMap` in your Kotlin Multiplatform app.

---

### 📝 Create Camera State

This defines the initial position and zoom level of the map:

```kotlin
val cameraState = rememberKrossCameraPositionState(
    latitude = 32.60370,      // Latitude of the location
    longitude = 70.92179,     // Longitude of the location
    zoom = 18f,               // Zoom level (higher = closer)
    tilt = 45f,               // Tilt angle of the camera (0 = top-down, 90 = side view)
    bearing = 0f              // Direction the camera is facing (0 = north)
)
```
### 📝 Create Map State

```kotlin
val mapState = rememberKrossMapState()

LaunchedEffect(Unit) {
    mapState.startLocationUpdate()
    mapState.onUpdateLocation = { coordinates ->
        // Update marker and camera when location changes
       
    }
}
```

### 📍 Custom Marker Icon in KrossMap

In `KrossMap`, you define markers using the `KrossMarker` data class. This class includes an optional `icon` parameter of type `ByteArray?`, allowing you to pass in a custom image for the marker.

---

### 🧱 KrossMarker Data Class
```kotlin
data class KrossMarker(
    var coordinate: KrossCoordinate,
    val title: String = "",
    val icon: ByteArray? = null // Nullable: provide icon or leave it null
)
```

- `coordinate`: Specifies the location (latitude, longitude)
- `title`: Optional title shown on the marker
- `icon`: Optional marker icon as a `ByteArray` (nullable)

---

### ✅ How to Add a Marker with a Custom Icon
You can add a marker like this:

```kotlin
LaunchedEffect(Unit) {
    val playerMarker = remember {
        KrossMarker(
            coordinate = KrossCoordinate(latitude = 32.60370, longitude = 70.92179),
            title = "Player",
            icon = Res.readBytes("drawable/ic_tracker.png") // Loads the image as ByteArray
        )
    }
    mapState.addOrUpdateMarker(playerMarker)
}
```

- `Res.readBytes("drawable/ic_tracker.png")` reads the image from your `resources/drawable` folder and converts it into a `ByteArray`.
- This `ByteArray` is platform-agnostic and will be decoded natively:
  - On **Android**, into a `Bitmap`.
  - On **iOS**, into a `UIImage`.

---

### 📝 Notes
- If `icon` is `null`, the default marker icon will be used.
- Make sure the image path is correct and included in your `resources` folder.
- This approach keeps your API clean and consistent across platforms.

```kotlin
// Simple marker without icon
KrossMarker(
    coordinate = KrossCoordinate(34.0151, 71.5249),
    title = "Default Marker"
)

// Marker with custom image icon
KrossMarker(
    coordinate = KrossCoordinate(34.0151, 71.5249),
    title = "Player",
    icon = Res.readBytes("drawable/ic_tracker.png")
)
```

### 📝 Add Polyline (Route)

```kotlin
LaunchedEffect(Unit) {
    val polyline = KrossPolyLine(
        points = listOf(
            KrossCoordinate(32.60370, 70.92179),
            KrossCoordinate(32.60450, 70.92230),
            KrossCoordinate(32.60500, 70.92300),
            // Add more coordinates as needed
        ),
        title = "Route",
        color = Color.Blue,
        width = 24f
    )

    mapState.addPolyLine(polyline)
}
```

###  📝 Show the Map

```kotlin
KrossMap(
    modifier = Modifier.fillMaxSize(),
    mapState = mapState,
    cameraPositionState = cameraState,
    mapSettings = {
        MapSettings(
            onCurrentLocationClicked = {
                mapState.requestCurrentLocation()
            }
        )
    }
)
```

### 🧩 KrossMapState

`KrossMapState` is the main state holder used to manage the map in KrossMap.  
It provides support for working with markers, polylines, and current location updates.

---

### ✅ Usage

```kotlin
val mapState = rememberKrossMapState()

LaunchedEffect(Unit) {
    mapState.startLocationUpdate()
    mapState.onUpdateLocation = { location ->
        mapState.addOrUpdateMarker(KrossMarker(location, "Current"))
        cameraState.currentCameraPosition = location
    }
}
```
### 🔧 Available Functions & Properties

- `addOrUpdateMarker(marker)` – Add or update a marker on the map.
- `removeMarker(marker)` – Remove a marker from the map.
- `addPolyLine(polyLine)` – Add a polyline (route) to the map.
- `removePolyLine(polyLine)` – Remove a polyline from the map.
- `requestCurrentLocation()` – Request the current device location once.
- `startLocationUpdate()` – Start listening to location updates.
- `stopLocationUpdate()` – Stop location updates.
- `currentLocation` – Holds the last known location.
- `onUpdateLocation` – Callback triggered when location changes.

---

### 🤝 Contribute

KrossMap is open-source and welcomes contributions!  
If you find a bug, have a feature request, or want to improve the library, feel free to open an issue or submit a pull request.

Check the [issues section](https://github.com/farimarwat/krossmap/issues) for things you can help with.

---

### 👨‍💻 About Me

Hi, I’m **Farman Ullah Khan** – a passionate Android & Kotlin Multiplatform developer.  
I love building open-source tools that simplify cross-platform development and improve developer experience.

You can connect with me on [LinkedIn](https://www.linkedin.com/in/farman-ullah-marwat-a02859196/) or check out my other projects on [GitHub](https://github.com/farimarwat).

