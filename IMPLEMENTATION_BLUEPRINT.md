# Petrovich.ru deep design / UX blueprint

Дата прогона: 2026-07-21

## 1. Покрытие

| Слой | Факт |
|---|---:|
| Sitemap всего | 69,144 URL |
| Catalog URLs | 29,875 |
| Product URLs | 39,194 |
| Static/service URLs | 75 |
| Репрезентативная выборка | 52 URL |
| Dembrandt crawl | 30 страниц, desktop + mobile |
| 39nous swarm | 39/39 агентов, 111KB заметок |
| DESIGN.md lint | 0 errors, 0 warnings |

Артефакты:

```text
/opt/data/design-research/petrovich-deep/crawl_summary.json
/opt/data/design-research/petrovich-deep/selected_urls.txt
/opt/data/design-research/petrovich-deep/dembrandt/latest/DESIGN.md
/opt/data/design-research/petrovich-deep/dembrandt/latest/tokens.json
/opt/data/design-research/petrovich-deep/dembrandt/latest/raw.json
/opt/data/design-research/petrovich-deep/nous39/agent_*.txt
```

## 2. Архитектура сайта

| Тип | URL-паттерн | Роль |
|---|---|---|
| Home | `/` | вход, быстрый поиск, крупные категории, промо |
| Catalog root | `/catalog/` | верхний каталог строительных категорий |
| Category listing | `/catalog/<id>/` | PLP: фильтры, сортировка, сетка товаров |
| Product detail | `/product/<id>/` | PDP: галерея, цена, наличие, доставка, характеристики |
| Search | `/search/?q=...` | динамическая выдача, в sitemap не попадает |
| Promo | `/promos/` | акции и кампании |
| Services/tools | `/calculator/`, `/services/3d-planner/`, `/fitting-room/`, `/kalkulyator-remonta/...` | калькуляторы, планировщики, сервисная конверсия |
| Buyers/support | `/buyers/return/`, `/contacts/complaint/`, `/about/review-rules/` | доверие, правила, возврат, жалобы |
| Loyalty/B2B | `/loyalties/ub/about/`, `/kdp/` | удержание, программа лояльности, профклиенты |

Главный вывод: это не просто интернет-магазин. Это строительная e-commerce платформа с тремя опорами: **каталог → товарка → сервисные инструменты**.

## 3. Главная страница, skeleton

Обязательные блоки для оригинального аналога:

1. **Sticky/header shell**
   - логотип/название проекта;
   - выбор города/доставки;
   - крупный search bar;
   - каталог-кнопка;
   - профиль/заказы/избранное/корзина.
2. **Hero/search-first зона**
   - главный акцент не на “красоту”, а на быстрый путь к материалам;
   - промо/сезонные баннеры;
   - быстрые строительные категории.
3. **Category shortcuts**
   - плитки категорий: сухие смеси, пиломатериалы, инструмент, электрика, сантехника и т.д.;
   - иконка + короткий заголовок + счётчик/подкатегория.
4. **Promo rails**
   - акции, распродажи, комплектные предложения.
5. **Trust/service strip**
   - доставка сегодня/завтра;
   - самовывоз;
   - возврат;
   - профессиональная поддержка;
   - расчёт материалов.
6. **Footer**
   - покупателям;
   - бизнесу;
   - сервисы;
   - контакты;
   - документы/правила.

## 4. Каталог / категория, PLP skeleton

Ключевой экран для 29,875 catalog URL.

| Зона | Что должно быть |
|---|---|
| Breadcrumbs | путь: Главная → Каталог → Раздел → Подраздел |
| Category title | H1 + количество товаров |
| Subcategory chips | быстрые вложенные категории |
| Filter sidebar | цена, бренд, размеры, материал, наличие, рейтинг, доставка |
| Sort bar | популярность, цена, новизна, наличие |
| View toggle | сетка/список, плотность карточек |
| Product grid | 2-5 колонок desktop, 1-2 mobile |
| Pagination/infinite | осторожно: для SEO лучше пагинация/URL |
| Selected filters | chips с быстрым сбросом |
| Empty state | “нет товаров”, похожие категории, сброс фильтров |

Мобильный PLP:

- фильтры должны уходить в bottom sheet;
- сортировка отдельной компактной кнопкой;
- карточка товара плотная, без перегруза;
- sticky нижняя панель “Фильтры / Сортировка”.

## 5. Product card anatomy

Reusable `ProductCard` должен поддерживать строительные товары, где важны упаковки, штуки, м², кг, доставка.

| Элемент | Назначение |
|---|---|
| Image | товар, бейджи, placeholder |
| Badges | акция, хит, доставка сегодня, выгодно упаковкой |
| Title | 2-3 строки, SEO-friendly |
| SKU/brand | мелкий вторичный слой |
| Rating/reviews | доверие |
| Price | крупно, главный якорь |
| Unit price | ₽/шт, ₽/м², ₽/кг, ₽/упак |
| Availability | “в наличии”, “под заказ”, склад/город |
| Delivery/pickup | дата, способ, стоимость/условия |
| Qty stepper | +/-, кратность упаковки |
| CTA | “В корзину”, state added |
| Secondary | сравнить, избранное |

