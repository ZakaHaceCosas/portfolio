---
layout: ../../layouts/Project.astro
titleES: Kantra
titleEN: Kantra
id: kantra
descriptionES: "«Controla quién entra» - Sistema de Acceso Controlado local, basado en la web."
descriptionEN: '"Control who enters" - Self-hosted Access Control System, based on the web.'
color: "#C55FFF"
chapters:
    - depth: 1
      titleES: "Qué es y qué hace"
      hrefES: "qué-es-y-qué-hace"
      titleEN: "What it is and what it does"
      hrefEN: "what-it-is-and-what-it-does"
    - depth: 2
      titleES: "La arquitectura"
      hrefES: "la-arquitectura"
      titleEN: "The architecture"
      hrefEN: "the-architecture"
    - depth: 1
      titleES: "Desarrollo"
      hrefES: "desarrollo"
      titleEN: "Development"
      hrefEN: "development"
    - depth: 2
      titleES: "La pila tecnológica"
      hrefES: "la-pila-tecnológica"
      titleEN: "Tech stack"
      hrefEN: "the-technology-stack"
    - depth: 2
      titleES: "Modelo de distribución"
      hrefES: "modelo-de-distribución"
      titleEN: "distribution-model"
      hrefEN: "distribution-model"
    - depth: 1
      titleES: "Funcionalidades"
      hrefES: "funcionalidades"
      titleEN: "Features"
      hrefEN: "features"
    - depth: 1
      titleES: "Disponibilidad"
      hrefES: "disponibilidad"
      titleEN: "Availability"
      hrefEN: "availability"
    - depth: 1
      titleES: "Capturas de pantalla"
      hrefES: "capturas-de-pantalla"
      titleEN: "Screenshots"
      hrefEN: "screenshots"
---

<esp>

# Qué es y qué hace

Kantra es una aplicación web que sirve como SAC; es decir, programa para registrar entradas y salidas de un edificio. Kantra está específicamente diseñado para el uso educativo, por lo que permite, por ejemplo, filtrar entre alumnos, docentes o personal del centro. También es apropiado para centros educativos ya que es totalmente privado, no porque lo prometa yo sino porque los centros usuarios tienen que alojar el software en sus propios servidores (labor para la que servirá un ordenador que esté encendido siempre que haya alguien dentro del centro; Kantra no es la gran cosa en exigencias de hardware), y de todos modos requiere HTTPS para cifrado en tránsito y una clave maestra usada para cifrado en reposo. Solo quien conozca la clave (se presupone alguien fiable dentro del centro) puede acceder a los datos, o conceder a otros usuarios autentificados el permiso para consultarlos desde la app.

<br>

Lo desarrollé con un compañero[^1].

## La arquitectura

Es bastante sencilla, y eso es intencional. Por su naturaleza, Kantra no espera cargas de trabajo intensas o picos de concurrencia, por lo que nos podemos permitir un diseño mucho más plano que sacrifique «migajas» de rendimiento a cambio de un desarrollo mucho más fácil y rápido.

<br>

Kantra sigue una arquitectura **cliente-servidor-BD**, los clientes (bien la interfaz web o la línea de comandos) se comunican mediante HTTP a un servidor central (alojado por el centro), que hace todo el trabajo complejo y orquesta una base de datos SQLite que procesa todos los datos.

<br>

Como decía, Kantra se va a usar en colegios para registrar entradas y salidas a lo largo del día (cosa que, por diseño, no incluye la entrada/salida de todos los alumnos a las nueve y a las tres; _solo_ todo lo demás), así que tampoco hacen falta cosas más complejas. Esto bastará.

# Desarrollo

## La pila tecnológica

No es nada sobre lo que vaya a escribir demasiado.

<br>

**Svelte(Kit) para la interfaz gráfica.** No hay mucho que explicar respecto a la decisión, Svelte es en líneas generales (y eso incluye mi opinión, claro) la mejor opción para casi cualquier proyecto web.

<br>

