# Karina Flowers — Структура сайту (index.html)

> **Файл:** `index.html` | **Рядків:** ~1450 | **Кодування:** UTF-8
> **Актуалізовано:** 2026-08-17 (читання даних з GitHub, динамічний FAQ)

---

## Карта файлів сайту

| Файл | Розмір | Призначення |
|------|--------|-------------|
| `index.html` | ~3 517 KB | Головна сторінка (з base64 фото в JS) |
| `admin.html` | ~3 075 KB | Панель управління (публікує зміни на GitHub) |
| `favicon.jpg` | 105 KB | Іконка сайту |
| `images/` | 22 файли | Фотографії галереї (зовнішні) |
| `site-data.json` | ~519 KB | **Актуальні дані сайту** (публікує адмінка, читає сайт) |
| `docs/SITE_STRUCTURE.md` | 9 KB | Ця документація |
| `docs/DESIGN.md` | — | Дизайн-система (кольори, стилі, блоки) |
| `docs/ADMIN_STRUCTURE.md` | 7 KB | Документація адмінки |
| `publish.ps1` | 2 KB | Скрипт публікації коду на GitHub |
| `.env` | — | Секрети (GitHub токен) — НІКОЛИ не публікується |

### Джерело даних (порядок пріоритету)
```
1. site-data.json (GitHub Pages) — актуальні дані, які публікує адмінка
2. localStorage "karina_admin_config" — для локального перегляду
3. DEFAULT_CONFIG — вбудовані дефолти
```

### images/ — фото галереї (22 шт.)
- `photo_1.jpg` (155 KB) … `photo_11.jpg` (116 KB)
- `photo_2026-08-09_11-35-14.jpg` … `photo_2026-08-09_11-35-50.jpg` (11 шт.)
- Адмінка завантажує нові фото сюди через GitHub API, галерея посилається на них через `images/photo_N.jpg`

---

## HTML структура (зверху вниз)

| # | Елемент | Рядки | ID | Призначення |
|---|---------|-------|-----|-------------|
| 1 | Speed Dial | 599-626 | `#speedDialMenu`, `#callFloatingBtn`, `#displayViberLink`, `#displayTelegramLink`, `#displayCallLink` | Плаваюча кнопка + меню Viber/Telegram/Дзвінок, z-index 1050 |
| 2 | Header | 628-645 | — | Лого, навігація, кнопка "Замовити дзвінок", z-index 1000 |
| 3 | Hero | 647-661 | `#hero`, `#displayHeroTitle`, `#displayHeroSubtitle`, `#heroMainPhoto` | Заголовок + фото в арці |
| 4 | About | 663-677 | `#about`, `#aboutPhoto`, `#displayAboutTitle`, `#displayAboutLead` | Фото + текст про Карінку |
| 5 | Services | 679-692 | `#services`, `#dynamicServicesGrid` | 4 картки послуг (динамічний рендер) |
| 6 | Portfolio | 694-707 | `#portfolio`, `#dynamicAlbumFilterBtns`, `#galleryGrid` | Галерея з фільтрами альбомів |
| 7 | Mobile Nav | 709-721 | `#mobileNavDrawer` | Повноекранне меню, z-index 9998 |
| 8 | Success Modal | 723-734 | `#successModal`, `#successModalTitle`, `#successModalText` | Підтвердження заявки, z-index 9999 |
| 9 | Order Modal | 736-748 | `#orderModal`, `#modalTitle`, `#modalName`, `#modalPhone`, `#modalDetails` | Форма замовлення, z-index 9999 |
| 10 | Lightbox | 750-763 | `#lightboxModal`, `#lightboxImg`, `#lightboxCaption`, `#lightboxCounter` | Перегляд фото, z-index 2000 |
| 11 | Reviews | 765-775 | `#reviews`, `#dynamicReviewsGrid` | Відгуки клієнтів |
| 12 | FAQ | 777-828 | `#faq` | 4 питання-акордеон |
| 13 | Contact | 830-849 | `#contact`, `#displayPhoneLink`, `#displayEmailText`, `#mainContactForm`, `#clientName`, `#clientPhone`, `#serviceSelect`, `#clientMessage` | Контакти + форма заявки |
| 14 | Footer | 851-856 | — | © 2026 + лінк на адмінку |

