---
title: Ratatouille — Meal Planning & Recipe Companion
---

<style>
  .rt-hero {
    display: flex;
    align-items: center;
    gap: 28px;
    padding: 32px;
    margin-bottom: 32px;
    background: linear-gradient(135deg, #fff8f3 0%, #f5f5f7 100%);
    border: 1px solid #e5e7eb;
    border-radius: 18px;
  }
  .rt-hero img {
    width: 108px;
    height: 108px;
    border-radius: 24px;
    box-shadow: 0 6px 20px rgba(0, 0, 0, 0.12);
    flex-shrink: 0;
  }
  .rt-hero-title {
    margin: 0 0 8px 0 !important;
    border-bottom: none !important;
    padding-bottom: 0 !important;
    font-size: 2.2em;
    color: #1d1d1f;
  }
  .rt-hero-sub {
    margin: 0 0 14px 0;
    color: #4b5563;
    font-size: 1.1em;
    line-height: 1.5;
  }
  .rt-pills {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
  }
  .rt-pill {
    display: inline-block;
    padding: 5px 12px;
    font-size: 0.85em;
    font-weight: 600;
    color: #374151;
    background: #ffffff;
    border: 1px solid #e5e7eb;
    border-radius: 999px;
  }
  .rt-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
    gap: 18px;
    margin: 20px 0 32px 0;
  }
  .rt-card {
    padding: 20px;
    background: #ffffff;
    border: 1px solid #e5e7eb;
    border-radius: 12px;
  }
  .rt-card h3 {
    margin-top: 0 !important;
    margin-bottom: 8px;
    font-size: 1.08em;
    color: #1d1d1f;
  }
  .rt-card p {
    margin-bottom: 0;
    color: #4b5563;
    font-size: 0.95em;
    line-height: 1.55;
  }
  .rt-steps {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 16px;
    margin: 18px 0 32px 0;
  }
  .rt-step {
    padding: 18px;
    background: #f9fafb;
    border: 1px solid #e5e7eb;
    border-radius: 12px;
  }
  .rt-step-num {
    display: inline-block;
    width: 28px;
    height: 28px;
    line-height: 28px;
    text-align: center;
    font-weight: 700;
    font-size: 0.9em;
    color: #ffffff;
    background: #1d1d1f;
    border-radius: 50%;
    margin-bottom: 10px;
  }
  .rt-step h3 {
    margin-top: 0 !important;
    margin-bottom: 6px;
    font-size: 1em;
    color: #1d1d1f;
  }
  .rt-step p {
    margin-bottom: 0;
    color: #4b5563;
    font-size: 0.92em;
    line-height: 1.5;
  }
  @media (max-width: 600px) {
    .rt-hero {
      flex-direction: column;
      text-align: center;
    }
    .rt-pills {
      justify-content: center;
    }
  }
</style>

<div class="rt-hero">
  <img src="./ratatouille.png" alt="Ratatouille app icon" />
  <div>
    <h1 class="rt-hero-title">Ratatouille</h1>
    <p class="rt-hero-sub">
      Your personal kitchen assistant for effortless meal planning, cookbook recipe discovery, and smart shopping lists.
    </p>
    <div class="rt-pills">
      <span class="rt-pill">Weekly &amp; Daily Menus</span>
      <span class="rt-pill">Your Cookbooks</span>
      <span class="rt-pill">Photo Ingredient Search</span>
      <span class="rt-pill">English &amp; Fran&ccedil;ais</span>
    </div>
  </div>
</div>

## Cook Smarter, Every Day

Ratatouille turns the question *"What should we eat?"* into a calm, inspiring conversation. Whether you are planning a full week of family dinners or looking for a quick recipe from what is already in your fridge, Ratatouille helps you decide, shop, and cook with confidence.

<div class="rt-grid">
  <div class="rt-card">
    <h3>Personalized Meal Plans</h3>
    <p>Generate balanced daily or weekly menus tailored to your tastes, dietary preferences, and household size—complete with organized shopping lists.</p>
  </div>
  <div class="rt-card">
    <h3>Recipes From Your Cookbooks</h3>
    <p>Choose which cookbooks in your library Ratatouille should draw from, and get clear recipe cards with ingredients, servings, and original page references.</p>
  </div>
  <div class="rt-card">
    <h3>Snap What You Have</h3>
    <p>Take or upload a photo of the ingredients on your counter or in your fridge. Ratatouille recognizes them and suggests diverse recipes to put them to use.</p>
  </div>
  <div class="rt-card">
    <h3>In Season, Where You Live</h3>
    <p>Set your location and season so menu ideas naturally highlight fresh, seasonal produce available right now.</p>
  </div>
  <div class="rt-card">
    <h3>Save &amp; Share Easily</h3>
    <p>Search past meal-planning conversations anytime, copy recipes to your clipboard in one tap, or share menus directly via WhatsApp.</p>
  </div>
  <div class="rt-card">
    <h3>Bilingual Experience</h3>
    <p>Chat and browse seamlessly in English or French—Ratatouille adapts to your language throughout your planning session.</p>
  </div>
</div>

---

## How It Works

<div class="rt-steps">
  <div class="rt-step">
    <div class="rt-step-num">1</div>
    <h3>Set Your Context</h3>
    <p>Pick the cookbooks you want to use and set your location and season—or jump straight into a new chat.</p>
  </div>
  <div class="rt-step">
    <div class="rt-step-num">2</div>
    <h3>Ask or Snap a Photo</h3>
    <p>Ask for a weeknight dinner, a balanced weekly menu, or snap a photo of ingredients you want to use up.</p>
  </div>
  <div class="rt-step">
    <div class="rt-step-num">3</div>
    <h3>Shop &amp; Cook</h3>
    <p>Review structured recipe cards, check nutritional balance, grab your shopping list, and share with family.</p>
  </div>
</div>

---

*Built with care for home cooks — [sarnoud.github.io](https://sarnoud.github.io)*
