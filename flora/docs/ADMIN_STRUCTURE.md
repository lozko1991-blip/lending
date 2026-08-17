# Karina Flowers — Структура адмінки (admin.html)

> **Файл:** `admin.html` | **Рядків:** ~890 | **Кодування:** UTF-8
> **Актуалізовано:** 2026-08-17 (GitHub-синхронізація + редактор Відгуків & FAQ)

---

## 🌍 Автопублікація на GitHub (центральна фіча)

**Призначення:** зміни з адмінки публікуються в репозиторій GitHub → сайт читає їх → всі пристрої бачать оновлення.

**Механіка:**
- При "Зберегти" адмінка завантажує всі base64-фото на GitHub (`flora/images/photo_N.jpg`) та оновлює `flora/site-data.json`
- Сайт (`index.html`) при завантаженні читає `site-data.json` (відносний шлях) → застосовує дані
- Якщо `site-data.json` недоступний (локально) → fallback на localStorage → дефолти
- Деплой GitHub Pages ~1-2 хв, після чого зміни бачать усі

**🔑 Активація токена (важливо!):**
- Токен **НЕ зберігається в коді** (GitHub Push Protection блокує публікацію секретів)
- Для активації відкрити один раз: `admin.html?token=ВАШ_ТОКЕН`
- Токен зберігається в localStorage `karina_github_settings` (на пристрої)
- Токен автоматично прибирається з URL (history.replaceState)
- На кожному новому пристрої — активація повторюється 1 раз

**Поля (вкладка "Контакти", блок "Автопублікація на GitHub"):**
| ID | Призначення |
|----|-------------|
| `cfgGithubToken` | GitHub Token (localStorage `karina_github_settings`) |
| `cfgGithubOwner` | Власник репо (default: `lozko1991-blip`) |
| `cfgGithubRepo` | Назва репо (default: `lending`) |
| `cfgGithubBranch` | Гілка (default: `main`) |

**Функції:**
| Функція | Призначення |
|---------|-------------|
| `loadGithubSettings()` | Читає налаштування GitHub з localStorage |
| `saveGithubSettingsToStorage()` | Зберігає налаштування GitHub |
| `githubHeaders(gh)` | Формує Authorization header |
| `githubUploadImage(gh, base64, name)` | PUT фото в `flora/images/` |
| `githubGetExistingSha(gh, path)` | Отримує SHA файлу для оновлення |
| `githubPushSiteData(gh)` | PUT `flora/site-data.json` |
| `uploadAllBase64Photos(gh)` | Завантажує всі base64-фото конфігу як файли |
| `setPublishStatus(text, type)` | Показує індикатор статусу (ok/err/wait) |

**Потік збереження (`saveSettings`):**
```
Збір полів → localStorage → статус "⏳ Публікую..." 
→ зберегти github налаштування 
→ uploadAllBase64Photos (фото) 
→ githubPushSiteData (site-data.json) 
→ статус "🚀 ОПУБЛІКОВАНО" (зелений) / "❌ Помилка" (червоний)
```

---

## Аутентифікація

- **Логін:** `admin_2026` / **Пароль:** `AdMin__2026`
- **sessionStorage ключ:** `admin_authenticated` (значення `"true"`)
- При завантаженні: якщо ключ є, одразу показується dashboard
- Кнопка "Вийти" видаляє ключ і показує auth-екран

---

## Конфігурація (DEFAULT_CONFIG)

Зберігається у localStorage ключ `karina_admin_config`. Кнопка "Зберегти" серіалізує весь siteConfig у JSON.

```
telegramToken       — string (бот токен)
telegramChatId      — string (ID чату)
telegramUsername    — string (@username)
phone               — string (телефон)
email               — string
viberPhone          — string
viberEnabled        — boolean
heroTitle           — string (HTML дозволено)
heroSubtitle        — string
aboutTitle          — string (HTML дозволено)
aboutLead           — string
heroPhoto           — string (base64 URL)
aboutPhoto          — string (base64 URL)
services            — DEFAULT_SERVICES[]
gallery             — DEFAULT_GALLERY[]
reviews             — DEFAULT_REVIEWS[]
faq                 — DEFAULT_FAQ[]
```