---

## Контактна форма (mainContactForm) — ОДИН екземпляр полів

```
#clientName    — text, required, placeholder "Ваше ім'я"
#clientPhone   — tel, required, placeholder "Ваш телефон"
#serviceSelect — select (4 опції: Майстер-клас, B2B Навчання, Весільна флористика, WOW-доставка)
#clientMessage — textarea (2 rows), "Опишіть ваше замовлення"
button submit  — "Надіслати заявку 🌸"
onsubmit       — handleFormSubmit(event)
```

**ВАЖЛИВО:** поля `serviceSelect`/`clientMessage` НЕ дублюються (фікс 2026). Дублікат ID ламає форму.

---

## JavaScript функції (актуальні рядки)

### Ініціалізація та рендеринг
| Функція | Рядок | Призначення |
|---------|-------|-------------|
| `loadSiteConfig()` | ~887 | Асинхронно: fetch site-data.json з GitHub → localStorage → дефолти |
| `applyConfigToDOM()` | ~900 | Оновлення всіх DOM-елементів |
| `renderFaq()` | ~1285 | Рендерить FAQ з `siteConfig.faq` у `#faqContainer` (динамічний) |
| `renderPortfolioGallery()` | 1099 | Портфоліо-сітка (застаріла, не використовується в DOM) |
| `filterPortfolio()` | 1142 | Фільтр портфоліо |
| `expandMobilePortfolio()` | 1150 | Розгортання на мобільному |
| `renderServicesCatalog()` | 1157 | Картки послуг |
| `renderGalleryAndAlbums()` | 1287 | Галерея + фільтри |
| `renderReviews()` | 1267 | Відгуки |

### Форми та модали
| Функція | Рядок | Призначення |
|---------|-------|-------------|
| `sendTelegramMessage()` | 971 | Відправка в Telegram |
| `scrollToContactForm()` | 996 | Скрол до форми |
| `showSuccessModal()` | 1011 | Показати success modal |
| `handleFormSubmit()` | 1027 | Обробка контактної форми |
| `openModal()` | 1214 | Відкрити order modal |
| `closeModal()` | 1223 | Закрити order modal |
| `handleModalSubmit()` | 1233 | Обробка модальної форми |
| `sendEmailNotification()` | 1258 | mailto: fallback |

### Галерея / Lightbox
| Функція | Рядок | Призначення |
|---------|-------|-------------|
| `openServicePhotoLightbox()` | 1277 | Лайтбокс з картки послуги |
| `openLightboxByIndex()` | 1329 | Відкрити за індексом |
| `prevLightboxImage()` | 1339 | Попереднє |
| `nextLightboxImage()` | 1344 | Наступне |
| `closeLightbox()` | 1349 | Закрити |

### Навігація та інше
| Функція | Рядок | Призначення |
|---------|-------|-------------|
| `toggleMobileMenu()` | 1056 | Мобільне меню |
| `navigateToSection()` | 1069 | Скрол до секції |
| `toggleSpeedDial()` | 1077 | Speed dial |
| `toggleFAQ()` | 1387 | Акордеон FAQ — **використовує клас `open`, НЕ `active`** |

### Події
| Рядок | Подія | Дія |
|-------|-------|-----|
| 1355 | DOMContentLoaded | loadSiteConfig + header scroll |
| 1371 | IntersectionObserver | Анімація `.reveal` → додає `.active` |

**ВАЖЛИВО:** FAQ акордеон використовує клас `.open` (не `.active`), бо `.active` конфліктує зі scroll-анімацією `.reveal.active`. Не повертати `.active`!

---

## Потік даних

```
localStorage("karina_admin_config")
  └─ loadSiteConfig() (887)
      └─ siteConfig = merge(DEFAULT_CONFIG (863), parsed)
          └─ applyConfigToDOM() (900)
              ├─ Лінки (phone, viber, telegram, call)
              ├─ Тексти (hero, about, email)
              ├─ Фото (heroMainPhoto, aboutPhoto)
              ├─ CSS (--font-heading)
              ├─ renderServicesCatalog() (1157)
              ├─ renderPortfolioGallery() (1099)
              ├─ renderGalleryAndAlbums() (1287)
              └─ renderReviews() (1267)
```
