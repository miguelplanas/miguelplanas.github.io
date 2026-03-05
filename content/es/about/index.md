---
title: Sobre mí
layout: about
ShowToc: true
showReadingTime: false
---

## Mi Trayectoria

Soy estudiante de **Ciencia e Ingeniería de Datos** en la **Universidade da Coruña (UDC)**. Mi formación abarca todo el ciclo de vida del dato: desde la infraestructura hasta el análisis avanzado. Combino la computación de alto rendimiento y la estadística con conocimientos especializados en ingeniería de datos, bases de datos e inteligencia artificial para una interpretación de datos más eficiente y profunda.

Durante el curso **2024/2025**, tuve la oportunidad de realizar un intercambio **Erasmus+ en el Politecnico di Milano**, una de las universidades técnicas más prestigiosas de Europa. Allí cursé asignaturas del **MSc in Artificial Intelligence**, profundizando en ramas como el Deep Learning o el procesamiento de lenguaje natural en un entorno internacional de primer nivel. A nivel personal, tuve también la oportunidad de conocer a estudiantes de todas partes del mundo, viajar por Italia y Europa, y vivir una experiencia que sin duda ha marcado mi vida.

<div style="display: flex; justify-content: space-around; align-items: center; background: #ffffff; padding: 25px; border-radius: 12px; margin: 25px 0; gap: 30px; flex-wrap: wrap; box-shadow: 0 4px 12px rgba(0,0,0,0.08);">
  <a href="https://www.udc.es/" target="_blank" style="text-decoration: none !important; box-shadow: none !important;"><img src="/images/udc.png" alt="UDC Logo" width="300"></a>
  <a href="https://www.polimi.it/" target="_blank" style="text-decoration: none !important; box-shadow: none !important;"><img src="/images/polimi.png" alt="Polimi Logo" width="250"></a>
</div>

## Experiencia Laboral

Desde **septiembre de 2025**, realizo prácticas en **Denodo**, multinacional de origen coruñés y líder global en el sector de la virtualización de datos. Esta primera inmersión en un entorno corporativo de alto nivel me está permitiendo desarrollar soft skills clave mientras profundizo en la Denodo Platform y contribuyo a la optimización operativa mediante:

* **Desarrollo de Automatizaciones:** Creación de aplicaciones web con JavaScript y scripts en entornos Python para agilizar flujos de trabajo internos. Este desarrollo también me ha ayudado a profundizarme en el **control de versiones con git** y a redactar **documentación interna** de calidad, clara y concisa.
* **Gestión y Visualización:** Aprendizaje técnico sobre la plataforma Denodo y aplicación de nociones básicas de Power BI.
* **Integración de IA:** Colaboración en el despliegue de soluciones inteligentes aplicadas a la mejora de procesos empresariales.

<div style="display: flex; flex-direction: column; align-items: center; background: #ffffff; padding: 25px 40px; border-radius: 16px; margin: 30px auto; box-shadow: 0 10px 30px rgba(0,0,0,0.05); max-width: fit-content; border: 1px solid rgba(0,0,0,0.03);">
  <a href="https://www.denodo.com/en" target="_blank" style="text-decoration: none !important; box-shadow: none !important;"><img src="/images/denodo.png" alt="Denodo Logo" width="220"></a>
</div>

## Reconocimientos
### Mejor Proyecto de IA Open Source & 2º Puesto Reto INDITEXTECH (HackUDC 2026)
El 1 de marzo de 2026, logramos el premio al **Mejor Proyecto de IA Open Source** y el **segundo puesto** en el reto de Inditex durante el HackUDC 2026. Nuestra solución, [**MXNJ**](/es/projects/hackudc_2026/), es un sistema de reconocimiento visual de productos de moda desarrollado con IA open source.

