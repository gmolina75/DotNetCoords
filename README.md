# DotNetCoords

Una **biblioteca de clases .NET** ligera y sin dependencias externas para trabajar con sistemas de coordenadas geográficas y referencias de cuadrícula. DotNetCoords ofrece conversión bidireccional entre un par latitud/longitud (`LatLng`) y las principales referencias de coordenadas nacionales, militares y proyectadas — **OSGB**, **Irish Grid**, **UTM**, **MGRS** y **ECEF** — sobre un amplio conjunto de datums geodésicos y elipsoides de referencia.

La biblioteca es un port al .NET del conocido paquete Java *jcoord* de xyliott, conservando su API pública y sus algoritmos numéricos.

---

## Características

- **Conversión bidireccional** entre `LatLng` y `OSRef`, `IrishRef`, `UTMRef`, `MGRSRef` y `ECEFRef`.
- **Transformación de datum** entre cualquier par de datums admitidos mediante una **transformación de Helmert de 7 parámetros** (traslación, escala y rotación), con WGS84 como eje central.
- **23 datums**, entre ellos WGS84, OSGB36, ED50, ETRF89, Ireland 1965 y la familia completa de variantes regionales NAD27.
- **25 elipsoides de referencia**, desde Airy 1830 y Bessel 1841 hasta GRS 80 y WGS 84.
- **Control de precisión MGRS** de 10 km hasta 1 m, con soporte para la convención del elipsoide Bessel 1841.
- **Cálculo de distancias geodésicas** entre dos puntos `LatLng` en kilómetros o millas.
- **Representación en grados y en grados-minutos-segundos (DMS)** de la latitud y la longitud, con validación de formato integrada en todos los constructores.
- **API pública totalmente documentada en XML**, adecuada para generar IntelliSense y documentación de referencia.
- **Sin dependencias externas** — solo requiere las bibliotecas base de .NET Framework.

---

## Sistemas de coordenadas admitidos

| Tipo | Clase | Descripción |
| --- | --- | --- |
| Geográfico | `LatLng` | Latitud / longitud con altura elipsoidal, ligada a un datum (WGS84 por defecto). |
| Gran Bretaña | `OSRef` | Referencia de cuadrícula del Ordnance Survey de Gran Bretaña (British National Grid, Transverse Mercator sobre Airy 1830). |
| Irlanda | `IrishRef` | Referencia del Irish National Grid (Transverse Mercator sobre Modified Airy). |
| Universal Transverse Mercator | `UTMRef` | Este/Oeste UTM con número de zona de longitud y letra de zona de latitud. |
| Military Grid Reference System | `MGRSRef` | Cadena de coordenadas MGRS con precisión seleccionable (1 m – 10 km). |
| Earth-Centred, Earth-Fixed | `ECEFRef` | Coordenadas cartesianas geocéntricas (X, Y, Z). |

Todos los tipos de referencia derivan de la clase base abstracta `CoordinateSystem`, que expone el contrato uniforme `ToLatLng()` y transporta el `Datum` asociado.

---

## Datums admitidos

Cada datum se define por su elipsoide de referencia y por los siete parámetros de Helmert (DX, DY, DZ, DS, RX, RY, RZ) utilizados para transformar coordenadas a WGS84.

| Categoría | Datums |
| --- | --- |
| Global | `WGS84Datum` |
| Europeos | `OSGB36Datum`, `ED50Datum`, `ETRF89Datum` |
| Islas Británicas | `Ireland1965Datum` |
| North American Datum 1927 (regionales) | `NAD27Alaska`, `NAD27AlbertaBritishColumbia`, `NAD27AleutianEast`, `NAD27AleutianWest`, `NAD27Bahamas`, `NAD27Canada`, `NAD27CanadaEast`, `NAD27CanadaManitobaOntario`, `NAD27CanadaNWTerritory`, `NAD27CanadaYukon`, `NAD27CanalZone`, `NAD27Caribbean`, `NAD27CentralAmerica`, `NAD27ContiguousUS`, `NAD27Cuba`, `NAD27EasternUS`, `NAD27Greenland`, `NAD27Mexico`, `NAD27SanSalvador`, `NAD27WesternUS` |

