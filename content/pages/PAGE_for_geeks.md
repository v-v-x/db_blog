---
title: 'Для гиков'
date: Fri, 24 Apr 2015 08:15:11 +0000
draft: false
---

### EPUB изнутри

На самом деле EPUB — это ZIP-файл, сжатый особым образом. В этом легко убедиться: измените расширение любого файла _.epub_ на .zip и разархивируйте его любым архиватором. Внутри этого архива вы обнаружите файлы содержимого книги (в формате XHTML), дополнительные файлы иллюстраций, шрифтов и т. д., и обязательные вспомогательные файлы, которые стандарт EPUB требует для описания книги. Можно считать, что EPUB — это веб-сайт, с некоторыми ограничениями и созданный по особым правилам.

Минимальный набор файлов, который входит в EPUB (речь о EPUB 2, пока самом распространённом, который с большей или меньшей гарантией будет прочитан любой программой или устройством, поддерживающим EPUB) , должен быть следующим:

```
.
├── mimetype
├── META-INF
│   └── container.xml
└── OEBPS
    ├── content.opf
    ├── toc.ncx
    ├── Text
    ├── Styles
    ├── Fonts
    ├── Images
    ├── Video
    ├── Audio
    └── Misc
```



* файл _mimetype_, единственный файл внутри EPUB, который остаётся несжатым в составе архива, с одной единственной строчкой `application/epub+zip`,

* папка _META-INF_ с файлом _container.xml_, который указывает, где хранится содержимое книги,

* папка _OEBPS_, в которой должны быть:
  *   файл с метаданными книги, списком всех файлов, которые нужны для ее содержимого, описанием последовательности чтения файлов и путеводителем по ключевым файлам (обычно он называется _content.opf_ ),
  *   файл _toc.ncx_ — техническое оглавление книги в том виде, в котором оно будет считано программой для чтения и показано пользователю не внутри книги, а при клике на соответствующую кнопке интерфейса,
  *   файл _stylesheet.css_ с описанием таблицы стилей для оформления текста (CSS-файлов может быть несколько, обычно их группируют в папку *Styles*),
  *   собственно файлы содержимого, например, в папке *Text*,
  *   папка с иллюстрациями *Images*, шрифтами *Fonts* и т. д.

  #### Ключевой файл *content.opf*

  *Content.opf* — главный файл EPUB, в котором хранятся метаданные книги, опись всего содержимого EPUB и определяет для программы чтения последовательность воспроизведения книги. Он состоит из трёх (EPUB3) или четырёх (EPUB2) разделов:

  `<metadata>…</metadata>` — метаданные (или выходные данные) книги в формате Dubline Core, то есть язык, название книги, :её автор, аннотация, идентификатор, название файла обложки:

  ```xml
<?xml version="1.0" encoding="utf-8"?>
<package version="2.0" unique-identifier="BookId" xmlns="http://www.idpf.org/2007/opf">
  <metadata xmlns:opf="http://www.idpf.org/2007/opf" xmlns:dc="http://purl.org/dc/elements/1.1/">
    <dc:language>ru</dc:language>
    <dc:title>Пир во время чумы</dc:title>
    <dc:creator opf:role="aut">Александр Пушкин</dc:creator>
    <dc:description>Эта маленькая пьеса, состоящая из одной сцены, является переводом фрагмента из пьесы шотландского поэта Джона Вильсона «Чумной город», посвящённой лондонской чуме 1665 года.</dc:description>
    <meta content="0.9.14" name="Sigil version" />
    <dc:date xmlns:opf="http://www.idpf.org/2007/opf" opf:event="modification">2020-03-26</dc:date>
    <dc:identifier opf:scheme="UUID" id="BookId">urn:uuid:3599715c-ad56-4270-b21c-07c9c0ce3d31</dc:identifier>
    <meta name="cover" content="cover.jpg" />
  </metadata>
  ```
  
`<manifest>…</manifest>` — полная опись файлов внутри EPUB:
  
```xml
    <manifest>
      <item id="ncx" href="toc.ncx" media-type="application/x-dtbncx+xml"/>
      <item id="cover.xhtml" href="Text/cover.xhtml" media-type="application/xhtml+xml"/>
    <item id="cover.jpg" href="Images/cover.jpg" media-type="image/jpeg"/>
      <item id="title.xhtml" href="Text/title.xhtml" media-type="application/xhtml+xml"/>
    <item id="text.xhtml" href="Text/text.xhtml" media-type="application/xhtml+xml"/>
      <item id="styles.css" href="Styles/styles.css" media-type="text/css"/>
  </manifest>
  ```

  `<spine>…</spine>` — «корешок», то есть последовательность воспроизведения текстовых файлов:

  ```xml
  <spine toc="ncx">
      <itemref idref="cover.xhtml"/>
      <itemref idref="title.xhtml"/>
      <itemref idref="text.xhtml"/>
    </spine>
  ```
  
  `<guide>…</guide>` — «путеводитель по книге», то есть описание назначения ключевых текстовых файлов:
  
  ```xml
    <guide>
      <reference type="cover" title="Cover" href="Text/cover.xhtml"/>
      <reference type="title-page" title="Title Page" href="Text/title.xhtml"/>
      <reference type="text" title="Text" href="Text/text.xhtml"/>
    </guide>
  </package>
  ```

