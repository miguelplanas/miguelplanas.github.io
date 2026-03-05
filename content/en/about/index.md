---
title: "About me"
layout: about
ShowToc: true
showReadingTime: false
---

## My Background

I am a **Data Science and Engineering** student at the **Universidade da Coruña (UDC)**. My background covers the entire data lifecycle: from infrastructure to advanced analysis. I combine high-performance computing and statistics with specialized knowledge in data engineering, databases, and artificial intelligence for a more efficient and profound interpretation of data.

During the **2024/2025** academic year, I had the opportunity to do an **Erasmus+ exchange at the Politecnico di Milano**, one of the most prestigious technical universities in Europe. There, I took courses in the **MSc in Artificial Intelligence**, deepening into branches such as Deep Learning and natural language processing in a top-tier international environment. On a personal level, I also had the opportunity to meet students from all over the world, travel through Italy and Europe, and live an experience that has undoubtedly marked my life.

<div style="display: flex; justify-content: space-around; align-items: center; background: #ffffff; padding: 25px; border-radius: 12px; margin: 25px 0; gap: 30px; flex-wrap: wrap; box-shadow: 0 4px 12px rgba(0,0,0,0.08);">
  <a href="https://www.udc.es/" target="_blank" style="text-decoration: none !important; box-shadow: none !important;"><img src="/images/udc.png" alt="UDC Logo" width="300"></a>
  <a href="https://www.polimi.it/" target="_blank" style="text-decoration: none !important; box-shadow: none !important;"><img src="/images/polimi.png" alt="Polimi Logo" width="250"></a>
</div>

## Work Experience

Since **September 2025**, I have been interning at **Denodo**, a multinational company originally from A Coruña and a global leader in the data virtualization sector. This first immersion in a high-level corporate environment is allowing me to develop key soft skills while deepening my knowledge of the Denodo Platform and contributing to operational optimization by:

* **Automations Development:** Creating web applications with JavaScript and scripts in Python environments to streamline internal workflows. This development has also helped me dive deeper into **version control with git** and write high-quality, clear, and concise **internal documentation**.
* **Management and Visualization:** Technical learning on the Denodo platform and applying foundational knowledge of Power BI.
* **AI Integration:** Collaborating in the deployment of intelligent solutions applied to the improvement of business processes.

<div style="display: flex; flex-direction: column; align-items: center; background: #ffffff; padding: 25px 40px; border-radius: 16px; margin: 30px auto; box-shadow: 0 10px 30px rgba(0,0,0,0.05); max-width: fit-content; border: 1px solid rgba(0,0,0,0.03);">
  <a href="https://www.denodo.com/en" target="_blank" style="text-decoration: none !important; box-shadow: none !important;"><img src="/images/denodo.png" alt="Denodo Logo" width="220"></a>
</div>

## Recognitions
### Best Open Source AI Project & 2nd Place INDITEXTECH Challenge (HackUDC 2026)
On March 1, 2026, we were awarded **Best Open Source AI Project** and secured **second place** in the Inditex challenge at HackUDC 2026. Our solution, [**MXNJ**](/en/projects/hackudc_2026/), is a visual product recognition system developed with open source AI models.

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

You can see the full project on [Devpost](https://devpost.com/software/mxnj-inditex-hackudc2026).

### First Prize at DATATHON DIHGIGAL 2025
On January 30, 2026, alongside two university classmates, we achieved **first place** in the Datathon organized by [DIHGIGAL](https://dihgigal.com/), in collaboration with the [ITG Technology Center](https://itg.es/) and the [Universidade da Coruña](https://www.udc.es/). The jury, composed of experts from leading companies in their sectors, highlighted our project for its potential for sectorial transformation, its viability of short-term implementation, and the communicative solidity during the proposal's defense.

Our winning solution was [**MXP AutoScan**](/en/projects/mxp_autoscan/), a computer vision-based tool that automates vehicle appraisals. The system analyzes images to identify parts and damages, generating professional reports that include an immediate estimation of repair costs.

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

You can read more about our project in the [news published by El Español](https://www.elespanol.com/quincemil/economia/tecnologia/20260130/camino-datos-premia-mejores-soluciones-ia-universidad-coruna/1003744110955_0.html).

---

## Areas of Interest

During my education in Data Science and Engineering, my main interest has centered on Artificial Intelligence and its applications. In this field, I found special motivation in subjects such as Machine Learning or Artificial Neural Networks and Deep Learning, where I was able to explore the foundations of these technologies.

In parallel, I felt comfortable delving into data architecture and management, taking courses focused on database modeling, ETL systems, NoSQL databases, and the handling of spatio-temporal information.

Throughout my degree, I have been acquiring knowledge and practice with the following tools:
* **Data Engineering:** Design of pipelines and management of distributed environments. Familiarity with version control and containerization using **Git** and **Docker**, as well as data processing through **SQL** and **Apache Spark**.
* **Artificial Intelligence & Deep Learning:** Development of predictive models and advanced solutions using leading frameworks like **PyTorch** and **TensorFlow**, complemented with **Scikit-learn** for classical Machine Learning algorithms.
* **Mathematical Analysis & Visualization:** Statistical treatment and complex data manipulation with the **Python** scientific stack (NumPy, Pandas, Matplotlib...) and **R**. I can transform these analyses into visual *insights* and interactive dashboards with **Power BI**.

## Personal Interests

In my free time, I prioritize spending time with the people I love. I also enjoy sports, cinema, and music. Besides, whenever I can, I try to travel, as experiencing new cultures is something I am truly passionate about.

---

## Contact

* **LinkedIn:** [linkedin.com/in/miguelplanasdiaz](https://www.linkedin.com/in/miguelplanasdiaz)
* **Email:** [miguelplanasdiaz@gmail.com](mailto:miguelplanasdiaz@gmail.com)
* **Location:** A Coruña, Spain