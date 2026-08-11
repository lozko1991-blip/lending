# Karina Flowers — Структура сайту (index.html)

> **Файл:** `index.html` | **Рядків:** ~1411 | **Кодування:** UTF-8

---

## Конфігурація (DEFAULT_CONFIG)

Зберігається у localStorage ключ `karina_admin_config`. При завантаженні мерджиться поверх `DEFAULT_CONFIG`.

```
telegramToken       — string (бот токен)
telegramChatId      — string (ID чату)
telegramUsername    — string (@username)
phone               — string (телефон)
email               — string
viberPhone          — string
viberEnabled        — boolean
fontFamily          — string (CSS)
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

## Секції сайту (зверху вниз)

### 1. Speed Dial (фіксована кнопка)
- `#speedDialMenu` — приховане меню (Viber, Telegram, Дзвінок)
- `#callFloatingBtn` — зелена кнопка з пульсацією, z-index 1050
- OnClick: `toggleSpeedDial()`

### 2. Header (фіксований)
- Логотип "Karinka Flowers"
- Навігація: Про мене (#about), Послуги (#services), Портфоліо (#portfolio), Контакти (#contact)
- `.header-cta` — кнопка "Замовити дзвінок" → `scrollToContactForm('Замовити дзвінок')`
- `.mobile-menu-btn` — бургер (тільки на mobile), `toggleMobileMenu()`
- z-index: 1000

### 3. Hero (#hero)
- `#displayHeroTitle` — заголовок h1
- `#displayHeroSubtitle` — підзаголовок
- `#heroMainPhoto` — головне фото
- Кнопка "Обрати послугу" → #services

### 4. About (#about)
- `#aboutPhoto` — фото
- `#displayAboutTitle` — заголовок h2
- `#displayAboutLead` — текст

### 5. Services (#services)
- `#dynamicServicesGrid` — динамічний контейнер карток
- 4 послуги: Майстер-класи, B2B Навчання, Весільна флористика, WOW-доставка
- Кожна картка: тег, назва, опис, міні-галерея (до 4 фото), ціна, кнопка "Замовити послугу" → `openModal()`

### 6. Portfolio (#portfolio)
- `#dynamicAlbumFilterBtns` — динамічні кнопки фільтрів альбомів
- `#galleryGrid` — сітка фото
- 11 фото початково, фільтрація по альбомах

### 7. Reviews (#reviews)
- `#dynamicReviewsGrid` — сітка карток відгуків
- Зірочки, текст, аватар, ім'я, роль

### 8. FAQ (#faq)
- 4 питання-відповіді (акордеон): весільний декор, майстер-класи для новачків, довговічність букетів, доставка
- OnClick: `toggleFAQ(this)`

### 9. Contact (#contact)
- `#displayPhoneLink` — телефон
- `#displayEmailText` — email
- `#mainContactForm` — форма заявки
  - `#clientName` (text, required)
  - `#clientPhone` (tel, required)
  - `#serviceSelect` (select)
  - `#clientMessage` (textarea)
  - OnSubmit: `handleFormSubmit(event)` → Telegram + mailto:

### 10. Footer
- © 2026 Karina Flowers
- Лінк на адмінку

---

## Модальні вікна

| ID | Призначення | z-index | Як відкрити |
|----|-------------|---------|-------------|
| `#mobileNavDrawer` | Мобільне меню | 9998 | `toggleMobileMenu()` |
| `#successModal` | Успішна відправка | 9999 | `showSuccessModal(name)` |
| `#orderModal` | Замовлення послуги | 9999 | `openModal(serviceName)` |
| `#lightboxModal` | Перегляд фото | 2000 | `openLightboxByIndex(index)` |

### Order Modal форма
- `#modalTitle` — назва послуги
- `#modalName` — ім'я (text, required)
- `#modalPhone` — телефон (tel, required)
- `#modalDetails` — деталі (textarea)
- OnSubmit: `handleModalSubmit(event)` → Telegram + mailto: + close + success modal

---

## JavaScript функції

### Ініціалізація
| Функція | Призначення |
|---------|-------------|
| `loadSiteConfig()` | Завантажує конфіг з localStorage, викликає applyConfigToDOM |
| `applyConfigToDOM()` | Оновлює всі DOM-елементи: телефони, тексти, фото, рендерить каталоги |

### Рендеринг
| Функція | Призначення |
|---------|-------------|
| `renderServicesCatalog()` | Рендерить картки послуг у #dynamicServicesGrid |
| `renderPortfolioGallery()` | Рендерить портфоліо у #portfolioGrid |
| `renderGalleryAndAlbums()` | Рендерить галерею + фільтри у #galleryGrid + #dynamicAlbumFilterBtns |
| `renderReviews()` | Рендерить відгуки у #dynamicReviewsGrid |

### Форми та модали
| Функція | Призначення |
|---------|-------------|
| `handleFormSubmit(e)` | Обробка контактної форми → Telegram + mailto: + success modal |
| `openModal(name)` | Відкриває order modal |
| `closeModal(e)` | Закриває order modal |
| `handleModalSubmit(e)` | Обробка модальної форми → Telegram + mailto: |
| `showSuccessModal(name)` | Показує success modal |
| `closeSuccessModal(e)` | Закриває success modal |
| `scrollToContactForm(name)` | Скрол до форми + встановлює serviceSelect |

### Telegram
| Функція | Призначення |
|---------|-------------|
| `sendTelegramMessage(text)` | POST до Telegram Bot API |

### Галерея / Lightbox
| Функція | Призначення |
|---------|-------------|
| `openLightboxByIndex(index)` | Відкриває фото за індексом |
| `prevLightboxImage()` | Попереднє фото |
| `nextLightboxImage()` | Наступне фото |
| `closeLightbox(e)` | Закриває лайтбокс |
| `openServicePhotoLightbox(src, title)` | Лайтбокс для фото з картки послуги |

### Навігація
| Функція | Призначення |
|---------|-------------|
| `toggleMobileMenu()` | Відкриває/закриває мобільне меню |
| `navigateToSection(id)` | Закриває меню + скрол до секції |
| `toggleSpeedDial()` | Відкриває/закриває speed dial |

### Інше
| Функція | Призначення |
|---------|-------------|
| `toggleFAQ(btn)` | Акордеон FAQ |
| `filterPortfolio(cat, btn)` | Фільтр портфоліо (всі/букети/весільні/майстер-класи) |
| `expandMobilePortfolio()` | "Переглянути всі" на мобільному |
| `sendEmailNotification(...)` | Відкриває mailto: лінк |

---

## CSS змінні (кольори)

```
--bg-ivory: #FAF6F3      — фон сторінки
--bg-warm: #FFFBF9       — теплий фон для секцій
--bg-card: #FFFFFF        — фон карток
--bg-blush: #F7EBEF      — рожевий фон
--rose-light: #F4D8E1     — світло-рожевий
--rose-soft: #E8B4C3      — м'який рожевий
--rose-medium: #D9899E    — середній рожевий
--rose-deep: #C25975      — глибокий рожевий (основний)
--rose-dark: #8F2D46      — темно-рожевий
--text-main: #2E1F23      — основний текст
--text-body: #59474B      — текст абзаців
--text-muted: #8C787C     — приглушений текст
--font-heading: 'Cormorant Garamond', serif
```

---

## Мобільна версія (≤768px)

- Header: 65px, бургер-меню, CTA приховано
- Hero: 1 колонка, фото 85%×300px
- About: 1 колонка, фото 280px
- Services: картки 100% ширини
- Gallery: 2 колонки, квадратні фото
- Reviews: 1 колонка
- Contact: 1 колонка
- Section padding: 70px (замість 110px)
- Lightbox: зменшені стрілки, max-height 60vh
- Inputs: font-size 16px (запобігає iOS autozoom)

---

## Потік даних

```
localStorage("karina_admin_config")
  └─ loadSiteConfig()
      └─ siteConfig = merge(DEFAULT_CONFIG, parsed)
          └─ applyConfigToDOM()
              ├─ Телефони / Viber / Telegram лінки
              ├─ Тексти (hero, about, email)
              ├─ Фото (heroMainPhoto, aboutPhoto)
              ├─ CSS (font-family)
              ├─ renderServicesCatalog()
              ├─ renderPortfolioGallery()
              ├─ renderGalleryAndAlbums()
              └─ renderReviews()
```