---

## Екрани

### Auth Screen (#authScreen)
- Логін форма: `#loginInput`, `#passwordInput`
- OnSubmit: `handleAuth(event)`
- Посилання повернення на сайт (index.html)

### Dashboard Screen (#dashboardScreen)
- Заголовок + кнопки: "Переглянути сайт", "Вийти"
- 5 вкладок (таби)
- Кнопка "Зберегти та Застосувати" внизу
- `#publishStatus` — індикатор статусу публікації (під кнопкою)

---

## Вкладки (таби)

### Tab 1 — Контакти & Telegram (#tabNotifications)
- **Telegram Bot:** `#cfgTelegramToken`, `#cfgTelegramChatId`
- **Месенджери:** `#cfgTelegramUsername`, `#cfgViberPhone`
- **Чекбокс:** `#cfgViberEnabled` — показувати Viber на сайті
- **Контакти:** `#cfgPhone`, `#cfgEmail`

### Tab 2 — Послуги (#tabServices)
- Кнопка "+ Додати послугу" → `addNewServicePrompt()`
- `#adminServicesContainer` — динамічний список карток
- Кожна картка: назва, ціна, опис, прив'язаний альбом, кнопка "Видалити"
- Inline редагування через `onchange` на полях

### Tab 3 — Альбоми & Фото (#tabPhotos)
- Завантаження з ПК/Телефону: `<input type="file">` → `uploadGalleryImage(this)`
- Додавання по URL: кнопка → `addNewPhotoByUrlPrompt()`
- `#adminGalleryContainer` — динамічний список карток
- Кожна картка: прев'ю 90×90px, назва, альбом, URL, кнопки "Замінити" та "Видалити"
- `optimizeAndUploadImage(file, callback)` — ресайз до 1400px + JPEG 85%

### Tab 4 — Тексти & Контент (#tabContent)
- `#cfgHeroTitle` — заголовок Hero (HTML)
- `#cfgHeroSubtitle` — підзаголовок Hero
- `#cfgAboutTitle` — заголовок About
- `#cfgAboutLead` — текст About

### Tab 5 — Відгуки & FAQ (#tabReviews) — НОВЕ
- **Відгуки:** `#adminReviewsContainer`, кнопка "+ Додати відгук"
  - Кожна картка: ім'я, роль, текст, оцінка (1-5), URL аватара, кнопка "Видалити"
- **FAQ:** `#adminFaqContainer`, кнопка "+ Додати питання"
  - Кожна картка: питання (q), відповідь (a), кнопка "Видалити"
- Функції: `renderReviewsTab()`, `addNewReview()`, `renderFaqTab()`, `addNewFaq()`

---

## JavaScript функції

### Auth
| Функція | Призначення |
|---------|-------------|
| `handleAuth(e)` | Перевіряє логін/пароль, встановлює sessionStorage |
| `logout()` | Видаляє sessionStorage, показує auth-екран |

### Ініціалізація
| Функція | Призначення |
|---------|-------------|
| `loadConfig()` | Асинхронно завантажує конфіг: спершу site-data.json з GitHub, потім localStorage, потім дефолти |
| `showDashboard()` | Заповнює всі поля форми з siteConfig, рендерить всі 5 табів |
| `switchTab(tabId, btn)` | Перемикає вкладки |

### Відгуки & FAQ (Tab 5)
| Функція | Призначення |
|---------|-------------|
| `renderReviewsTab()` | Рендерить картки відгуків |
| `addNewReview()` | Додає новий відгук |
| `renderFaqTab()` | Рендерить картки FAQ |
| `addNewFaq()` | Додає нове питання |

### Послуги
| Функція | Призначення |
|---------|-------------|
| `renderServicesTab()` | Рендерить картки послуг |
| `addNewServicePrompt()` | Додає нову послугу |