Cada datum expone un singleton de inicialización diferida mediante `ClassName.Instance`.

---

## Elipsoides de referencia admitidos

| Elipsoide | Clase |
| --- | --- |
| Airy 1830 | `Airy1830Ellipsoid` |
| Modified Airy | `ModifiedAiryEllipsoid` |
| Australian National 1966 | `AustralianNational1966Ellipsoid` |
| Bessel 1841 | `Bessel1841Ellipsoid` |
| Clarke 1866 / 1880 | `Clarke1866Ellipsoid`, `Clarke1880Ellipsoid` |
| Everest / Modified Everest | `EverestEllipsoid`, `ModifiedEverestEllipsoid` |
| Fischer 1960 / 1968 | `Fischer1960Ellipsoid`, `Fischer1968Ellipsoid` |
| GRS 67 / 75 / 80 | `GRS67Ellipsoid`, `GRS75Ellipsoid`, `GRS80Ellipsoid` |
| Hayford 1910 | `Hayford1910Ellipsoid` |
| Helmert 1906 | `Helmert1906Ellipsoid` |
| Hough 1956 | `Hough1956Ellipsoid` |
| IERS 1989 | `IERS1989Ellipsoid` |
| International / New International 1967 | `InternationalEllipsoid`, `NewInternational1967Ellipsoid` |
| Krassovsky 1940 | `Krassovsky1940Ellipsoid` |
| South American 1969 | `SouthAmerican1969Ellipsoid` |
| WGS 60 / 66 / 72 / 84 | `WGS60Ellipsoid`, `WGS66Ellipsoid`, `WGS72Ellipsoid`, `WGS84Ellipsoid` |

---

## Estructura del proyecto

```
DotNetCoords/
├── CoordinateSystem.cs          # Clase base abstracta de todas las referencias de coordenadas
├── LatLng.cs                    # Tipo latitud/longitud, núcleo de conversión
├── OSRef.cs                     # Referencia de cuadrícula OSGB
├── IrishRef.cs                  # Referencia del Irish National Grid
├── UTMRef.cs                    # Referencia Universal Transverse Mercator
├── MGRSRef.cs                   # Referencia Military Grid Reference System
├── ECEFRef.cs                   # Referencia cartesiana geocéntrica
├── Util.cs                      # Utilidades trigonométricas internas (sin², cos³, sec, …)
├── NotDefinedOnUTMGridException.cs
├── Datum/
│   ├── Datum.cs                 # Definición abstracta de datum + base singleton genérica
│   ├── WGS84Datum.cs, OSGB36Datum.cs, ED50Datum.cs, ETRF89Datum.cs, …
│   └── nad27/                   # 20 variantes regionales del datum NAD27
└── Ellipsoid/
    ├── Ellipsoid.cs             # Definición abstracta de elipsoide de referencia
    └── *.cs                     # 25 elipsoides de referencia concretos
```

### Abstracciones clave

| Tipo | Función |
| --- | --- |
| `CoordinateSystem` | Base abstracta que expone `ToLatLng()` y el `Datum` propietario. |
| `Datum` | Nombre, elipsoide de referencia y transformación de Helmert de 7 parámetros a WGS84. |
| `Ellipsoid` | Semiejes mayor y menor; deriva el aplanamiento y la excentricidad al cuadrado. |
| `NorthSouth` / `EastWest` | Enumeraciones usadas por los constructores DMS de `LatLng`. |
| `Precision` | Niveles de precisión MGRS: 10000 m, 1000 m, 100 m, 10 m, 1 m. |

---

## Primeros pasos

### Requisitos

- Microsoft .NET Framework 4.8.1 (o Visual Studio / MSBuild compatible con este destino).
- No se requieren paquetes NuGet ni bibliotecas de terceros.

### Compilación

Abra `DotNetCoords.sln` en Visual Studio y compile, o compílelo desde la línea de comandos:

```bat
msbuild DotNetCoords.sln /p:Configuration=Release
```

