# Karina Flowers — Дизайн-система (DESIGN)

> Еталонний документ стилю. Будь-які зміни повинні зберігати ці правила, щоб дизайн не зламався.

---

## 🎨 Колірна палітра (CSS variables, рядок 27-55)

| Змінна | Значення | Використання |
|--------|----------|--------------|
| `--bg-ivory` | `#FAF6F3` | Основний фон сторінки |
| `--bg-warm` | `#FFFBF9` | Теплий фон (FAQ, Portfolio) |
| `--bg-card` | `#FFFFFF` | Фон карток |
| `--bg-blush` | `#F7EBEF` | Рожевий фон (теги, іконки) |
| `--rose-light` | `#F4D8E1` | Рамки, бордери |
| `--rose-soft` | `#E8B4C3` | Світлі акценти |
| `--rose-medium` | `#D9899E` | Hover-бордери |
| `--rose-deep` | `#C25975` | **ОСНОВНИЙ** рожевий (кнопки, акценти) |
| `--rose-dark` | `#8F2D46` | Темний рожевий (заголовки, hover) |
| `--text-main` | `#2E1F23` | Заголовки |
| `--text-body` | `#59474B` | Абзаци |
| `--text-muted` | `#8C787C` | Підписи, другорядний текст |
| `--text-light` | `#B3A0A4` | Футер, декоративний |

### Градієнти
- Кнопки: `linear-gradient(135deg, #C25975 0%, #8F2D46 100%)`
- Hover: тінь `rgba(194, 89, 117, 0.16)`
- Теги активних табів: `linear-gradient(135deg, #C25975, #8F2D46)`

---

## 🔤 Типографіка

| Шрифт | Використання |
|-------|--------------|
| `--font-heading: 'Cormorant Garamond', serif` | Заголовки h1-h4, лого, ціни, FAQ питання |
| `'Plus Jakarta Sans', sans-serif` | Основний текст, кнопки, інпути |

- Підключено також: `Montserrat`, `Playfair Display` (Google Fonts)
- Заголовки: `line-height: 1.18`
- Абзаци: `line-height: 1.7`

### Розміри
| Елемент | Розмір |
|---------|--------|
| H1 (hero) | `clamp(2.5rem, 5vw, 4rem)` |
| H2 (секцій) | `clamp(2.2rem, 4vw, 3.2rem)` |
| H3 (послуги) | `2rem` |
| Тіло | `16px` (1rem) |
| Підписи секцій (tag) | `0.82rem`, uppercase, `letter-spacing: 2px` |

---

## 🔘 Кнопки (рядки 135-178)

| Клас | Вигляд | Hover |
|------|--------|-------|
| `.btn` | inline-flex, padding 18px 40px, radius 50px, min-height 52px, shadow | — |
| `.btn-primary` | Рожевий градієнт, білий текст | `translateY(-3px)` + тінь |
| `.btn-outline` | Білий, рамка rose-light | bg-blush + `translateY(-3px)` |
| `.btn-sm` | padding 10px 22px, 0.85rem, min-height 40px | — |

---

## 📐 Відступи та радіуси

| Змінна | Значення |
|--------|----------|
| `--radius-arch` | `240px 240px 24px 24px` (hero фото) |
| `--radius-lg` | `28px` (картки послуг) |
| `--radius-md` | `18px` (картки галереї, адмінки) |
| `--radius-sm` | `12px` (FAQ, інпути) |
| `.section` padding | `110px 0` (десктоп) / `70px 0` (мобільний) |
| `.container` | max-width `1240px`, padding `0 28px` / `0 16px` (mobile) |
| `.section-header` | max-width 680px, margin-bottom 60px |

---

## 🧱 Блоки та їх стилі (актуальні рядки CSS)

| Блок | Рядок CSS | Ключові стилі |
|------|-----------|---------------|
| Hero | 216-330 | Градієнтний фон, 2 колонки, арка 450×560px, біла рамка 8px |
| About | 334-344 | 2 колонки 0.95fr/1.05fr, фото 500px |
| Services | 346-391 | Картки flex 320-370px, тег `inline-block` (НЕ absolute!), ціна 2.2rem |
| Gallery | 426-460 | auto-fill minmax(280px), 4:5 aspect, hover zoom 1.08 |
| Reviews | 408-424 | auto-fit minmax(300px), зірочки #F1C40F |
| FAQ | 549-594 | Акордеон, `max-height` анімація, іконка ＋→× rotate 45deg, **клас `.open`** |
| Lightbox | 462-492 | z-index 2000, img max-height 75vh, стрілки 52px |
| Contact | 486-493 | 2 колонки, картка форми padding 44px |
| Speed Dial | 599-626 | fixed bottom-right, z-index 1050, пульсація greenPulse |
| Reveal (scroll) | 494-503 | opacity 0→1, translateY 40px→0, `.active` додає IntersectionObserver |

---

## 📱 Мобільна версія (ЄДИНИЙ @media max-width: 768px, рядок 206)

| Блок | Мобільне правило |
|------|------------------|
| Header | 65px, nav hidden, burger flex, CTA hidden |
| `.section` | padding 70px |
| `.container` | padding 0 16px |
| Hero | 1 колонка, арка 85%/260px/300px, title 1.9rem |
| About | 1 колонка, фото 280px |
| Services | картки 100% ширини, padding 24px |
| Gallery | 2 колонки, 1:1 aspect |
| Reviews | 1 колонка |
| FAQ | title 1.1rem, padding 18px 12px |
| Lightbox | стрілки 40px, img max-height 60vh |
| Forms | font-size 16px (iOS fix), `.btn` min-height 48px |

**ВАЖЛИВО:** Єдиний мобільний медіа-запит. Не додавати другі `@media 768` — були видалені дублікати.

---

## 🪜 Z-index ієрархія (конфліктів немає)

```
Header (1000) < Speed Dial (1050) < Lightbox (2000) < Mobile Nav (9998) < Modals (9999)
```

---

## ⚠️ Критичні правила (щоб не зламати дизайн)

1. **FAQ:** використовує клас `.open`, НЕ `.active` (конфлікт з `.reveal.active`)
2. **Сервіс-тег:** `display: inline-block; margin-bottom: 12px` — НЕ `position: absolute`
3. **Контактна форма:** поля `serviceSelect`/`clientMessage` — рівно по 1 екземпляру
4. **Мобільний CSS:** один `@media (max-width: 768px)` блок, всі правила з `!important`
5. **Фотографії:** `#heroMainPhoto` (hero), `#aboutPhoto` (about) — ID обов'язкові для оновлення з адмінки
6. **Змінні кольорів:** всі кольори через CSS variables, не хардкодити нові
7. **Модальні вікна:** `display:none` за замовчуванням, показ через `style.display='flex'`
8. **Не видаляти:** `@keyframes greenPulse` (пульсація кнопки дзвінка)
