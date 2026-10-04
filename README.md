# Playlist Button

Mini-web estática para lanzar y controlar música de Spotify en un dispositivo Spotify Connect (móvil, ordenador, altavoz) desde cualquier navegador. Pensada para usarse como botones en un wall panel (por ejemplo, un Sonoff NSPanel Pro) sin Home Assistant ni servidor propio.

Es un único archivo (`index.html`), sin dependencias ni backend. Funciona con GitHub Pages.

## Aviso legal

Proyecto personal e independiente. **No está afiliado, patrocinado ni respaldado por Spotify AB.** Spotify es una marca registrada de Spotify AB. Los nombres, carátulas y listas de reproducción pertenecen a sus titulares y se muestran mediante la API oficial de Spotify, con enlace al servicio.

El uso de la API de Spotify está sujeto a sus [Términos de desarrollador](https://developer.spotify.com/terms) y a sus [guías de diseño y marca](https://developer.spotify.com/documentation/design). La licencia MIT de este repositorio cubre únicamente el código de esta integración, no el contenido ni los servicios de Spotify.

El logotipo y el icono de Spotify (`logo/Spotify_Full_Logo_RGB_Green.png` y `logo/Spotify_Primary_Logo_RGB_Green.png`, este último usado como icono de pestaña y de pantalla de inicio) son **propiedad exclusiva de Spotify AB** y no está cubierto por la licencia MIT. Se usa solo para atribuir el contenido de Spotify, sin modificarlo, conforme a sus guías de marca (el logo verde, sobre fondo negro). Si lo sustituyes o actualizas, descárgalo siempre desde las guías oficiales de Spotify.

## Requisitos

- Cuenta de Spotify **Premium** (necesaria para controlar la reproducción por API).
- Una app creada en el [Spotify Developer Dashboard](https://developer.spotify.com/dashboard) (modo desarrollo).
- Un dispositivo que aparezca como Spotify Connect (la app de Spotify abierta en el móvil u ordenador, o un altavoz compatible).

## Puesta en marcha

Esta página **no incluye ningún Client ID**: cada persona usa el de su propia app de Spotify, que debe indicar en la dirección (`?client_id=...`). Así cada uno depende de su propia app y de sus propios usuarios autorizados.

1. Sube los archivos a un repositorio (incluida la carpeta `logo/` con el logo completo y el icono oficiales de Spotify, descargados del [kit de marca de Spotify](https://newsroom.spotify.com/media-kit/logo-and-brand-assets/)) y activa **Settings → Pages** (rama `main`, carpeta raíz). Tu URL será algo como `https://TUUSUARIO.github.io/REPO/`.
2. En el [Dashboard de Spotify](https://developer.spotify.com/dashboard) pulsa **Create app**. Rellena nombre y descripción, marca **Web API** y añade como *Redirect URI* la URL exacta de la página (con la barra final y sin parámetros).
3. En **Settings** de tu app copia el **Client ID**. No se usa Client Secret.
4. Abre `https://TUUSUARIO.github.io/REPO/?client_id=TU_CLIENT_ID`, pulsa **Iniciar sesión en Spotify** y acepta los permisos.

Si abres la página sin Client ID, mostrará el aviso **Falta el Client ID** con estos mismos pasos. El Client ID se recuerda en ese navegador, y los enlaces que genera la página para los botones del panel ya lo incluyen.

La autenticación usa Authorization Code con PKCE. Los tokens se guardan solo en el almacenamiento local del navegador donde inicias sesión.

## Uso

Sin parámetros, la página muestra un selector: desplegable de dispositivos, tus listas (con carátula y enlace a Spotify), un mando de reproducción y, al pulsar cualquier botón, el enlace equivalente para usarlo como botón fijo.

Con parámetros en la URL ejecuta una acción directa:

`https://TUUSUARIO.github.io/REPO/?device=NOMBRE&playlist=ID&autoplay=1`

| Parámetro | Descripción |
|---|---|
| `device` | Nombre (mejor) o ID del dispositivo Spotify Connect |
| `playlist` | ID, URI o enlace de Spotify (lista, álbum, artista, podcast) |
| `autoplay=1` | Ejecuta al abrir la página; sin él se muestra un botón grande |
| `shuffle` | `1` o `0` al reproducir una lista |
| `volume` | 0-100 al reproducir una lista |
| `action` | Ver tabla siguiente (sin `playlist`) |
| `list=1` | Fuerza el selector |
| `client_id` | **Obligatorio la primera vez.** Client ID de tu app de Spotify (se recuerda en el navegador; cambiarlo cierra la sesión) |
| `logout=1` | Borra la sesión y el Client ID guardados en este navegador |

En el selector hay un botón **Cerrar sesión**, que borra la sesión del usuario actual y conserva el Client ID. Al volver a iniciar sesión, Spotify pide confirmar la cuenta. No aparece en las páginas de acción (botones del panel) para evitar cierres accidentales; para cerrar sesión desde un panel, abre la página sin parámetros.

Acciones disponibles (`action=`): `prev`, `next`, `playpause`, `pause`, `resume`, `back` (−15 s), `fwd` (+15 s), `shuffle`, `shuffle_on`, `shuffle_off`, `repeat`, `repeat_off`, `repeat_context`, `repeat_track`, `vol_up`, `vol_down`.

Para botones de panel conviene usar las variantes explícitas (`pause`, `shuffle_on`...), que siempre dan el mismo resultado.

Ejemplo: `?client_id=TU_CLIENT_ID&device=Salon&action=pause&autoplay=1` (el `client_id` solo hace falta si el navegador aún no lo recuerda, pero incluirlo hace el enlace autónomo)

## Un Client ID por persona

En modo desarrollo, cada app de Spotify admite como máximo 5 usuarios autorizados (*User Management* en el Dashboard). Por eso esta página no lleva ningún Client ID incluido: quien la use debe crear su propia app y pasarla por la URL. Si inicias sesión con una cuenta que no está autorizada en esa app, la página lo avisa. Cambiar el Client ID cierra la sesión anterior, porque los tokens van ligados a cada app.

## Uso en un panel Sonoff NSPanel Pro

Añade cada URL como una tarjeta de **Webpages** en la app eWeLink (una por altavoz, lista o acción), o ábrelas con un navegador instalado en el panel.

## Limitaciones

- La API solo lista los dispositivos disponibles en ese momento; si no aparece uno, abre Spotify en él.
- El **aleatorio inteligente** de Spotify no se puede activar mediante la Web API; solo aleatorio activado/desactivado.
- En modo desarrollo, solo pueden usarla las cuentas registradas en *User Management* del Dashboard.
- Si cambias los permisos (scopes) en el código, hay que volver a iniciar sesión.

## Licencia

[MIT](LICENSE) © 2026 juanitoteleco

Excepción: el logotipo y el icono de Spotify no están bajo esta licencia (ver el apartado *Materiales de terceros* en [LICENSE](LICENSE)).