### EPUB для продвинутых пользователей

Программисту и гику разобраться в том, что такое EPUB, будет совсем несложно. Достаточно ознакомиться со [спецификацией формата](https://www.w3.org/community/epub3/) на сайте W3Consortium. Советуем, однако, прочитать краткие руководства, написанные известным специалистом по EPUB Лайзой Дейли о формате EPUB 2 [Build a digital book with EPUB](http://www.ibm.com/developerworks/xml/tutorials/x-epubtut/index.html?ca=drs-)» и EPUB 3 «[Create rich-layout publications in EPUB 3 with HTML5, CSS3, and MathML](http://www.ibm.com/developerworks/web/library/x-richlayoutepub/index.html)». Руководства предназначены в первую очередь для тех, кто знает, что такое XHTML и CSS и как ими пользоваться. Они будут полезны и тем, кто не хочет разбираться в тонкостях XML, но может столкнуться, к примеру, с проблемами при конвертации файлов верстки в EPUB. Если же информации вам недостаточно, то рекомендуем вам книги Джарета Бьюза «[EPUB From the Ground Up: A Hands-On Guide to EPUB 2 and EPUB 3](http://goo.gl/IwlwrO)», Лизы Кастро «[EPUB: Straight to the Point](http://goo.gl/EwuLJ)», «[From InDesign CS 5.5 to EPUB and Kindle](http://goo.gl/338GQE)», Пэрайи Бёрка «[Epublishing with InDesign](http://iampariah.com/books/epublishing-with-indesign)», а также книгу Мэтта Гарриша и Маркуса Гиллинга «[EPUB 3 Best Practices](http://shop.oreilly.com/product/0636920024897.do)».

Теперь, если вы смогли дочитать это руководство до конца, то вы и в самом деле можете открыть текстовый редактор, создать все эти файлы, например, в директории _MY-EBOOK_. Имейте в виду, что размер каждого текстового файла в EPUB 2 не должен превышать 260 Кб. (Этого ограничения уже нет в EPUB 3, однако свои ограничения накладывают платформы для дистрибуции.) Вся книга может быть практически любого размера, но состоять она должна из отдельных частей.

Теперь осталось упаковать файлы таким образом, чтобы файл _mimetype_ оказался архиве первым и несжатым, а все остальные файлы сохранили структуру папки. Для этого надо перейти в папку с файлами для EPUB и выполните две команды:

```
$ zip -0Xq my-book.epub mimetype
$ zip -Xr9Dq my-book.epub
```

(То же самое можно сделать с помощью утилиты ePub Zip/Unzip или eCanCrusher.) На выходе у вас получится файл электронной книги _my-book.epub_. Не забудьте проверить его — с помощью утилиты [epubcheck](http://code.google.com/p/epubcheck/), [EPUB-checker](http://www.pagina-online.de/produkte/epub-checker/) или воспользовавшись [онлайновым валидатором IDPF](http://validator.idpf.org/). Кроме того, вы можете воспользоваться онлайновым сервисом [FlightDeck](http://ebookflightdeck.com/), чтобы узнать, как можно улучшить вашу книгу (с технической точки зрения, естественно).

Готово!

Для тех, кто всё это смог сделать, рекомендуем, чтобы не мучиться с обычными текстовыми редакторами, использовать для производства EPUB-файлов специализированными редакторами — [Sigil](https://github.com/user-none/Sigil), [BBEdit](https://www.barebones.com/products/bbedit/) или [oXygen XML Author](http://www.oxygenxml.com/xml_author.html). Самый удобный из них — редактор **Sigil**, позволяющий с нуля сделать EPUB 2 (и 3) или отредактировать его. Создавая EPUB, Sigil — на основе стандартного шаблона — генерирует базовую структуру EPUB. Все изменения файлов контента (добавление или удаление текстовых файлов, файлов шрифтов и т. д.) Sigil автоматически вносит изменения в _content.opf_ и _toc.ncx_, избавляя от большого объёма ручной работы.

Все эти редакторы, конечно, не идеальны. Sigil не слишком устойчив в работе, не всегда корректно отображает шрифты и т. д. BBEdit умеет открывать, редактировать и корректно сохранять EPUB, но не умеет автоматически исправлять состав `manifest` внутри *content.opf*. Редактор oXygen XML Author — вариант программы oXygen XML Editor, мощного XML-редактора, предназначенного для работы со структурированными данными индустриальных масштабов. Это и его минус — для вёрстки электронных книг его функции избыточны, а в настройках легко разберется только профессионал в XML.

Для дизайнеров книг более привычным, скорее всего, окажется создание EPUB не в специализированном редакторе, а в привычной и знакомой программе вёрстки InDesign. Подробнее об этом читайте [дальше](/pages/page_indesign_to_epub). Впрочем, без редактора, скорее всего, не обойтись при доведении EPUB, конвертированного в InDesign, до кондиции.