### Фотоальбоми
| Функція | Призначення |
|---------|-------------|
| `renderPhotosTab()` | Рендерить картки фото |
| `uploadGalleryImage(input)` | Завантажує фото з пристрою |
| `addNewPhotoByUrlPrompt()` | Додає фото по URL |
| `replaceGalleryImage(index, input)` | Замінює фото |
| `optimizeAndUploadImage(file, cb)` | Ресайз + конвертація в base64 JPEG |

### Збереження
| Функція | Призначення |
|---------|-------------|
| `saveSettings()` | Збирає всі поля в siteConfig, зберігає в localStorage |

---

## Структура DEFAULT_SERVICES (4 послуги)

```js
{
  id: "s1".."s4",
  tag: "Популярне" | "Для Бізнесу..." | "Особливий День" | "VIP / WOW Ефект",
  title: string,
  description: string,
  slogan: string,
  price: string,
  priceNote: string,
  highlight: boolean,
  albumName: string
}
```

## Структура DEFAULT_GALLERY

```js
{
  src: string,        // base64 або URL
  title: string,      // назва фото
  cat: string,        // ключ категорії (mk, wedding, wow)
  albumName: string   // назва альбому
}
```

## Структура DEFAULT_REVIEWS

```js
{
  id: string,
  name: string,
  role: string,
  text: string,
  stars: number (1-5),
  avatar: string (base64)
}
```

## Структура DEFAULT_FAQ

```js
[
  { "q": "Питання...", "a": "Відповідь..." },
  ...
]
```

---

## CSS (десктоп)

| Клас | Призначення |
|------|-------------|
| `.admin-container` | Біла картка 1040px, тінь, border-radius 24px |
| `.auth-box` | Форма входу 420px |
| `.btn` / `.btn-primary` / `.btn-outline` / `.btn-danger` / `.btn-sm` | Кнопки |
| `.nav-tabs` | Горизонтальні таби, overflow-x: auto |
| `.tab-btn` / `.tab-btn.active` | Кнопки табів |
| `.tab-content` / `.tab-content.active` | Вміст табів (display:none/block) |
| `.card-item` | Картка послуги/фото |
| `.grid-2` | 2-колонковий grid |
| `.form-group` / `.form-label` / `.form-input` / `.form-select` / `.form-textarea` | Форми |

---

## Мобільна версія (≤768px)

| Елемент | Десктоп | Мобільний |
|---------|---------|-----------|
| body | padding 40px 20px | padding 8px |
| .admin-container | padding 40px, max-width 1040px | padding 12px, 100% |
| .auth-box | margin 60px, padding 40px | margin 20px, padding 24px |
| Таби | горизонтально | горизонтальний swipe |
| .grid-2 | 2 колонки | 1 колонка |
| Кнопки | inline-flex | 100% ширина, min-height 48px |
| Inputs | 0.95rem | font-size 16px (без iOS zoom) |
| .card-item | padding 20px | padding 16px |

---

## Потік даних

```
GitHub API (коли є токен):
  PUT flora/images/photo_N.jpg  — завантаження фото
  PUT flora/site-data.json      — публікація конфігу
  └─ GitHub Pages деплой (~1-2 хв) → зміни видно всім

sessionStorage("admin_authenticated")
  └─ true → showDashboard()
  └─ false → auth screen

localStorage:
  "karina_admin_config"    — основний конфіг (тексти, послуги, галерея, відгуки, FAQ)
  "karina_github_settings" — GitHub owner/repo/branch/token

siteConfig
  ├─ tabNotifications: telegram, viber, phone, email, github
  ├─ tabServices: services[] + renderServicesTab()
  ├─ tabPhotos: gallery[] + renderPhotosTab()
  ├─ tabContent: heroTitle, heroSubtitle, aboutTitle, aboutLead
  └─ tabReviews: reviews[] + faq[] + renderReviewsTab() + renderFaqTab()
```