**Bun para el servidor.** Elegí usar JavaScript del lado del servidor ya que al estar la interfaz hecha también en JS, a partir de un mono-repo toda la lógica de validación y de tipado son un tercer repositorio compartido entre ambos, eliminando duplicidad y discrepancias (cosas que en el pasado dieron bastante guerra, por lo despistados que somos...)

<br>

**C# para la línea de comandos.** ¡Sí, hay una _CLI_! La «`kantractl`», se utiliza principalmente como gestor de instalación y actualización del programa, aunque a futuro prevemos permitir su uso para interactuar con todo el software desde la terminal. Elegí C# por ser un lenguaje bastante disfrutable y por .NET, que me lo pone muy fácil para enchufar un kit como Avalonia para en el futuro darle interfaz gráfica al instalador/actualizador (o incluso portar Kantra de web a nativo para reducir exigencias, quién sabe).

## Modelo de distribución

La idea es muy sencilla, tú te descargas el programa `kantractl`, y este te explica paso a paso que cosas hacer en tu ordenador o red (si tienes que abrir puertos, instalar certificados o lo que sea), para luego encargarse este por sí mismo de instalar dependencias (Bun, básicamente), descargar la última versión de Kantra, generar y exportar certificados HTTPS locales, y arrancar el servidor.

<br>

Sobre si debería hacer todo en la máquina directamente o instalar una solución de contenedorización y meter Kantra en un contenedor, es algo que aún debatimos, pero por lo pronto lo hace todo en la máquina directamente.

# Funcionalidades

- Registro de entradas, salidas y recogidas de hijos/as
    - Diferencia docentes, alumnos, trabajadores, o visitantes (mas «otros»)
    - Agrupación por puertas
    - Registra acompañantes o hijos/as
    - Ofrece tique a imprimir (formatos Zebra y PDF)
    - Ofrece gafete a imprimir en papel (formato PDF)
    - Permite añadir notas adicionales por si algo se da he esclarecer
    - Una vez registrada una entrada, con un clic se auto-registra la salida de quien haya entrado a la hora actual (u otra a especificar)
- Registro interactivo en el que la propia persona anota los datos
    - Muestra indicaciones de privacidad (R.G.P.D.) y plan de evacuación (a determinar por el centro)
    - Dicha interfaz es personalizable con colores y logotipos del centro
- Visualización de los datos, agrupados por fecha
- Interfaz para filtrar datos de múltiples maneras (por tipo de autor, por DNI)
- Generación de informes (formato PDF) con todos los movimientos asociados a una o varias personas
- Prevención de borrado de datos con menos de 5 años de antigüedad (requerido por ley)

# Disponibilidad

Kantra no está disponible todavía.

<br>

Lo estamos desarrollando entre dos personas[^1] como proyecto escolar adicional, se probará inicialmente en nuestro centro y en caso de demostrarse funcional (que debería) se lanzará como producto con disponibilidad general, momento en que actualizaré esta sección.

# Capturas de pantalla

Pág de inicio con datos:

<img src="/screens/kantra_1.png" alt="Captura 1" class="proj-d-png" />

Registrar entrada desde recepción:

<img src="/screens/kantra_2.png" alt="Captura 2" class="proj-d-png" />

Auto-registro:

<img src="/screens/kantra_3.png" alt="Captura 3" class="proj-d-png" />

(Cabe aclarar que estas son viejas, hace no demasiado que empezamos a re-diseñar Kantra usando un diseño propio en vez de Microsoft Fluent 2; actualizaré las capturas en cuanto esté terminado)

</esp>

<eng>

# What it is and what it does

Kantra is a web application that serves as an ACS; that is, a program to register entries and exits of a building. Kantra is specifically designed for educational use, so it allows, for example, filtering between students, teachers, or center staff. It is also suitable for educational centers since it is totally private, not because I promise it but because user centers must host the software on their own servers (task for which a computer that stays on whenever there is someone inside the center will do; Kantra is not demanding on hardware requirements whatsoever), and anyways requires HTTPS for encryption in transit and a master key used for encryption at rest. Only those who know the key (presumably someone trustworthy within the center) can access the data, or grant authenticated users permission to query it from the app.

