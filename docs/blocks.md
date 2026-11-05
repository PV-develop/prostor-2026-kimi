# Библиотека блоков и компонентов

Структура:

- `src/components/ui/` — мелкие переиспользуемые элементы
- `src/components/blocks/` — секции страницы
- `src/components/layouts/` — шаблоны страниц
- Контент главной — `src/content/home.ts` (типизированные данные)

## UI-компоненты

| Компонент | Параметры | Назначение |
| --- | --- | --- |
| `Container.astro` | `as?` | обёртка с полями страницы |
| `SectionLabel.astro` | `text`, `variant: h\|v`, `tone: light\|dark`, `class?` | подпись секции с красным квадратом; `v` — вертикальная (раскрывается скролл-нитью), `h` — горизонтальная |
| `SectionHeading.astro` | `html`, `as?`, `size: xl\|lg\|md\|sm` | дисплейный заголовок Steelfish (HTML-строки поддерживают `<br>`, stroke-спаны) |
| `Button.astro` | `href`, `label`, `variant: fill-accent\|outline-light\|outline-dark\|fill-light-to-accent`, `arrow?`, `class?` | кнопка/ссылка с hover-свайпом |
| `TextField.astro` | `name`, `label`, `type?`, `placeholderOnFocus?`, `autocomplete?`, `tone: ink\|dark`, `class?` | поле с плавающей подписью (состояние `.is-filled` вешает `main.ts`) |
| `TextArea.astro` | `name`, `label`, `rows?`, `class?` | многострочное поле |
| `FileField.astro` | `name?`, `label?`, `hint?`, `class?` | поле вложения с подписью выбранного файла |
| `CaseCard.astro` | `photo`, `photoAlt`, `tag`, `industry`, `value`, `suffix`, `metric`, `title`, `description`, `href?` | карточка кейса: счётчик `data-count`, hover-съём серой вуали |
| `TeamCard.astro` | `photo`, `name`, `role`, `since?`, `num` | карточка члена команды |

## Блоки

| Блок | Данные | Примечания |
| --- | --- | --- |
| `SiteHeader` | `contacts` | фиксированная шапка: телефон, CTA, кнопка меню; состояние скролла и морф логотипа — `main.ts` (`data-hdr`, `data-hconn`, `data-hsp`) |
| `MobileMenu` | `contacts`, `menuNav`, `menuNavExtra`, `menuServiceGroups` | выпадающая панель; пункты без страниц скрыты `hidden` + TODO (разметка сохранена) |
| `HeroMain` | `hero` | герой: шум, вертикальная подпись, заголовок, лид, подписи «20», CTA; включает `TrustMarquee`; частицы/параллакс — `main.ts` |
| `TrustMarquee` | `trustMarquee` | бегущая строка «Нам доверяют» |
| `ServiceList` | `services`, `industries` | заголовок, 6 строк направлений (06 — featured), отраслевые чипы с фото-подложкой по hover |
| `CasesSection` | `cases` | сетка `CaseCard` |
| `ProcessSection` | `process` | 5 карточек этапов; на ≤1024 — горизонтальный скролл-снап; стрелки анимирует `main.ts` |
| `CtaForm` | `ctaForm` | тёмная панель с формой (mock-отправка) |
| `TeamSection` | `team` | сетка `TeamCard` |
| `ReviewsSection` | `reviews` | marquee логотипов-заглушек + цитата; живёт внутри тёмной обёртки вместе с командой |
| `ContactSection` | `contacts` | форма + бриф-карточка + контакты; внутри тёмной обёртки вместе с футером |
| `ContactForm` | — | сама форма заявки (используется в `ContactSection`) |
| `SiteFooter` | `contacts`, `footer` | футер с колонками ссылок; незаполненные `href: null` рендерятся как `<span>` с TODO; кнопка «Наверх», анимированная марка логотипа |

## Шаблоны

- `BaseLayout.astro` — `title`, `description`, `pageUrl?`; lang=ru, OG/Twitter,
  preload критических шрифтов, favicon, подключение `src/scripts/main.ts`, noscript-фолбэки.

## Пример сборки страницы

```astro
<BaseLayout title="…" description="…">
  <SiteHeader />
  <MobileMenu />
  <main>
    <HeroMain />
    <ServiceList />
    …
  </main>
</BaseLayout>
```

Тёмные секции (команда+отзывы, контакты+футер) объединяются обёрткой
`<div class="dark-section">` (определена в `src/pages/index.astro`).

## Правила пополнения

1. Новая страница собирается из существующих блоков; новый блок — только если
   существующие не подходят, и сразу сюда в документацию.
2. Контент — в `src/content/`, не в разметке.
3. Пункты навигации на ещё не созданные страницы скрываем (`hidden`/не рендерим),
   разметку сохраняем с комментарием TODO и возвращаем при появлении страницы.
4. Каждый блок проверяем на 1440px и 375px: нет горизонтального скролла,
   нет ошибок консоли, ресурсы локальные.
