---
title: Ratatouille — Meal Planning & Recipe Companion
---

<style>
  .rt-topbar {
    display: flex;
    justify-content: flex-end;
    margin-bottom: 14px;
  }
  .rt-lang-switch {
    display: inline-flex;
    background: #f3f4f6;
    border: 1px solid #e5e7eb;
    border-radius: 999px;
    padding: 3px;
    gap: 2px;
  }
  .rt-lang-btn {
    border: none;
    background: transparent;
    color: #4b5563;
    font-size: 0.85em;
    font-weight: 600;
    padding: 5px 12px;
    border-radius: 999px;
    cursor: pointer;
    transition: background 0.15s ease, color 0.15s ease;
  }
  .rt-lang-btn:hover {
    color: #1d1d1f;
  }
  .rt-lang-btn.active {
    background: #1d1d1f;
    color: #ffffff;
  }
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

<div class="rt-topbar">
  <div class="rt-lang-switch" role="group" aria-label="Language switcher">
    <button type="button" class="rt-lang-btn" data-set-lang="en">EN</button>
    <button type="button" class="rt-lang-btn" data-set-lang="fr">FR</button>
  </div>
</div>

<div class="rt-hero">
  <img src="./ratatouille.png" alt="Ratatouille app icon" />
  <div>
    <h1 class="rt-hero-title">Ratatouille</h1>
    <p class="rt-hero-sub" data-i18n="hero.subtitle">
      Your personal kitchen assistant for effortless meal planning, cookbook recipe discovery, and smart shopping lists.
    </p>
    <div class="rt-pills">
      <span class="rt-pill" data-i18n="pills.menus">Weekly &amp; Daily Menus</span>
      <span class="rt-pill" data-i18n="pills.cookbooks">Your Cookbooks</span>
      <span class="rt-pill" data-i18n="pills.photo">Photo Ingredient Search</span>
      <span class="rt-pill" data-i18n="pills.bilingual">English &amp; Fran&ccedil;ais</span>
    </div>
  </div>
</div>

<h2 data-i18n="features.heading">Cook Smarter, Every Day</h2>

<p data-i18n="features.intro">
  Ratatouille turns the question <em>"What should we eat?"</em> into a calm, inspiring conversation. Whether you are planning a full week of family dinners or looking for a quick recipe from what is already in your fridge, Ratatouille helps you decide, shop, and cook with confidence.
</p>

<div class="rt-grid">
  <div class="rt-card">
    <h3 data-i18n="cards.plans.title">Personalized Meal Plans</h3>
    <p data-i18n="cards.plans.body">Generate balanced daily or weekly menus tailored to your tastes, dietary preferences, and household size—complete with organized shopping lists.</p>
  </div>
  <div class="rt-card">
    <h3 data-i18n="cards.cookbooks.title">Recipes From Your Cookbooks</h3>
    <p data-i18n="cards.cookbooks.body">Choose which cookbooks in your library Ratatouille should draw from, and get clear recipe cards with ingredients, servings, and original page references.</p>
  </div>
  <div class="rt-card">
    <h3 data-i18n="cards.photo.title">Snap What You Have</h3>
    <p data-i18n="cards.photo.body">Take or upload a photo of the ingredients on your counter or in your fridge. Ratatouille recognizes them and suggests diverse recipes to put them to use.</p>
  </div>
  <div class="rt-card">
    <h3 data-i18n="cards.season.title">In Season, Where You Live</h3>
    <p data-i18n="cards.season.body">Set your location and season so menu ideas naturally highlight fresh, seasonal produce available right now.</p>
  </div>
  <div class="rt-card">
    <h3 data-i18n="cards.share.title">Save &amp; Share Easily</h3>
    <p data-i18n="cards.share.body">Search past meal-planning conversations anytime, copy recipes to your clipboard in one tap, or share menus directly via WhatsApp.</p>
  </div>
  <div class="rt-card">
    <h3 data-i18n="cards.bilingual.title">Bilingual Experience</h3>
    <p data-i18n="cards.bilingual.body">Chat and browse seamlessly in English or French—Ratatouille adapts to your language throughout your planning session.</p>
  </div>
</div>

---

<h2 data-i18n="how.heading">How It Works</h2>