<div class="hackudc-gallery-container" style="position: relative; max-width: 100%; margin: 25px auto; overflow: hidden; border-radius: 8px; box-shadow: 0 4px 12px rgba(0,0,0,0.1); background-color: #f8f9fa;">
  <div class="hackudc-gallery-slides" style="display: flex; transition: transform 0.4s ease-in-out;">
    <img src="/images/hackudc/foto1.jpg" style="min-width: 100%; object-fit: cover; aspect-ratio: 16/9;" alt="HackUDC">
    <img src="/images/hackudc/foto2.jpg" style="min-width: 100%; object-fit: cover; aspect-ratio: 16/9;" alt="HackUDC">
    <img src="/images/hackudc/foto3.jpg" style="min-width: 100%; object-fit: cover; aspect-ratio: 16/9;" alt="HackUDC">
    <img src="/images/hackudc/foto4.jpg" style="min-width: 100%; object-fit: cover; aspect-ratio: 16/9;" alt="HackUDC">
    <img src="/images/hackudc/foto5.jpg" style="min-width: 100%; object-fit: cover; aspect-ratio: 16/9;" alt="HackUDC">
    <img src="/images/hackudc/foto6.jpg" style="min-width: 100%; object-fit: cover; aspect-ratio: 16/9;" alt="HackUDC">
  </div>
  <button onclick="moveHackudcGallery(-1)" style="position: absolute; top: 50%; left: 15px; transform: translateY(-50%); background: rgba(255,255,255,0.8); border: none; border-radius: 50%; width: 40px; height: 40px; cursor: pointer; font-size: 20px; display: flex; align-items: center; justify-content: center; color: #333; box-shadow: 0 2px 4px rgba(0,0,0,0.2); transition: background 0.2s;">&#10094;</button>
  <button onclick="moveHackudcGallery(1)" style="position: absolute; top: 50%; right: 15px; transform: translateY(-50%); background: rgba(255,255,255,0.8); border: none; border-radius: 50%; width: 40px; height: 40px; cursor: pointer; font-size: 20px; display: flex; align-items: center; justify-content: center; color: #333; box-shadow: 0 2px 4px rgba(0,0,0,0.2); transition: background 0.2s;">&#10095;</button>
</div>
<script>
  let hackudcSlideIndex = 0;
  function moveHackudcGallery(step) {
    const slidesContainer = document.querySelector('.hackudc-gallery-slides');
    const totalSlides = slidesContainer.children.length;
    hackudcSlideIndex = (hackudcSlideIndex + step + totalSlides) % totalSlides;
    slidesContainer.style.transform = `translateX(-${hackudcSlideIndex * 100}%)`;
  }
</script>

