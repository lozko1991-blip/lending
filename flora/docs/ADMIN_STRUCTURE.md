# Karina Flowers — Структура адмінки (admin.html)

> **Файл:** `admin.html` | **Рядків:** ~599 | **Кодування:** UTF-8

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
```

---

## Екрани

### Auth Screen (#authScreen)
- Логін форма: `#loginInput`, `#passwordInput`
- OnSubmit: `handleAuth(event)`
- Посилання повернення на сайт (index.html)

### Dashboard Screen (#dashboardScreen)
- Заголовок + кнопки: "Переглянути сайт", "Вийти"
- 4 вкладки (таби)
- Кнопка "Зберегти та Застосувати" внизу

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
| `loadConfig()` | Завантажує конфіг з localStorage |
| `showDashboard()` | Заповнює всі поля форми з siteConfig, рендерить таби |
| `switchTab(tabId, btn)` | Перемикає вкладки |

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
sessionStorage("admin_authenticated")
  └─ true → showDashboard()
  └─ false → auth screen

localStorage("karina_admin_config")
  ├─ Читання: loadConfig() → siteConfig
  └─ Запис: saveSettings() → JSON.stringify(siteConfig)

siteConfig
  ├─ tabNotifications: telegram, viber, phone, email
  ├─ tabServices: services[] + renderServicesTab()
  ├─ tabPhotos: gallery[] + renderPhotosTab()
  └─ tabContent: heroTitle, heroSubtitle, aboutTitle, aboutLead
```