<div class="rt-steps">
  <div class="rt-step">
    <div class="rt-step-num">1</div>
    <h3 data-i18n="how.step1.title">Set Your Context</h3>
    <p data-i18n="how.step1.body">Pick the cookbooks you want to use and set your location and season—or jump straight into a new chat.</p>
  </div>
  <div class="rt-step">
    <div class="rt-step-num">2</div>
    <h3 data-i18n="how.step2.title">Ask or Snap a Photo</h3>
    <p data-i18n="how.step2.body">Ask for a weeknight dinner, a balanced weekly menu, or snap a photo of ingredients you want to use up.</p>
  </div>
  <div class="rt-step">
    <div class="rt-step-num">3</div>
    <h3 data-i18n="how.step3.title">Shop &amp; Cook</h3>
    <p data-i18n="how.step3.body">Review structured recipe cards, check nutritional balance, grab your shopping list, and share with family.</p>
  </div>
</div>

---

<p><em data-i18n="footer.note">Built with care for home cooks</em> &mdash; <a href="./privacy.html" data-i18n="footer.privacy">Privacy Policy</a> &middot; <a href="./terms.html" data-i18n="footer.terms">Terms of Service</a> &middot; <a href="https://sarnoud.github.io">sarnoud.github.io</a></p>

<script>
  (function () {
    const STORAGE_KEY = "ratatouille_site_lang";
    const SUPPORTED = ["en", "fr"];

    const TRANSLATIONS = {
      en: {
        pageTitle: "Ratatouille — Meal Planning & Recipe Companion",
        "hero.subtitle": "Your personal kitchen assistant for effortless meal planning, cookbook recipe discovery, and smart shopping lists.",
        "pills.menus": "Weekly & Daily Menus",
        "pills.cookbooks": "Your Cookbooks",
        "pills.photo": "Photo Ingredient Search",
        "pills.bilingual": "English & Français",
        "features.heading": "Cook Smarter, Every Day",
        "features.intro": 'Ratatouille turns the question <em>"What should we eat?"</em> into a calm, inspiring conversation. Whether you are planning a full week of family dinners or looking for a quick recipe from what is already in your fridge, Ratatouille helps you decide, shop, and cook with confidence.',
        "cards.plans.title": "Personalized Meal Plans",
        "cards.plans.body": "Generate balanced daily or weekly menus tailored to your tastes, dietary preferences, and household size—complete with organized shopping lists.",
        "cards.cookbooks.title": "Recipes From Your Cookbooks",
        "cards.cookbooks.body": "Choose which cookbooks in your library Ratatouille should draw from, and get clear recipe cards with ingredients, servings, and original page references.",
        "cards.photo.title": "Snap What You Have",
        "cards.photo.body": "Take or upload a photo of the ingredients on your counter or in your fridge. Ratatouille recognizes them and suggests diverse recipes to put them to use.",
        "cards.season.title": "In Season, Where You Live",
        "cards.season.body": "Set your location and season so menu ideas naturally highlight fresh, seasonal produce available right now.",
        "cards.share.title": "Save & Share Easily",
        "cards.share.body": "Search past meal-planning conversations anytime, copy recipes to your clipboard in one tap, or share menus directly via WhatsApp.",
        "cards.bilingual.title": "Bilingual Experience",
        "cards.bilingual.body": "Chat and browse seamlessly in English or French—Ratatouille adapts to your language throughout your planning session.",
        "how.heading": "How It Works",
        "how.step1.title": "Set Your Context",
        "how.step1.body": "Pick the cookbooks you want to use and set your location and season—or jump straight into a new chat.",
        "how.step2.title": "Ask or Snap a Photo",
        "how.step2.body": "Ask for a weeknight dinner, a balanced weekly menu, or snap a photo of ingredients you want to use up.",
        "how.step3.title": "Shop & Cook",
        "how.step3.body": "Review structured recipe cards, check nutritional balance, grab your shopping list, and share with family.",
        "footer.note": "Built with care for home cooks",
        "footer.privacy": "Privacy Policy",
        "footer.terms": "Terms of Service"
      },
      fr: {
        pageTitle: "Ratatouille — Planification de repas et recettes",
        "hero.subtitle": "Votre assistant culinaire personnel pour planifier vos repas sans effort, explorer vos livres de cuisine et créer des listes de courses.",
        "pills.menus": "Menus du jour & de la semaine",
        "pills.cookbooks": "Vos livres de cuisine",
        "pills.photo": "Recherche par photo",
        "pills.bilingual": "English & Français",
        "features.heading": "Cuisinez plus sereinement, chaque jour",
        "features.intro": "Ratatouille transforme la question <em>« Qu'est-ce qu'on mange ? »</em> en une conversation simple et inspirante. Que vous prépariez les dîners de toute la semaine ou cherchiez une idée rapide avec ce qu'il reste au frigo, Ratatouille vous aide à décider, faire vos courses et cuisiner en toute confiance.",
        "cards.plans.title": "Menus personnalisés",
        "cards.plans.body": "Générez des menus quotidiens ou hebdomadaires équilibrés, adaptés à vos goûts, vos préférences alimentaires et la taille de votre foyer, avec une liste de courses organisée.",
        "cards.cookbooks.title": "Recettes tirées de vos livres",
        "cards.cookbooks.body": "Choisissez les livres de votre bibliothèque que Ratatouille doit consulter et obtenez des fiches recettes claires avec ingrédients, portions et numéros de page.",
        "cards.photo.title": "Photographiez vos ingrédients",
        "cards.photo.body": "Prenez en photo ou importez les ingrédients présents sur votre plan de travail ou dans votre réfrigérateur. Ratatouille les identifie et vous propose des recettes variées.",
        "cards.season.title": "De saison, près de chez vous",
        "cards.season.body": "Indiquez votre lieu et la saison pour que les suggestions mettent naturellement en valeur les produits frais du moment.",
        "cards.share.title": "Enregistrez et partagez facilement",
        "cards.share.body": "Retrouvez vos anciennes discussions à tout moment, copiez une recette en un clic ou partagez vos menus directement sur WhatsApp.",
        "cards.bilingual.title": "Expérience bilingue",
        "cards.bilingual.body": "Discutez et naviguez librement en français ou en anglais : Ratatouille s'adapte à votre langue tout au long de votre session.",
        "how.heading": "Comment ça marche",
        "how.step1.title": "Définissez votre contexte",
        "how.step1.body": "Sélectionnez vos livres de cuisine ainsi que votre lieu et la saison, ou démarrez directement une nouvelle conversation.",
        "how.step2.title": "Demandez ou prenez une photo",
        "how.step2.body": "Demandez un dîner rapide, un menu équilibré pour la semaine, ou prenez en photo les ingrédients à utiliser.",
        "how.step3.title": "Faites vos courses et cuisinez",
        "how.step3.body": "Consultez vos fiches recettes structurées, vérifiez l'équilibre nutritionnel, récupérez votre liste de courses et partagez avec vos proches.",
        "footer.note": "Conçu avec soin pour cuisiner à la maison",
        "footer.privacy": "Confidentialité",
        "footer.terms": "Conditions d'utilisation"
      }
    };

    function normalizeLang(tag) {
      if (!tag) return null;
      const base = String(tag).toLowerCase().split("-")[0];
      return SUPPORTED.includes(base) ? base : null;
    }

    function detectInitialLang() {
      const param = new URLSearchParams(window.location.search).get("lang");
      const fromParam = normalizeLang(param);
      if (fromParam) return fromParam;

      try {
        const stored = normalizeLang(localStorage.getItem(STORAGE_KEY));
        if (stored) return stored;
      } catch (e) {}

      const browserLangs = navigator.languages && navigator.languages.length
        ? navigator.languages
        : [navigator.language];
      for (const l of browserLangs) {
        const match = normalizeLang(l);
        if (match) return match;
      }
      return "en";
    }

    function applyLang(lang, persist) {
      const dict = TRANSLATIONS[lang] || TRANSLATIONS.en;
      document.documentElement.lang = lang;
      document.title = dict.pageTitle;

      document.querySelectorAll("[data-i18n]").forEach((el) => {
        const key = el.getAttribute("data-i18n");
        if (dict[key] !== undefined) {
          el.innerHTML = dict[key];
        }
      });

      document.querySelectorAll("[data-set-lang]").forEach((btn) => {
        const isActive = btn.getAttribute("data-set-lang") === lang;
        btn.classList.toggle("active", isActive);
        btn.setAttribute("aria-pressed", String(isActive));
      });

      if (persist) {
        try {
          localStorage.setItem(STORAGE_KEY, lang);
        } catch (e) {}
      }
    }

    document.querySelectorAll("[data-set-lang]").forEach((btn) => {
      btn.addEventListener("click", () => {
        applyLang(btn.getAttribute("data-set-lang"), true);
      });
    });

    applyLang(detectInitialLang(), false);
  })();
</script>