Puedes ver el proyecto completo en [Devpost](https://devpost.com/software/mxnj-inditex-hackudc2026).

### Primer Premio en DATATHON DIHGIGAL 2025
El 30 de enero de 2026, junto a dos compañeros de la universidad, logramos el **primer puesto** en el Datathon organizado por [DIHGIGAL](https://dihgigal.com/), en colaboración con el [ITG Technology Center](https://itg.es/) y la [Universidade da Coruña](https://www.udc.es/). El jurado, compuesto por expertos de empresas líderes en sus sectores, destacó nuestro proyecto por su potencial de transformación sectorial, su viabilidad de implementación a corto plazo y la solidez comunicativa durante la defensa de la propuesta.

Nuestra solución ganadora fue [**MXP AutoScan**](/es/projects/mxp_autoscan/), una herramienta basada en visión por computador que automatiza el peritaje de vehículos. El sistema analiza imágenes para identificar piezas y daños, generando reportes profesionales que incluyen una estimación inmediata de los costes de reparación.

<div class="datathon-gallery-container" style="position: relative; max-width: 100%; margin: 25px auto; overflow: hidden; border-radius: 8px; box-shadow: 0 4px 12px rgba(0,0,0,0.1); background-color: #f8f9fa;">
  <div class="datathon-gallery-slides" style="display: flex; transition: transform 0.4s ease-in-out;">
    <img src="/images/datathon/datathon_1.jpg" style="min-width: 100%; object-fit: cover; aspect-ratio: 16/9;" alt="Datathon">
    <img src="/images/datathon/datathon_2.jpg" style="min-width: 100%; object-fit: cover; aspect-ratio: 16/9;" alt="Datathon">
    <img src="/images/datathon/datathon_3.jpg" style="min-width: 100%; object-fit: cover; aspect-ratio: 16/9;" alt="Datathon">
    <img src="/images/datathon/datathon_4.jpg" style="min-width: 100%; object-fit: cover; aspect-ratio: 16/9;" alt="Datathon">
    <img src="/images/datathon/datathon_5.jpg" style="min-width: 100%; object-fit: cover; aspect-ratio: 16/9;" alt="Datathon">
  </div>
  <button onclick="moveDatathonGallery(-1)" style="position: absolute; top: 50%; left: 15px; transform: translateY(-50%); background: rgba(255,255,255,0.8); border: none; border-radius: 50%; width: 40px; height: 40px; cursor: pointer; font-size: 20px; display: flex; align-items: center; justify-content: center; color: #333; box-shadow: 0 2px 4px rgba(0,0,0,0.2); transition: background 0.2s;">&#10094;</button>
  <button onclick="moveDatathonGallery(1)" style="position: absolute; top: 50%; right: 15px; transform: translateY(-50%); background: rgba(255,255,255,0.8); border: none; border-radius: 50%; width: 40px; height: 40px; cursor: pointer; font-size: 20px; display: flex; align-items: center; justify-content: center; color: #333; box-shadow: 0 2px 4px rgba(0,0,0,0.2); transition: background 0.2s;">&#10095;</button>
</div>
<script>
  let datathonSlideIndex = 0;
  function moveDatathonGallery(step) {
    const slidesContainer = document.querySelector('.datathon-gallery-slides');
    const totalSlides = slidesContainer.children.length;
    datathonSlideIndex = (datathonSlideIndex + step + totalSlides) % totalSlides;
    slidesContainer.style.transform = `translateX(-${datathonSlideIndex * 100}%)`;
  }
</script>

Puedes leer más sobre nuestro proyecto en la [noticia publicada por El Español](https://www.elespanol.com/quincemil/economia/tecnologia/20260130/camino-datos-premia-mejores-soluciones-ia-universidad-coruna/1003744110955_0.html).

---

## Áreas de interés

Durante mi formación en Ciencia e Ingeniería de Datos, mi interés principal se ha centrado en la Inteligencia Artificial y sus aplicaciones. En este ámbito, encontré especial motivación en asignaturas como Aprendizaje Automático o Artificial Neural Networks and Deep Learning, donde pude explorar los fundamentos de estas tecnologías.

Paralelamente, me sentí cómodo profundizando en la arquitectura y gestión del dato, cursando materias enfocadas al modelado de bases de datos, sistemas ETL, bases de datos NoSQL y el manejo de información espacio-temporal.

A lo largo de la carrera, he ido adquiriendo conocimientos y práctica con las siguientes herramientas:
* **Ingeniería de Datos:** Diseño de pipelines y gestión de entornos distribuidos. Familiaridad con el control de versiones y la contenerización mediante **Git** y **Docker**, así como el procesamiento de datos mediante **SQL** y **Apache Spark**.
* **Inteligencia Artificial & Deep Learning:** Desarrollo de modelos predictivos y soluciones avanzadas utilizando frameworks líderes como **PyTorch** y **TensorFlow**, complementados con **Scikit-learn** para algoritmos de Machine Learning clásico.
* **Análisis Matemático & Visualización:** Tratamiento estadístico y manipulación de datos complejos con el stack científico de **Python** (NumPy, Pandas, Matplotlib...) y **R**. Puedo transformar estos análisis en *insights* visuales y cuadros de mando interactivos con **Power BI**.

## Intereses personales

En mi tiempo libre, priorizo pasar tiempo con la gente que quiero. También disfruto el deporte, el cine y la música. Además, siempre que puedo intento viajar, ya que conocer nuevas culturas es algo que realmente me apasiona.

---

## Contacto

* **LinkedIn:** [linkedin.com/in/miguelplanasdiaz](https://www.linkedin.com/in/miguelplanasdiaz)
* **Email:** [miguelplanasdiaz@gmail.com](mailto:miguelplanasdiaz@gmail.com)
* **Ubicación:** A Coruña, España