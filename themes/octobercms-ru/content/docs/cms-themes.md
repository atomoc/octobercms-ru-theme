# Темы ( Themes )

- [ Введение](#introduction)
- [ Подпапки](#subdirectories)
- [ Структура шаблона](#structure)

Темы определяют внешний вид вашего сайта или веб-приложения, созданного на October. Темы в October все полностью в файлах и могут управляться с помощью любой системы контроля версий, например - Git. Эта страница дает вам высокоуровневое описание тем в October. Вы можете найти больше информации о [страницах](./cms-pages.md), [чанках](./cms-partials.md), [макетах](./cms-layouts.md) и [файлах содержимого](./cms-content.md) в соответствующих статьях.

> Активная тема CMS устанавливается с помощью параметра `activeTheme` в файле `app/config/cms.php`.

## <a name="introduction" class="anchor" href="#introduction"></a> Введение

Темы - это папки, которые находятся в папке **/themes** по умолчанию. Темы могут содержать следующие объекты.

- [Страницы](./cms-pages.md) - представляют собой страницы сайта.
- [Чанки](./cms-partials.md) - содержит повторно используемые куски HTML-разметки.
- [Макеты](./cms-layouts.md) - определяет каркас страницы.
- [Содержимое](./cms-content.md) - текст, HTML или [Markdown](http://daringfireball.net/projects/markdown/syntax) блоки, которые можно редактировать отдельно от страницы или макета.
- **Асетсы** - файлы-ресурсы такие как изображения, CSS и JavaScript.

Ниже вы можете видеть пример структуры папок темы. Каждая тема в October представлена отдельной папкой. Пример показывает папку темы "website".

    themes/
      website/              <== Тема начинается здесь
        pages/              <== Папка со страницами 
          home.htm
        layouts/            <== Папка с макетами
          default.htm
        partials/           <== Папка с чанками
          sidebar.htm
        content/            <== Папка с контентом
          intro.htm
        assets/             <== Папка с асетсами
          css/
            my-styles.css
          js/
          images/

## <a name="subdirectories" class="anchor" href="#subdirectories"></a> Подпапки

October поддерживает одноуровневые подпапки для страниц, чанков, макетов и файлов содержимого (папка **assets** может содержать любую структуру). Это упрощает организацию крупных сайтов. В примере структуры папок ниже вы можете выдеть, что папки страниц и чанков содержат подпапку **blog**, а папка content содержит подпапку **home**.

    themes/
      website/
        pages/
          home.htm
          blog/                 <== Подпапка
            archive.htm
            category.htm
        partials/
          sidebar.htm
          blog/                 <== Подпапка
            category-list.htm
        content/
          footer-contacts.txt
          home/                 <== Подпапка
            intro.htm
        ...
Чтобы сослаться на чанк или файл содержимого в подпапке, укажите имя подпапки перед имене шаблона. Пример отображения чанка из подпапки.

    {% partial "blog/category-list" %}

> **Важно:** Пути к шаблонам всегда абсолютные. Если в чанке вы отображаете другой чанк из той же подпапки, вам все равно нужно указать имя подпапки.

## <a name="structure" class="anchor" href="#structure"></a> Структура шаблона

Шаблоны страниц, чанков и макетов могут включать в себя 3 раздела: **конфигурация**, **PHP код**, and **Twig разметка**.
Разделы отделяются с помощью символов `==`.
Пример:

    url = "/blog"
    layout = "default"
    ==
    function onStart()
    {
        $this['posts'] = ...;
    }
    ==
    <h3>Blog archive</h3>
    {% for post in posts %}
        <h4>{{ post.title }}</h4>
        {{ post.content }}
    {% endfor %}

### <a name="configuration-section" class="anchor" href="#configuration-section"></a> Раздел конфигурации

Раздел конфигруации устанавливает параметры шаблона. Поддерживаемые параметры конфигурации специфичны для различных шаблонов CMS и описаны в соответствующих статьях документации. Раздел конфигурации испоьзует простой [INI формат](http://en.wikipedia.org/wiki/INI_file), где значения строковых параметров заключены в кавычки. Пример раздела конфигурации для шаблона страницы:

    url = "/blog"
    layout = "default"

    [component]
    parameter = "value"

### <a name="php-section" class="anchor" href="#php-section"></a> Раздел PHP кода

Код в разделе PHP выполняется каждый раз перед тем, как шаблон будет отображен. Раздел PHP не обязателен для всех шаблонов CMS и его содержимое зависит от типа шаблона, где он определен. Раздел PHP кода может содержать необязательные открывающие и закрывающие PHP теги, чтобы поддерживать подстветку синтаксиса в редакторах. Открывающие и закрывающие теги всегда должны быть заданы на других строках, отличных от строк с разделителями разделов `==`.

    url = "/blog"
    layout = "default"
    ==
    <?
    function onStart()
    {
        $this['posts'] = ...;
    }
    ?>
    ==
    <h3>Blog archive</h3>
    {% for post in posts %}
        <h4>{{ post.title }}</h4>
        {{ post.content }}
    {% endfor %}

> **Важно:** В разделе PHP вы можете задать только функции и сослаться на пространство имен с помощью ключевого слова `use`. Другой PHP код не разрешен в разделе PHP. Это потому что раздел PHP конвертируется в класс, когда обрабатывается страница.

Пример ссылки на пространство имен:

    url = "/blog"
    layout = "default"
    ==
    <?
    use Acme\Blog\Classes\Post;

    function onStart()
    {
      $this['posts'] = Post::get();
    }
    ?>
    ==

### <a name="twig-section" class="anchor" href="#twig-section"></a> Раздел Twig разметки

Раздел Twig определяет разметку, которая будет отображена согласно шаблону. В разделе Twig вы можете использовать [функции, фильтры и теги Twig, предоставляемые October](./cms-markup.md) и все оригинальные функции, фильтры и теги [Twig](http://twig.sensiolabs.org/documentation). Содержимое в разделе Twig завивит от типа шаблона (страница, макет или чанк). Вы можете найти больше информации о конкретных объектах Twig далее в документации.
