
document.addEventListener("DOMContentLoaded", async () => {
  const buttons = document.querySelectorAll(".filters button");
  const projectsContainer = document.querySelector(".projects");
  let allProjects = [];

  // خواندن فایل JSON
  try {
    const response = await fetch("projects.json");
    allProjects = await response.json();
    renderProjects(allProjects);
  } catch (error) {
    console.error("خطا در دریافت JSON:", error);
  }

  // ساخت کارت‌ها
  function renderProjects(projects) {
    projectsContainer.innerHTML = ""; // پاک کردن قبلی‌ها

    projects.forEach(project => {
      const card = document.createElement("div");
      card.className = "project-card";
      card.setAttribute("data-category", project.category);

      card.innerHTML = `
        <h3 class="project-title">${project.title}</h3>
        <p class="project-description">${project.description}</p>
      `;

      projectsContainer.appendChild(card);
    });
  }

  // فیلتر دسته‌بندی
  buttons.forEach(btn => {
    btn.addEventListener("click", () => {
      const filter = btn.dataset.filter;
      const cards = document.querySelectorAll(".project-card");

      cards.forEach(card => {
        const category = card.dataset.category;
        if (filter === "all" || category === filter) {
          card.classList.remove("hide");
        } else {
          card.classList.add("hide");
        }
      });
    });
  });
});
