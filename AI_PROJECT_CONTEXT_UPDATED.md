# IGU Builder — AI Project Context

## Как использовать этот файл
При новом чате загрузите:
1. актуальный `index.html`
2. этот `AI_PROJECT_CONTEXT.md`

Стартовый запрос:
> Прочитай сначала `AI_PROJECT_CONTEXT.md`, затем изучи актуальный `index.html`. Продолжаем разработку с текущего состояния. Не переписывай проект с нуля и не возвращай базу данных.

---

## 1. Архитектура проекта
Репозиторий: `MaksymPrudnikov/igu-builder`

Текущая версия — автономный статический IGU Builder.

Рекомендуемая структура:
```text
igu-builder/
├── index.html
└── AI_PROJECT_CONTEXT.md
```

Главное правило: приложение должно работать как статический GitHub Pages сайт.

Не возвращать:
- Supabase
- обязательный `db.json`
- внешнюю БД
- runtime-загрузку каталога из сети
- старую зависимость от отдельного `admin.html`

Все рабочие каталоги и настройки находятся внутри `index.html`.

---

## 2. Catalog / Admin
В `index.html` встроен режим Catalog / Admin.

Он используется для редактирования каталогов и локального сохранения/экспорта.

При изменении каталогов:
- сохранять текущую структуру данных;
- не удалять ID;
- не заменять полный каталог тестовыми данными;
- не ломать совместимость с существующими записями.

---

## 3. Основной функционал, который нельзя случайно удалить
Проект включает:
- IGU make-up configurator;
- make-up A / B / C и далее;
- single / double / triple IGU;
- Order Lines;
- расчёт SQ-FT;
- 2D / 3D preview;
- Smart Shape;
- Fabrication CAD;
- drill holes;
- hardware / cutouts;
- DXF export;
- production drawings;
- muntin configuration;
- печатную production form;
- Catalog / Admin.

При обычных правках печати или таблицы не переписывать Smart Shape/Fabrication блоки.

---

## 4. Header заказа
Поле `Project` удалено и возвращать его не нужно.

Текущие поля:
- Company
- PO #
- Order #

В печати они должны быть заметны, но компактны.

Не возвращать очень крупный размер текста.

---

## 5. Due Date
`Date` заменено на:

**Due Date**

Поле должно использовать календарный picker:
```html
<input type="date">
```

На печати:
**DUE DATE**

Пользователь должен иметь возможность менять дату вручную через календарь.

---

## 6. Order Lines — начальное количество
При запуске должна быть только:

**1 строка Order Line**

Дополнительные строки пользователь добавляет сам через:
- `+ line`
- `+ 10 lines`
- Paste from Excel

Не возвращать автоматические 6/10/12 пустых строк.

---

## 7. Order Lines — порядок колонок
Текущий production-порядок:

```text
LINE → QTY → WIDTH → HEIGHT → MARK → SQ-FT → M-U
```

Этот порядок должен быть одинаковым:
- на экране;
- в печати;
- в Copy table;
- в логике keyboard navigation/paste, если это затрагивается.

Production/CAD controls могут оставаться отдельной непечатной колонкой справа на экране.

---

## 8. Визуальный приоритет Order Lines
Это производственная бумага.

Главный акцент:
1. QTY
2. WIDTH
3. HEIGHT

Второстепенные:
- MARK
- SQ-FT
- M-U

Поэтому:
- QTY — жирный;
- WIDTH / HEIGHT — крупные и жирные;
- MARK — компактный;
- SQ-FT — компактный и выровнен по центру;
- M-U — компактный бейдж.

MARK и SQ-FT не должны занимать много места.

---

## 9. Order Lines — линии таблицы
Раньше одновременно печатались:
- нижняя линия строки;
- underline самого input.

Это создавало двойную линию.

Текущее правило:
- одна горизонтальная линия между строками;
- без внутренних подчёркиваний input;
- чистый production look.

Таблица должна выглядеть как производственный лист, а не как HTML-форма.

---

## 10. Make-up A / B / C
Одна маленькая буква A/B недостаточна для ч/б печати.

Для B/C/D и других альтернативных make-up использовать визуально сильный monochrome highlight:
- контрастный M-U badge;
- рамку/линию;
- при необходимости `MAKE-UP B`;
- не полагаться только на серый фон.

На обычном ч/б принтере должно быть сразу понятно, какие строки относятся к B/C.

---

## 11. Формат печати
Основной формат:

**US Letter — Landscape**

```css
@page {
  size: Letter landscape;
}
```

Боковые поля уже уменьшены, чтобы всё надёжнее помещалось на Letter.

Не увеличивать margins без необходимости.

---

## 12. Чёрно-белый production print
Все производственные формы печатаются ч/б.

Печать должна быть максимально monochrome:
- белый фон;
- чёрный текст;
- чёрные/серые линии;
- отсутствие зависимости от цвета;
- высокая читаемость.

Использовать:
- толщину линий;
- контраст;
- рамки;
- бейджи;
- hatch/dash patterns.

Не кодировать важную информацию только цветом.

---

## 13. Не допускать наложения линий
Ранее underline Company / PO / Order почти совпадал с рамкой MAKE-UP.

Также возникали двойные границы между секциями.

Правило:
- между крупными секциями должен быть небольшой вертикальный зазор;
- не ставить две сильные горизонтальные линии почти в одной координате;
- по возможности использовать один ясный separator.

---

## 14. MAKE-UP block
MAKE-UP блок должен быть компактным по высоте.

Обычно содержит:
- L1
- C1
- L2
- IGU cross-section diagram

Он не должен забирать значительную часть Letter page.

---

## 15. Assembly Layers
Для L1/L2 показывать стекло и необходимые производственные детали компактно.

C1 должен быть в одну строку:

```text
Spacer · nominal size · Gas · Sealant
```

Пример:
```text
Black Warm Edge · 1/2" · Argon 90% · Polysulfide
```

Если газа нет:
```text
No gas
```

Если sealant не задан:
```text
No sealant
```

C1 должен иметь немного воздуха сверху/снизу, но не превращаться в 2–3 строки.

---

## 16. ACTUAL CAVITY
Не показывать пользователю:

**ACTUAL CAVITY**

Также не показывать вычисленное значение вроде:
```text
13.5 mm
```
рядом с nominal spacer:
```text
1/2"
```

Это намеренно скрыто, потому что вводило производство в заблуждение.

Внутренние расчёты могут продолжать использовать actual cavity.

---

## 17. IGU BUILD-UP
Заголовок:

**IGU BUILD-UP**

удалён из печатного блока, чтобы не занимать место.

Саму схему оставлять.

Не возвращать этот заголовок без явной просьбы.

---

## 18. Coated Glass / Low-E
Покрытие должно быть очевидно видно в ч/б печати.

Недостаточно тонкой цветной dashed-line.

Текущий intent:
- видимая штриховка/полоса на нужной поверхности;
- контрастный outline/dash;
- текстовая подпись.

Для Low-E:
```text
LoE
```

Для другого coated glass:
```text
COATED
```

Должно быть понятно, **на какой поверхности находится покрытие**.

Не полагаться на синий цвет.

---

## 19. IGU diagram
Диаграмма должна быть компактной.

Не увеличивать её без необходимости.

Если в блоке появляется много пустого пространства:
- лучше корректировать layout/grid;
- центрировать элементы;
- немного уменьшать diagram width;
- не увеличивать весь MAKE-UP block.

---

## 20. Company / PO / Order print size
Эти поля уже несколько раз уменьшались.

Текущий intent:
- заметные;
- жирные;
- компактные;
- не доминируют над производственными размерами.

Не возвращать их к очень большим 24–25px значениям.

---

## 21. Footer
Footer содержит:
- production warning;
- Notes;
- Prepared By;
- Due Date.

Он должен быть компактным.

Production warning:
> If any information on this sheet is unclear, contact the Production Manager before running the job.

Не удалять без отдельной просьбы.

---

## 22. Fit на одной странице
Печатный layout неоднократно оптимизировался, чтобы нормальный заказ помещался на одном Letter landscape листе.

При будущих изменениях:
- не увеличивать вертикальные padding без необходимости;
- не делать MAKE-UP слишком высоким;
- не делать header слишком большим;
- не заставлять всю Order Lines table перескакивать на страницу 2;
- избегать `page-break-inside: avoid` на большой таблице, если это ухудшает layout.

---

## 23. Screen vs Print
Если проблема касается только печати, предпочтительно исправлять её через:

```css
@media print
```

Не ломать экранный UI ради print-only задачи.

---

## 24. Embedded datasets
В проекте есть важные встроенные данные, включая:
- ITEMS
- SUB
- COAT
- LAM
- spacer sizes
- spacer products
- gas
- sealants
- interlayers
- paint
- heat treatment
- frit
- categories
- muntin data
- fabrication settings/templates

Не удалять и не заменять эти данные без явной задачи.

---

## 25. GitHub workflow
Обычный workflow:
1. изменить текущий `index.html`;
2. протестировать;
3. скачать готовый файл;
4. заменить `index.html` в GitHub;
5. commit;
6. GitHub Pages отдаёт новую версию.

`AI_PROJECT_CONTEXT.md` хранить рядом, чтобы легко продолжать работу в новом чате.

---

## 26. Как начать новый чат
Загрузить:
- `index.html`
- `AI_PROJECT_CONTEXT.md`

И отправить:

```text
Прочитай сначала AI_PROJECT_CONTEXT.md полностью.
Затем изучи актуальный index.html.

Это текущая рабочая версия IGU Builder.
Продолжаем разработку именно с неё.

Не переписывай проект с нуля.
Не возвращай Supabase, db.json или старую архитектуру с базой данных.
Не удаляй Smart Shape / Fabrication / CAD.

При изменениях:
1. найди конкретный существующий код;
2. внеси минимальные безопасные правки;
3. сохрани весь остальной функционал;
4. верни мне полный готовый index.html для замены на GitHub.
```

---

## 27. Приоритеты production UI
Если есть конфликт между красотой и производственной читаемостью, приоритет:

1. точность;
2. QTY;
3. WIDTH;
4. HEIGHT;
5. make-up identification;
6. ч/б читаемость;
7. fit на Letter landscape;
8. визуальная аккуратность.

Производственный сотрудник должен понять лист быстро, без расшифровки декоративных UI-элементов.

---

## 28. Ключевые решения, которые нельзя случайно откатить
Сохранять:
- standalone architecture;
- no database;
- 1 initial Order Line;
- Project removed;
- Due Date calendar;
- Letter landscape;
- black-and-white print;
- compact Company / PO / Order;
- compact MAKE-UP block;
- no ACTUAL CAVITY;
- no IGU BUILD-UP caption;
- Cavity in one line;
- visible LoE / COATED marker;
- Order Lines order:
  `LINE → QTY → WIDTH → HEIGHT → MARK → SQ-FT → M-U`;
- QTY/WIDTH/HEIGHT are visually primary;
- MARK/SQ-FT are secondary and compact;
- one clean row separator, no double underline;
- B/C make-ups clearly highlighted on monochrome print.

---

## 29. Rule for future AI edits
The uploaded current `index.html` is the source of truth.

Do not assume an older version is better.

Before editing:
1. inspect the current file;
2. locate exact HTML/CSS/JS;
3. patch minimally;
4. preserve embedded data and production functionality;
5. return a complete updated `index.html`.