States:

- default;
- hover;
- added to cart;
- out of stock;
- discounted;
- low stock;
- package-only quantity;
- mobile compact.

## 6. Product detail page, PDP skeleton

PDP для 39,194 product URL должен быть модульным.

1. **Top block**
   - breadcrumbs;
   - H1;
   - rating/reviews/SKU;
   - галерея + media thumbnails;
   - price panel.
2. **Buy box**
   - цена;
   - unit price;
   - quantity stepper;
   - add to cart;
   - delivery/pickup selector;
   - availability by store/warehouse;
   - loyalty hints.
3. **Specs**
   - короткий список ключевых характеристик;
   - полная таблица;
   - документы/сертификаты, если есть.
4. **Construction-specific calculators**
   - “сколько нужно на площадь/объём”;
   - “подобрать комплектующие”;
   - “расчёт доставки”.
5. **Trust**
   - возврат;
   - гарантия;
   - консультация;
   - отзывы.
6. **Recommendations**
   - аналоги;
   - вместе покупают;
   - комплектующие;
   - просмотренные.

## 7. Search / filters

Search должен быть отдельным high-value компонентом:

- autocomplete;
- подсказки категорий;
- история запросов;
- typo tolerance;
- synonyms для стройки: “гипса”, “гкл”, “лист гипсокартона”;
- zero-result recovery;
- быстрые chips “в наличии”, “с доставкой”, “дешевле”.

Фильтры должны быть schema-driven:

```ts
type FilterSchema = {
  key: string
  label: string
  type: 'range' | 'checkbox' | 'radio' | 'color' | 'dimension'
  unit?: string
  values?: Array<{label: string; count: number; value: string}>
}
```

## 8. Cart / checkout / delivery

Даже если extraction не открывал приватный checkout, e-commerce skeleton должен предусмотреть:

| Flow | Компоненты |
|---|---|
| Cart | cart rows, qty stepper, package multiples, unavailable items |
| Delivery | address, interval, price, heavy/bulky rules |
| Pickup | store selector, stock by store |
| Checkout | contacts, legal entity/individual, payment, confirmation |
| Order status | tracking, repeat order, documents |
| Returns | reason, photos, rules |

Строительная специфика: доставка тяжелых/габаритных товаров, разные единицы измерения, кратность упаковки, самовывоз по складам.

## 9. Services / calculators

Важный слой Petrovich: не только продажа, но помощь в проекте.

Реализуемые skeleton-модули:

- `MaterialCalculator`;
- `RepairCostEstimator`;
- `RoomPlanner3DEntry`;
- `FittingRoomEntry`;
- `DeliveryCostCalculator`;
- `ProjectChecklist`;
- `SavedEstimate`.

Каждый calculator должен иметь:

1. входные параметры;
2. live calculation;
3. список рекомендуемых товаров;
4. “добавить всё в корзину”;
5. сохранить расчёт.

## 10. Design system tokens

Из Dembrandt:

| Token | Значение |
|---|---|
| Primary | `#1F2937` |
| Secondary/accent | `#E6B800` |
| Surface/neutral | `#6B7280` |
| On-surface | `#101010` |
| Typography | Inter primary, Arial secondary |
| H1 | Inter 36px / 800 / 1.08 |
| Body | Inter 16px / 400 / 1.55 |
| Small/body | Inter 15px / 400-700 / 1.6 |
| Base spacing | 8px |
| Radius | 16px, 22px, 24px, 9999px |
| Breakpoint observed | 640px |
| Shadows | soft black 0.08, yellow glow 0.4 |

WCAG note:

- `#1F2937` on white: AAA;
- `#6B7280` on white: AA;
- `#E6B800` on white: fail for small text, use for fills/borders/icons or dark text on yellow, not small yellow text on white.

## 11. Component library blueprint

```text
AppShell
Header
MegaCatalogMenu
SearchBox
AutocompleteDropdown
Breadcrumbs
CategoryTile
PromoBanner
ProductGrid
ProductCard
ProductPrice
AvailabilityBadge
DeliveryPickupInfo
QtyStepper
FilterSidebar
FilterBottomSheet
SortControl
Pagination
PDPGallery
BuyBox
SpecsTable
ReviewSummary
RecommendationRail
CartDrawer
CartPage
CheckoutStepper
AddressSelector
StorePickupSelector
CalculatorWidget
ServiceCard
LoyaltyBanner
Footer
```

## 12. Implementation priorities

1. Build static component library from tokens.
2. Implement home + catalog + product page first.
3. Add schema-driven filters and product cards.
4. Add cart drawer/page.
5. Add delivery/pickup selector.
6. Add calculators as conversion layer.
7. Add mobile bottom-sheet filters.
8. Run visual QA desktop/mobile.

## 13. Limits of this run

- Raw HTTP got `401`; browser/Dembrandt path worked.
- Search pages are dynamic and absent from sitemap.
- Private account/checkout internals were not accessed.
- Extraction is for visual/UX reference. Do not reuse logos, trademarked copy, exact brand assets.