El ensamblado de salida es `DotNetCoords.dll`.

### Ejemplos de uso

**Latitud / longitud a referencia de cuadrícula OSGB**

```csharp
LatLng ll = new LatLng(52.657570, 1.717909);   // WGS84 por defecto
ll.ToOSGB36();                                 // Transforma el datum a OSGB36
OSRef os = ll.ToOSRef();                       // Proyecta al British National Grid
Console.WriteLine(os.ToSixFigureString());     // p. ej. "TG514131"
```

**Referencia de cuadrícula OSGB a latitud / longitud**

```csharp
OSRef os = new OSRef("TG514131");              // Analiza una referencia de 6 cifras
LatLng ll = os.ToLatLng();                     // El resultado queda en el datum OSGB36
ll.ToWGS84();                                  // Convierte a WGS84
Console.WriteLine(ll.Latitude + ", " + ll.Longitude);
```

**Conversión UTM y MGRS**

```csharp
LatLng ll = new LatLng(52.657570, 1.717909);
UTMRef utm = ll.ToUtmRef();                    // p. ej. "31U 412261 5834780"
MGRSRef mgrs = ll.ToMGRSRef();                 // p. ej. "31UCU1226134780"
MGRSRef coarse = new MGRSRef(mgrs.ToString(Precision.Precision1000M));
```

**Distancia entre dos puntos**

```csharp
LatLng a = new LatLng(51.5074, -0.1278);       // Londres
LatLng b = new LatLng(48.8566, 2.3522);        // París
double km = a.Distance(b);                     // Aprox. 343 km
double mi = a.DistanceMiles(b);
```

**Trabajar con un datum alternativo**

```csharp
// Construye sobre ED50 y luego transforma a WGS84 mediante Helmert de 7 parámetros.
LatLng ll = new LatLng(39.5, 20.0, 0, ED50Datum.Instance);
ll.ToDatum(WGS84Datum.Instance);
```

---

## Limitaciones y notas

- La cuadrícula UTM solo está definida entre **80°S y 84°N**; `LatLng.ToUtmRef()` lanza `NotDefinedOnUTMGridException` fuera de ese rango.
- Las referencias MGRS en regiones polares (al norte de 84°N, al sur de 80°S) y las zonas UTM irregulares de Noruega y Svalbard no se tratan de forma especial.
- La transformación de datum utiliza la **aproximación de Helmert de 7 parámetros**; su precisión es del orden de unos pocos metros y no es apta para trabajos de precisión topográfica.
- Los constructores UTM solo realizan una validación básica; un este válido no garantiza que la referencia esté bien formada en cualquier latitud.
- El repositorio incluye un proyecto heredado de Windows Phone (`DotNetCoordsPhone.csproj`), conservado como referencia histórica; el destino activo es la biblioteca de clases de .NET Framework 4.8.1 (`DotNetCoords.csproj`).

---

## Créditos del proyecto original

**DotNetCoords** es un port al .NET de la biblioteca Java de código abierto **Jcoord**, cuyo autor original es **Jonathan Stott**.

> **Jcoord**
> (c) 2006 **Jonathan Stott**
> Sitio web del proyecto original: <http://www.jstott.me.uk/jcoord/>
>
> Jcoord se distribuye bajo la **GNU General Public License (GPL)**. Los términos de la licencia pueden consultarse en <http://www.jstott.me.uk/gpl/>. Cualquier uso comercial requiere la adquisición de una licencia comercial por parte del autor original.
>
> Descargo de responsabilidad del autor original: *"Accuracy of the co-ordinate conversions contained within the Jcoord package is not guaranteed. Use of the conversions is entirely at your own risk and I cannot be held responsible for any consequences of errors created by the conversions."*

DotNetCoords conserva la API pública y los algoritmos numéricos del proyecto original, adaptados al lenguaje C# y al ecosistema .NET.

La lógica de conversión de MGRS a UTM se basa en el método publicado en [stellman-greene.com/mgrs_to_utm](http://www.stellman-greene.com/mgrs_to_utm/).
