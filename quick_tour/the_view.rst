Layer Tampilan (View)
========

Setelah membaca bagian pertama dari tutorial ini, kamu pasti beranggapan 
sebaiknya kita lanjutkan tutorial Symfony2 ini. Dalam bagian yg kedua ini,
kita akan mempelajari tentang `Twig`_, sebuah pengolah template PHP yg 
cepat, fleksibel dan aman. Twig membuat template yang kita buat lebih mudah
dibaca, ringkas, dan mudah dipahami oleh web desainer.

Mempelajari Twig lebih dalam
--------------------------

`Dokumentasi resmi Twig`_ merupakan sumber terbaik untuk mempelajari segala
sesuatu tentang pengolah template yang baru ini. Bagian ini hanya menjelaskan
secara singkat tentang konsep utamanya.

Template Twig berupa sebuah file yang dapat digunakan untuk menghasilkan berbagai
macam tipe dokumen (HTML, CSS, JavaScript, XML, CSV, LaTeX, ...). Elemen khusus
Twig ditandai secara khusus dengan menggunakan karakter pemisah berikut ini:

* ``{{ ... }}``: mencetak isi variabel atau hasil ekspresi;

* ``{% ... %}``: mengatur alur berpikir template, misalnya untuk menjalankan
  perintah perulangan ``for`` dan ``if``;

