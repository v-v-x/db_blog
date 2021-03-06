---
title: 'Краткая инструкция'
date: Fri, 24 Apr 2015 08:15:11 +0000
draft: false
---

### Краткая инструкция по конвертации из InDesign

1. Удалить все мягкие переносы, принудительные переносы и символы табуляции, фреймы шаблона(колонтитулы, колонцифра и пр.), если они не находятся на мастер-странице, а также выравнивание по базовой линии.
2. Проверить шрифты (все шрифты должны быть лицензированы).
3. Заменить все нелатинские символы на латинские в названиях стилей, именах файлов, названиях иллюстраций и т. д.
4. Проверить иллюстрации на соответствие стандарту EPUB (RGB, форматы JPG или PNG, наилучшее разрешение, не менее 150 ppi, лучше — 300 ppi, но не более, чем 127 kB для Kindle). Все иллюстрации должны быть привязаны к тексту. Выбрать метод встраивания подрисуночных подписей для взаимной группировки иллюстрации и подписи через задание правила для CSS или сгруппировать иллюстрации и подписи и задать растеризацию сгруппированных объектов. Оптимальные настройки графики **Preserve Appearance from Layout** и **Relative to Text Flow**.
5. Выбрать вариант встраивания таблиц.
6. Ввести метаданные (автор, название, аннотация и т. д.) в **File Info** (**File -> File Info…**).
7. Перенести техническую информацию в конец книги для улучшения читательского опыта.
8. Выбрать метод разбиения книги на части и, при необходимости поменять настройки стилей заголовков (в настройках стиля абзаца задать экспорт в CSS для стиля с `class` на `h1…h6`.
9. Проконтролировать последовательность фреймов (или определить последовательность фреймов в панели Article).
10. Подготовить отдельный стиль TOC для экспорта (стили, ссылки, убрать номера страниц).
11. Подготовить дополнительную обложку (RGB, JPG, разрешение не менее 1500 пикселей по меньшей стороне).

**Настройки экспорта**

*General*

- **Version** > **EPUB 2.0.1**
- **Cover** > **Choose Image…**
- **Navigation TOC** > **TOC Style**
- **Content Order** > **Based on Page Layout** / **Same as Articles Panel**
- **Split Document** > **Single Paragraph Style** / **Based on Paragraph Style Export Tags**

*Text*

- **Footnote Placement** > **End of the Section (Endnotes)**

*Object*

- **Preserve Appearance from Layout**
- **CSS Size** > **Relative to Text Flow**

*HTNL & CSS*

- **Include classes in HTML**, выбрать или убрать (рекомендуется) **Include Embeddable Fonts** и **Preserve Local Overrides**

*Metadata*

- Title
- Creator
- Description
- Publisher