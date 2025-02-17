---
title: '2. Из InDesign в EPUB'
date: Fri, 24 Apr 2015 07:28:26 +0000
draft: false
type: page
menu: ["side"]
---

Для подготовки конвертации макета книги, сделанного в InDesign, вам понадобится, понятно, сам **InDesign** (желательно, CC, то есть Creative Cloud) со всеми последними апдейтами, а также наиболее адекватно отображающие EPUB программы — **Adobe Digital Editions**, **Readium** для Chrome или **Books** для macOS. Сама по себе конвертация — довольно быстрая и несложная процедура. Однако перед конвертацией макет должен быть корректно подготовлен, чтобы соответствовать стандарту EPUB, сохранить максимум форматирования текста, не потерять его фрагменты, нужные иллюстрации и т. д. Будьте готовы к тому, что подготовка к конвертации может занять — в зависимости от сложности исходного макета — значительное время. Кроме того, после конвертации вам может понадобиться внести правку в код файлов EPUB, для чего можно использовать бесплатный редактор [Sigil](https://github.com/Sigil-Ebook/Sigil/releases).

Иногда может оказаться проще сверстать книгу заново, чем переделывать макет в соответствии с требованиями конвертации.

### Подготовка макета для экспорта в EPUB

#### Подготовка текста

InDesign — прекрасный WYSIWYG-редактор для PDF, но не для EPUB, поскольку EPUB предполагает некоторые ограничения, которых нет при подготовки PDF, и наоборот. Например — физические параметры макета. Очевидно, что при создании макета бумажной книги первым шагом является выбор размеров печатного блока. Для подготовки EPUB это  не имеет значения. Ориентироваться на представление макета в InDesign для просмотра EPUB невозможно — электронная книга все равно будет выглядеть не так, как напечатанная. При конвертации InDesign опустит всю информацию из мастер-страниц, где при правильной верстке дизайнеры размещают колонтитулы, колонцифры и другие регулярные элементы макета. Программы для чтения EPUB не отображают колонтитулов из макетов, хотя некоторые программы (например, Books для macOS) имитируют колонтитулы, извлекая информацию для него из тега _title_ файла, в которую попадает название файла InDesign. На это обстоятельство следует обратить внимание тем дизайнерам, которые склонны размещать регулярные элементы оформления макета не на мастер-страницах, а в основной верстке. В этом случае при конвертации все эти элементы подверстаются в конец книги, и их придется вычищать из кода вручную. Собственно, это следует из самой важной особенности EPUB — отсутствия разбиения на страницы: если нет страниц, то нет и колонтитулов и пр.

Нужно с особой аккуратностью относиться к навыкам размещения текста на странице, которые преследуют цель избежать висячих строк в начале и конце абзацев. Стоит рекомендовать дизайнерам отказаться от практики расстановки мягких или, тем более, принудительных переносов и разрывов строки: при конвертации переносы будут отображаться в виде дефисов, а разрыв строки — в виде принудительного переноса строки посредине абзаца `<br/>`, который довольно трудно будет отследить в коде EPUB, но он может броситься в глаза читателю электронной книги. Тех же результатов при подготовке печатного макета, но без последствий для EPUB, можно добиться использованием трекинга или командой **No Break**. Кроме того, в последних версиях InDesign появилась настройка «интеллектуального» выравнивания для абзацев (**Balanced Ragged Lines**), которую удобно использовать для выравнивания многострочных заголовков.

Как и в случае с мастер-страницами, некоторые обычные для использования опции типографики при переводе в EPUB работать перестанут, в частности, сдвиг базовой линии, выравнивание по сетке и т. д. На это следует обратить внимание, в частности, оценивая отбивку основного текста от заголовков: текст в EPUB может выглядеть совсем не так, как в макете. Из соображений надежности имеет смысл использовать настройку **Space Before** и **Space After** в свойствах абзаца.

Отдельная проблема верстки электронных книг — начать текст с новой полосы. При «бумажной» верстке дизайнер в свойствах стиля главы может просто указать **Start on Next page**. При конвертации в EPUB каждое принудительное начало полосы транслирует в CSS-правило `{page-break-before:always;}`, которое не всегда корректно отображается программами для чтения EPUB. Более надежный способ — предусмотреть разделение книги при конвертации на отдельные XHTML-файлы. InDesign предлагает для этого два способа такого разбиения. Во-первых, в настройках стиля можно указать разрезку книгу по этому стилю при экспорте в EPUB:

![Меню настроек стиля абзаца](/img/paragraph_split.png) 

Если вам нужно разделить книгу только по какому-то определенному стилю абзаца, то разбиение по нему можно задать в основных настройках конвертации (более подробно о них ниже):

![Разбиение книги по определенному стилю абзаца](/img/split.jpg)

Обратите внимание, что в этом случае желательно убрать из свойств абзаца принудительный перенос на новую полосу, поскольку в этом случае InDesign при конвертации всё равно припишет этому стилю свойство всегда разрывать страницу , а программа для чтения EPUB может перенести абзац сразу на две экрана.

Отдельное внимание стоит обратить внимание на отображение имитации *диакритики* или *символов с ударениями*, часто используемого дизайнерами в том случае, если нужный символ (например, á, é, ó, ý и т. д.) отсутствует в наборе шрифта. Комбинация буквы и знаков  [′] (Unicode: U+2032) или [´] (Unicode: U+00B4) с отрицательным трекингом при экспорте в EPUB превратится в последовательность буквы и знака — без эффекта ударения. Чтобы избежать такого результата можно подыскать необходимый символ в другом шрифте (что безразлично, если шрифты макета не будут использованы в EPUB, см. далее), или использовать знак Unicode U+301 (HTML: `&#769;`) [ &#769;] или Unicode U+341 (HTML: `&#833;`) [ &#833;], которые окажутся над буквой  после конвертации.

#### Подготовка шрифтов

Шрифты — очень важная часть макета книги. EPUB предоставляет много возможностей для их отображения (в формате _.ttf_ и _.otf_). Однако не все устройства и программы для чтения смогут их обработать. Поэтому самый безопасный, хотя и наименее выразительный, способ работы со шрифтами — отказаться от их внедрения в EPUB и пользоваться только форматированием.

В любом случае, InDesign делает все возможное: программа не только позволяет конвертировать макет в EPUB вместе со всеми шрифтами (при задании соответствующих настроек экспорта), но и вставляет в EPUB метафайл _com.apple.ibooks.display-options.xml_, который инструктирует Books пользоваться шрифтами издателя:

![Использование встроенных шрифтов](/img/css1.png)

Все, что вам нужно, — озаботиться лицензионной чистотой используемого шрифта. Или обойтись шрифтами, распространяемыми под подходящими свободными лицензиями (например, из репозитория [Google Fonts](https://fonts.google.com/)). Имейте в виду, что InDesign может, разглядев в нем охраняемое авторским правом произведение, поместить в EPUB использованный вами шрифт в урезанном виде с расширением _.ttc_ или _.otc_. Поэтому не будет лишним после конвертации зайти в EPUB и, при необходимости, вручную заменить урезанные шрифты на полные.

#### Подготовка иллюстраций

Особое внимание следует обращать на принципы конвертации иллюстраций. В ходе конвертации InDesign обрабатывает все фреймы в макете последовательно, руководствуясь простым правилом: фрейм,  который располагается правее (точнее — его левый верхний угол), будет обработан первым, а фрейм, который располагается левее — следующим, затем будет обработан фрейм, который располагается ниже и т. д.:

![Порядок обработки фреймов](/img/order.png)

Обычно дизайнеры избегают вставлять графику прямо в текст, резонно опасаясь, что при возможной перевёрстке иллюстрации могут оказаться совсем не на том месте, на котором нужно. Частично решить это проблему может опция **Custom Position** в меню **Anchored Object Options**. Но, в любом случае, только иллюстрация, вставленная в текстовый фрейм или привязанная к якорю внутри текста, будет экспортирована именно таким образом, как нужно вам, а не попадет в конец книги.

Обратите внимание, что допустимыми форматами графики для EPUB являются JPG, PNG и GIF, а разрешенным цветовым пространством — RGB и Grayscale. Все имена графических файлов должны состоять только из латинских букв, цифр и вспомогательной пунктуации.

Настроек экспорта графики в EPUB и HTML в последних версиях InDesign заметно прибавилось, но рекомендуется выставлять опции **Preserve Appearance from Layout** и **Relative to Text Flow**. Впрочем, вы можете выставить специфические настройки для отдельных иллюстраций в меню **Object** → **Object Export Options…** → **EPUB and HTML**:

![Настройка экспорта отдельной иллюстрации](/img/objectexportoptions.png)

Часто в макетах иллюстрации сопровождаются подписями. Для того, чтобы подпись не отрывалась от иллюстрации в EPUB (программы для чтения и букридеры без дополнительных инструкций свободно разбивают поток текста и объектов по экранам), можно или задать в CSS правило `{page-break-before:avoid;}`, или, сгруппировав иллюстрацию и фрейм подписи, задать в настройках экспорта сгруппированного объекта **Rasterize Container**:

![Растеризация сгруппированного объекта](/img/rasterize.png)

В этом случае InDesign в процессе конвертации растеризует иллюстрацию вместе с подписью, сгенерировав общую иллюстрацию.

##### Подготовка таблиц

Специальной подготовки таблиц для конвертации в общем случае не требуется, если основным результатом будет EPUB3, а читатели будут пользоваться современными программами для чтения и гаджетами. Однако, если рассчитывать на более широкую аудиторию, есть смысл сначала растеризовать таблицу, воспользовавшись встроенным в InDesign механизмом экспорта страниц в JPG, а затем вставить её обратно в файл макета в виде иллюстрации.

##### Подготовка обложки

В настройках экспорта в EPUB вам нужно будет выбрать один из вариантов — позволить программе растрировать первую страницу макета (в 99% случаев это не обложка, а титульный лист или авантитул), использовать готовый файл или отказаться от обложки. Файл обложки должен быть в цветовом пространстве RGB или Grayscale и пропорциях книжной страницы. 

#### Компоновка последовательности фреймов

Постарайтесь избегать верстки текста отдельными фреймами, поскольку, как и не привязанные к тексту иллюстрации, отдельные фреймы с текстом (например, фреймы с заголовками, заверстанные отдельно от основного текста) при экспорте в EPUB попадут в конец книги.

В InDesign есть отдельный инструмент, который позволяет правильно скомпоновать книгу, макет которой целиком состоит из отдельных фреймов и иллюстраций — это панель **Article**:

![Панель компоновки текста Articles](/img/articles.png)

Если вы задали последовательность компоновки текстов с помощью панели Articles, то при экспорте в EPUB не забудьте отметить это в настройках экспорта:

![Использование данных панели Article для разбиения книги](/img/article.jpg)

#### Подготовка оглавления

Последний, но не менее важный момент — оглавление книги. Оглавление для макета, подготовленного к печати, к сожалению не подойдет, поскольку, как правило, включает номера страниц печатного издания (в электронном же фиксированных страниц те) и не поддерживает ссылки на заголовки. Поэтому перед экспортом следует или переделать существующее оглавление, или подготовить стиль оглавления **Layout → Table of Contents Style…** → **New…**, в котором не будет номеров страниц **Page Number→ No Page Number, но будут предусмотрены ссылки Make text anchor in source paragraph**. На основе подготовленного вами стиля оглавления InDesign сгенерирует и _toc.ncx_, и _toc.html_, который разместит в начале EPUB.

Этап подготовки оглавления можно пропустить, точнее — перенести его на этап обработки EPUB после конвертации: редактор Sigil позволяет создать оглавление автоматически на основе заголовков `h1…h6`.

#### Форматирование абзацев и символов

Самый адекватный способ сохранить форматирование текста после его экспорта в EPUB — использовать для этого **Paragraph Styles** и **Character Styles**. Именно на стилевую разметку ориентируется InDesign при создании файла каскадных таблиц стилей (CSS), с помощью которого задает форматирование текста в электронной книге. Верстальщики книг довольно часто отступают от заданного ими же стилевого оформления, задавая какое-то локальное форматирование, не описанное в стилях абзацев или символов. Это локальное форматирование Indesign сумеет сохранить при задании соответствующей настройки экспорта:

![Сохранение локального форматирования](/img/css2.jpg)

Тем не менее, рекомендуется избегать локального форматирования, поскольку его большое количество может привести к тому, что CSS в EPUB станет слишком сложным, а код текста превратится в сплошную кашу из тегов _span_. Важный момент подготовки стилей — правило их именования то же, что и графических файлов: их имена должны состоять только из латинских букв и цифр.

#### Добавление метаданных книги

Прежде, чем приступать к конвертации, заполните форму с данными книги. по команде **File → File Info...** откроется диалоговое окно **File information**. На первой вкладке **Description**, очевидно, стоит заполнить поля **Document Title** (название книги), **Author** (автор), **Description** (аннотация), **Keywords** (рубрика каталога, к которой можно отнести книгу), выбрать **Copyright Status** (например, Copyrighted или Public Domain), внести копирайтную оговорку в поле **Copyright Notice** и вставить ссылку на условия копирайта (это актуально, в частности, для произведений, публикуемых на условиях Creative Commons).

### Конвертация в EPUB

Макет готов, и можно приступать к конвертации. По команде **File → Export…** вызывается меню, в поле **Format** которого вы выбираете **EPUB (Reflowable)**. Используйте латиницу для именования файлов EPUB — некоторые программы для чтения электронных книг не понимают кириллицу.

#### Общие настройки экспорта

После назначения имени файла появится диалог **EPUB - Reflowable Layout Export Options**.

![Меню экспорта. Общие настройки](/img/export-general.jpg)

На вкладке общих настроек **General** в поле **Version** выберите версию EPUB, которую вы хотите использовать — **EPUB 2.0.1** или **EPUB 3.0**. Имейте в виду, что EPUB 3 пока поддерживается далеко не всеми программами и устройствами для чтения EPUB, поэтому многие возможности этой версии (в частности размещение сносок во всплывающих окошках — **Text Options → Footnote Placement → Inside a Pop-up (EPUB 3)** отображаться не будут. Далее в общих настройках вы выбираете опции экспорта обложки **Cover → Choose Image…** Можно выбрать **None** (вариант без обложки), **Rasterize First Page** (InDesign превратит в обложку первую страницу книги) или **Choose Image…**, то есть выбрать готовый файл обложки. В поле **Navigation TOC** вы можете выбрать варианты построения оглавления — на основе имен файлов **File Name** (используется, как правило, при экспорте книги, собранной из отдельных файлов) или на основе стиля оглавления **Multilevel (TOC Style)** (после чего выбрать конкретный стиль). Далее в поле **Content Order** вы можете выбрать последовательность, в которой будет происходить экспорт книги: на основе последовательности страниц и, на основе структуры XML, если вы ее задавали в документе, или на основе последовательности, заданной в панели Articles — **Same as Articles Panel**. В поле **Split Document**, как мы писали выше, вы задаете правило «разрезания» книги на файлы и формирования начальных полос для глав, выбирая или настройки стилей абзацев **Based on Paragraph Style Export Tags**, или какой-то отдельный стиль абзаца **Single Paragraph Style**, по которому будет «разрезана» вся книга.

#### Настройка экспорта текста

В закладке **Text** вы определяете место для размещения сносок в книге Footnote Placement. Это может быть **After Paragraph**, размещение после абзаца, в котором есть ссылка на сноску, или **End of the section (Endnotes)** — размещение в конце главы. Последний вариант, исходя из эстетики книги, конечно, более предпочтительный. При выборе экспорта в EPUB3 вы сможете выбрать вариант **Inside a Pop-up**.

![Настройки экспорта текста](/img/text.jpg)

Настройки экспорта позволяют задать исключение из верстки принудительных переносов строк **Removed Forced Line Breaks**. Они часто используются верстальщиками при оформлении не влезающих в одну строку заголовков. Обратите внимание, что InDesign убирает эти переносы «неинтеллектуально», то есть не замещая эти переносы дополнительными пробелами. Так что в результате вы можете получить при экспорте слипшиеся слова. В меню **Bullets** и **Numbers** вы можете задать параметры экспорта, соответственно, неупорядоченных и упорядоченных списков — или **Convert to Text**, то есть в виде обычного текста, или с помощью соответствующих тегов HTML.

#### Настройки экспорта объектов и обработки иллюстраций

На вкладке **Objects** вы можете настроить параметры экспорта объектов:

![Настройка экспорта объектов](/img/objects.jpg)

Если вы хотите сохранить параметры отображения иллюстраций из макета, выберите опцию **Preserve Appearance from Layout**. Выбор опции **Use Existing Images for Graphic Objects** обеспечит отображение иллюстраций в исходном виде без конвертации. Поле **Layout** позволяет задать выключку изображения, а также отступы перед и после него. Можно даже задать вставку разрыва страницы перед или после изображением, если вы хотите, чтобы иллюстрация отображалась на отдельной странице. На этой же вкладке вы, кроме того, можете задать общие настройки CSS для размеров графики — без масштабирования (**CSS Size — Fixed**) или масштабирования, пропорционального ширине текстового поля, которое будет, естественно, разным на экранах разных пропорций (**CSS Size — Relative to Text Flow**); для выключки иллюстраций (**Layout**); для отступов перед и после иллюстраций (**Space Before** и **Space After**); для вставки разрыва страницы перед, после или перед и после иллюстраций (**Insert Page Break**). Обратите внимание, что задать обтекание иллюстрации текстом средства экспорта InDesign не позволяют. Наконец, вы можете задать игнорирование общих параметров экспорта, и тогда InDesign будет использовать те, которые были установлены для каждого отдельного графического элемента (**Ignore Object Export Settings**). На вкладке Conversion Settings верстальщик может задать общие настройки конвертации изображений.

![Настройки конвертации изображений](/img/conversion.jpg)

Например, задать формат, в который будут конвертированы все изображения (в том числе те, которые не поддерживаются стандартом EPUB, — EPS, TIFF и др.). Выбор формата можно оставить за программой (**Automatic**) или указать один из трёх форматов — **JPEG**, **GIF** или **PNG**. Кроме того, вы можете задать требуемое разрешение для графики (чем больше, тем лучше для отображения иллюстраций на цветных экранах планшетов; умеренное разрешение подойдет для черно-белых экранов букридеров) и параметры конвертации для конкретных форматов.

#### Настройка генерации CSS

Во вкладке **HTML & CSS** можно настроить экспорт классов CSS без создание файла таблиц стилей CSS или сгенерировать такой файл **Generate CSS** с параметрами отображения стилей форматирования текста.

![Настройки генерации CSS](/img/css.png)

В этом случае InDesign переводит на язык CSS параметры форматирования абзацев и символов. Какими они будут — вы можете проконтролировать, например, открыв редактирование настроек обзаца, перейдя на вкладку **Export Tagging** и изучив поле **Export Details**. Выбрав вариант **Generate CSS**, вы можете задать размеры полей для отображения на экране (этот параметр, правда, понимают далеко не все ридеры), а также выбрать сохранение локального форматирования (отличного от заданного в стилях) и включение в EPUB шрифтов документа. Кроме того, вы можете вставить в файл EPUB дополнительную таблицу стилей **Additional CSS**, например, уже использованную вами ранее. Последний вариант удобен, если вы уже усвоили CSS и понимаете, как именно будет работать ваша собственная CSS с полученным в результате экспорта файлом EPUB.

#### Дополнительные настройки

EPUB (ограниченно) поддерживает, а некоторые читалки (ограниченно) обрабатывают JavaScript, поэтому вкладка **JavaScript** может вам пригодиться, если вы хотите включить в книгу какие-то элементы интерактивности — тесты, анимацию и т. д.

![Подключение JavaScript](/img/javascript.jpg)

На вкладке **Metadata** вы можете отредактировать те данные, которые InDesign получает о книге после заполнения формы **File Info**, ввести название издательства и заменить обязательный для EPUB уникальный идентификационный номер **Identifier**, случайным образом генерируемый InDesign, на сгенерированный вами идентификатор или ISBN книги.

![Редактирование метаданных книги](/img/metadata.jpg)

Если вы хотите, чтобы сразу же после конвертации можно было посмотреть, что получилось, заполните данные на вкладке **Viewing Apps**. После экспорта готовый EPUB откроется в программе, которая определена в вашем компьютере в качестве программы для чтения EPUB по умолчанию и/или в тех, которые вам более предпочтительны.

![Настройка программ просмотра EPUB](/img/viewing.jpg)

На этом настройка конвертации в InDesign заканчивается и вы можете, наконец-то, жать **OK** и изучать, что у вас получилось.

### Работа с EPUB после экспорта

На этом ваша работа с EPUB закончилась. Скорее всего, просмотрев файл, например, в Adobe Digital Editions, вы обнаружите, что книга выглядит совсем не так или не совсем так, как вы задумывали. Не исключено, что вам еще придется повозиться с макетом, чтобы в электронном виде книга выглядела по крайней мере так, как позволяет стандарт. Кроме того, вам, скорее всего, придётся открыть EPUB в редакторе, чтобы исправить ошибки напрямую в XHTML-файлах текста или CSS-файле, задающим дизайн. Может быть, вы захотите добавить в книгу дополнительные метаданные. В частности, сейчас InDesign напрямую не позволяет это сделать, и вам придется вручную вписывать в файл *content.opf*. Более подробно познакомиться с особенностями верстки EPUB в InDesign, узнать некоторые хитрые приемы верстки (например, обход запрета на обтекание иллюстраций текстом) или подготовки EPUB для чтения в программе Books на мобильных устройствах Apple можно в книгах Лизы Кастро «[EPUB: Straight to the Point](http://goo.gl/EwuLJ)», «[From InDesign CS 5.5 to EPUB and Kindle](http://goo.gl/338GQE)» и Пэрайи Бёрка «[Epublishing with InDesign](http://iampariah.com/books/epublishing-with-indesign)» — в них содержится много полезной для профессионалов информации.