* ``{# ... #}``: untuk menambahkan komentar di dalam template.

Berikut ini contoh penggunaan ketiga perintah di atas dalam template sederhana
dengan dua variabel ``page_title`` dan ``navigation`` yang sudah diberikan ke
template:

.. code-block:: html+jinja

    <!DOCTYPE html>
    <html>
        <head>
            <title>{{ page_title }}</title>
        </head>
        <body>
            <h1>{{ page_title }}</h1>

            <ul id="navigation">
                {% for item in navigation %}
                    <li><a href="{{ item.url }}">{{ item.label }}</a></li>
                {% endfor %}
            </ul>
        </body>
    </html>

Untuk menampilkan template dalam Symfony, gunakan fungsi ``render`` di dalam
kontroller dan berikan variabel yang dibutuhkan sebagai array
di parameter ke dua yang bersifat opsional:

.. code-block:: jinja

    $this->render('AcmeDemoBundle:Demo:hello.html.twig', array(
        'name' => $name,
    ));

Variabel yang diberikan ke template dapat berupa string, array, atau bahkan
berupa objek. Twig sudah membuat fungsi abstrak untuk menyamakan perbedaan
diantara tiga tipe tersebut sehingga kita bisa mengakses atribut dari variabel
dengan notasi (``.``) dot. Kode berikut ini memberi contoh cara menampilkan
isi variabel sesuai tipe variabel yang diberikan dari kontroller:

.. code-block:: jinja

    {# 1. Variabel sederhana #}
    {# array('name' => 'Fabien') #}
    {{ name }}

    {# 2. Array #}
    {# array('user' => array('name' => 'Fabien')) #}
    {{ user.name }}

    {# sintak lain untuk array #}
    {{ user['name'] }}

    {# 3. Objek #}
    {# array('user' => new User('Fabien')) #}
    {{ user.name }}
    {{ user.getName }}

    {# sintak lain untuk objek #}
    {{ user.name() }}
    {{ user.getName() }}

Menghias Template
--------------------

Template dalam sebuah projek biasanya menggunakan element umum yang sama, seperti
elemen header dan footer. Twig mengatasi hal ini secara elegan dengan konsep 
"Pewarisan Template". Konsep ini memungkinkan kita untuk membuat sebuah template
dasar yang mengandung semua elemen umum dari web kita dengan beberapa definisi
"blok", dimana blok ini dapat diganti isinya dari template anak.

Berikut ini contoh template ``hello.html.twig`` yang menggunakan tag ``extends``
untuk mendefinisikan ``layout.html.twig`` sebagai template induk:

.. code-block:: html+jinja

    {# lokasi file : src/Acme/DemoBundle/Resources/views/Demo/hello.html.twig #}
    {% extends "AcmeDemoBundle::layout.html.twig" %}

    {% block title "Halo " ~ name %}

    {% block content %}
        <h1>Halo {{ name }}!</h1>
    {% endblock %}

Pasti kita sudah terbiasa dengan kode ``AcmeDemoBundle::layout.html.twig``.
Kode ini digunakan untuk mereferensikan lokasi file template. Bagian ``::`` 
menandakan bahwa tidak ada nama kontroller di lokasi template, jadi file tersebut
berada tepat di folder ``Resources/views/`` dari folder bundle.

Sekarang kita sederhanakan lagi file template ``layout.html.twig`` sebagai berikut:

.. code-block:: jinja

    {# src/Acme/DemoBundle/Resources/views/layout.html.twig #}
    <div>
        {% block content %}
        {% endblock %}
    </div>

Kode tag ``{% block %}`` menandakan bahwa bagian ini bisa didefinisikan ulang
oleh template anak. Pada contoh di atas, file template ``hello.html.twig``
mendefinisikan ulang bagian ``content``. Apabila variabel ``name`` diberi nilai
``Fabien``, berarti tulisan "Halo Fabien!" akan ditampilkan didalam elemen ``<div>``
seperti berikut:

.. code-block:: jinja

    <div>
        <h1>Halo Fabien!</h1>
    </div>
    
Using Tags, Filters, and Functions
----------------------------------

One of the best feature of Twig is its extensibility via tags, filters, and
functions. Take a look at the following sample template that uses filters
extensively to modify the information before displaying it to the user:

.. code-block:: jinja

    <h1>{{ article.title|trim|capitalize }}</h1>

    <p>{{ article.content|striptags|slice(0, 1024) }}</p>

    <p>Tags: {{ article.tags|sort|join(", ") }}</p>

    <p>Next article will be published on {{ 'next Monday'|date('M j, Y')}}</p>

Don't forget to check out the official `Twig documentation`_ to learn everything
about filters, functions and tags.

Including other Templates
~~~~~~~~~~~~~~~~~~~~~~~~~

The best way to share a snippet of code between several templates is to create a
new template fragment that can then be included from other templates.

First, create an ``embedded.html.twig`` template:

.. code-block:: jinja

    {# src/Acme/DemoBundle/Resources/views/Demo/embedded.html.twig #}
    Hello {{ name }}

And change the ``index.html.twig`` template to include it:

.. code-block:: jinja

    {# src/Acme/DemoBundle/Resources/views/Demo/hello.html.twig #}
    {% extends "AcmeDemoBundle::layout.html.twig" %}

    {# override the body block from embedded.html.twig #}
    {% block content %}
        {{ include("AcmeDemoBundle:Demo:embedded.html.twig") }}
    {% endblock %}

Embedding other Controllers
~~~~~~~~~~~~~~~~~~~~~~~~~~~

And what if you want to embed the result of another controller in a template?
That's very useful when working with Ajax, or when the embedded template needs
some variable not available in the main template.

Suppose you've created a ``topArticlesAction`` controller method to display the
most popular articles of your website. If you want to "render" the result of
that method (e.g. ``HTML``) inside the ``index`` template, use the ``render``
function:

.. code-block:: jinja

    {# src/Acme/DemoBundle/Resources/views/Demo/index.html.twig #}
    {{ render(controller("AcmeDemoBundle:Demo:topArticles", {'num': 10})) }}

Here, the ``AcmeDemoBundle:Demo:topArticles`` string refers to the
``topArticlesAction`` action of the ``Demo`` controller, and the ``num``
argument is made available to the controller::

    // src/Acme/DemoBundle/Controller/DemoController.php

    class DemoController extends Controller
    {
        public function topArticlesAction($num)
        {
            // look for the $num most popular articles in the database
            $articles = ...;

            return $this->render('AcmeDemoBundle:Demo:topArticles.html.twig', array(
                'articles' => $articles,
            ));
        }

        // ...
    }

Creating Links between Pages
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Creating links between pages is a must for web applications. Instead of
hardcoding URLs in templates, the ``path`` function knows how to generate
URLs based on the routing configuration. That way, all your URLs can be easily
updated by just changing the configuration:

.. code-block:: html+jinja

    <a href="{{ path('_demo_hello', { 'name': 'Thomas' }) }}">Greet Thomas!</a>

The ``path`` function takes the route name and an array of parameters as
arguments. The route name is the key under which routes are defined and the
parameters are the values of the variables defined in the route pattern::

    // src/Acme/DemoBundle/Controller/DemoController.php
    use Sensio\Bundle\FrameworkExtraBundle\Configuration\Route;
    use Sensio\Bundle\FrameworkExtraBundle\Configuration\Template;

    // ...

    /**
     * @Route("/hello/{name}", name="_demo_hello")
     * @Template()
     */
    public function helloAction($name)
    {
        return array('name' => $name);
    }

.. tip::

    The ``url`` function is very similar to the ``path`` function, but generates
    *absolute* URLs, which is very handy when rendering emails and RSS files:
    ``{{ url('_demo_hello', {'name': 'Thomas'}) }}``.

Including Assets: Images, JavaScripts and Stylesheets
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

What would the Internet be without images, JavaScripts, and stylesheets?
Symfony2 provides the ``asset`` function to deal with them easily:

.. code-block:: jinja

    <link href="{{ asset('css/blog.css') }}" rel="stylesheet" type="text/css" />

    <img src="{{ asset('images/logo.png') }}" />

The ``asset`` function's main purpose is to make your application more portable.
Thanks to this function, you can move the application root directory anywhere
under your web root directory without changing anything in your template's
code.

Final Thoughts
--------------

Twig is simple yet powerful. Thanks to layouts, blocks, templates and action
inclusions, it is very easy to organize your templates in a logical and
extensible way. However, if you're not comfortable with Twig, you can always
use PHP templates inside Symfony without any issues.

You have only been working with Symfony2 for about 20 minutes, but you can
already do pretty amazing stuff with it. That's the power of Symfony2. Learning
the basics is easy, and you will soon learn that this simplicity is hidden
under a very flexible architecture.

But I'm getting ahead of myself. First, you need to learn more about the controller
and that's exactly the topic of the :doc:`next part of this tutorial <the_controller>`.
Ready for another 10 minutes with Symfony2?

.. _Twig:               http://twig.sensiolabs.org/
.. _Dokumentasi resmi Twig: http://twig.sensiolabs.org/documentation
