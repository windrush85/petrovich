# Петрович: deep design / UX blueprint

Исследовательский пакет по публичному сайту `petrovich.ru` для создания **оригинального строительного e-commerce skeleton**.  
Цель: понять архитектуру страниц, дизайн-систему, карточки товара, каталог, фильтры, сервисы и собрать свой интерфейс в похожей категории качества, **без копирования бренда, логотипов, текстов и trademark assets**.

## Что внутри

| Папка/файл | Назначение |
|---|---|
| `IMPLEMENTATION_BLUEPRINT.md` | Главный файл: практический blueprint для реализации сайта |
| `DEEP_AUDIT.md` | Краткий итоговый аудит Petrovich |
| `STRUCTURE.md` | Дерево файлов репозитория |
| `docs/index.html` | HTML-обзор структуры, можно открыть через GitHub Pages или локально |
| `dembrandt/latest/DESIGN.md` | Извлечённые design tokens в формате DESIGN.md |
| `dembrandt/latest/tokens.json` | Машиночитаемые design tokens |
| `dembrandt/latest/raw.json` | Raw extraction output Dembrandt |
| `dembrandt/run-full/*` | Desktop/mobile extraction run: DESIGN, raw, tokens, lint |
| `nous39/agent_*.txt` | 39 параллельных Nous-анализа по UX/IA/компонентам |
| `raw/crawl_summary.json` | Sitemap/crawl counts и выбранные URL |
| `raw/selected_urls.txt` | 52 репрезентативные страницы |
| `raw/page_briefs.jsonl` | Попытка HTTP briefs, показала 401 для raw parser |

## Покрытие

| Метрика | Значение |
|---|---:|
| Sitemap URLs | `69,144` |
| Catalog URLs | `29,875` |
| Product URLs | `39,194` |
| Static/service URLs | `75` |
| Representative URLs | `52` |
| Dembrandt crawl | `30` pages, desktop + mobile |
| 39nous agents | `39/39` |
| DESIGN.md lint | `0 errors`, `0 warnings` |

## Как использовать

### 1. Быстро понять архитектуру

Открой:

```text
IMPLEMENTATION_BLUEPRINT.md
```

Там описаны:

- архитектура сайта;
- главная;
- каталог/PLP;
- карточка товара/ProductCard;
- PDP/product detail page;
- поиск/фильтры;
- корзина/checkout/delivery;
- сервисы и калькуляторы;
- component library blueprint.

### 2. Использовать как источник для агента-разработчика

Дай агенту файлы:

```text
IMPLEMENTATION_BLUEPRINT.md
DEEP_AUDIT.md
dembrandt/latest/DESIGN.md
dembrandt/latest/tokens.json
raw/selected_urls.txt
```

И промпт:

```text
Построй оригинальный строительный e-commerce prototype. Используй Petrovich blueprint только как референс архитектуры и UX-паттернов. Не копируй бренд, логотипы, тексты, изображения и trademark assets. Начни с Home + Catalog + Product Detail + Cart Drawer.
```

### 3. Использовать tokens в коде

`dembrandt/latest/tokens.json` можно конвертировать в CSS variables, Tailwind theme или design tokens.

Базовые значения:

| Token | Value |
|---|---|
| Primary | `#1F2937` |
| Accent | `#E6B800` |
| Neutral | `#6B7280` |
| Text/dark | `#101010` |
| Font | Inter + Arial |
| Base spacing | `8px` |
| Radius | `16px`, `22px`, `24px`, `9999px` |
| Breakpoint | `640px` |

## Рекомендуемый порядок реализации

1. `AppShell`, `Header`, `MegaCatalogMenu`, `SearchBox`.
2. `CategoryTile`, `PromoBanner`, `ProductGrid`, `ProductCard`.
3. `CatalogPage` + `FilterSidebar` + `FilterBottomSheet`.
4. `ProductDetailPage` + `PDPGallery` + `BuyBox` + `SpecsTable`.
5. `CartDrawer`, `CartPage`, `CheckoutStepper`.
6. `DeliveryPickupInfo`, `AddressSelector`, `StorePickupSelector`.
7. `CalculatorWidget` и сервисные страницы.
8. Mobile QA и accessibility pass.

## Ограничения

- Raw HTTP parser получил `401`, но browser/Dembrandt extraction прошёл.
- Search и checkout частично динамические, приватные/account зоны не вскрывались.
- Это исследовательский reference pack, не готовый клон сайта.
- Не использовать логотипы, бренд, тексты, изображения Petrovich в production.

## Локальный просмотр

Можно открыть HTML-обзор:

```bash
python3 -m http.server 8080
# открыть http://localhost:8080/docs/
```

Или просто смотреть markdown-файлы в GitHub UI.