<br>

I developed it with a partner[^1].

## The architecture

It is quite simple, and that is intentional. By its nature, Kantra does not expect intensive workloads or concurrency spikes, so we can afford a much flatter design that sacrifices "crumbs" of performance in exchange for much easier and faster development.

<br>

Kantra follows a **client-server-DB** architecture; clients (either the web interface or the command line) communicate via HTTP to a central server (hosted by the center), which does all the complex work and orchestrates a SQLite database that processes all data.

<br>

As I was saying, Kantra will be used in schools to register entries and exits throughout the day (something that, by design, does not include the entry/exit of all students at nine and at three; _only_ everything else), so we don't need more complex things anyway. This will do.

# Development

## The technology stack

There's not too much I can write about.

<br>

**Svelte(Kit) for the graphical interface.** There's not much to explain regarding the decision; Svelte is generally (and that includes my opinion, of course) the best option for almost any web project.

<br>

**Bun for the server.** I chose to use JavaScript on the server side since, with the interface also done in JS, by doing a monorepo all validation and typing logic becomes a third shared repository between both, eliminating duplication and discrepancies (things that in the past gave quite a bit of trouble, for how distracted we both are…)

<br>

**C# for the command line.** Yes, there is a _CLI_! The "`kantractl`", mainly used as an installation and program update manager, although in the future we foresee allowing its use to interact with the whole software from the terminal. I chose C# for being a quite enjoyable language and for .NET, which makes it very easy to plug in a kit like Avalonia to give the installer/updater a graphical interface in the future (or even port Kantra from web to native to reduce hardware requirements, who knows).

## Distribution model

The idea is very simple: you download the `kantractl` program, and it explains step by step what things to do on your computer or network (if you have to open ports, install certificates or whatever), then it takes care of installing dependencies (Bun, basically), downloading the latest version of Kantra, generating and exporting local HTTPS certificates, and starting the server.

<br>

About whether I should do everything directly on the machine or install a containerization solution and put Kantra in a container, that's something we still debate, but for now it does everything directly on the machine.

# Features

- Registration of entries, exits, and pick-ups of children
    - Differentiates teachers, students, workers, or visitors (plus "others")
    - Grouping by doors
    - Registers companions or children
    - Offers receipt to print (Zebra and PDF formats)
    - Offers badge to print on paper (PDF format)
    - Allows adding additional notes if something happens to clarify it
    - Once an entry is registered, with one click it auto-registers the exit of whoever entered at the current time (or another to specify)
- Interactive registration where the person themselves enters the data
    - Shows privacy indications (G.D.P.R.) and evacuation plan (to be determined by the center)
    - Said interface is customizable with colors and center logo
- Visualization of data, grouped by date
- Interface to filter data in multiple ways (by author type, by ID number)
- Report generation (PDF format) with all movements associated with one or several people
- Prevention of deletion of data less than 5 years old (required by law)

# Availability

Kantra is not available yet.

<br>

We are developing it between two people[^1] as an additional school project; it will initially be tested in our center and if it proves functional (which it should) it will launch as a product with general availability, at which point I will update this section.

# Screenshots

Home page with data:

<img src="/screens/kantra_1.png" alt="Screenshot 1" class="proj-d-png" />

Register entry from reception:

<img src="/screens/kantra_2.png" alt="Screenshot 2" class="proj-d-png" />

Self-registration:

<img src="/screens/kantra_3.png" alt="Screenshot 3" class="proj-d-png" />

(It should be clarified that these are old ones; not too long ago we started re-designing Kantra using our own design instead of Microsoft Fluent 2; I will update the screenshots as soon as it is finished)

</eng>

[^1]:
    <p><esp>Créditos a mi compañero <a href="https://mariomadorran-portfolio.vercel.app/" target="_blank">Mario</a> por aportar gran parte del desarrollo del proyecto.</esp><eng>Credits to my partner <a href="https://mariomadorran-portfolio.vercel.app/" target="_blank">Mario</a> for contributing much of the project development.</eng></p>
