.. index::
   single: Dasar-dasar Symfony2

Dasar-dasar Symfony2 and HTTP
=============================

Selamat! Dengan mempelajari Symfony2, anda telah berada di jalur yang akan membuat
anda menjadi pengembang web yang lebih *produktif*, *paripurna*, dan *populer*
(sebetulnya, anda harus berusaha sendiri untuk hal yang terakhir).
Symfony2 diwujudkan agar kembali ke dasar: membuat perkakas agar anda
dapat membuat dan mengembangkan aplikasi-aplikasi yang lebih tangguh,
keluar dari cara anda biasanya. Symfony diwujudkan berlandaskan ide-ide terbaik
dari berbagai teknologi: perkakas dan konsep yang akan anda pelajari mencerminkan
karya ribuan orang selama bertahun-tahun. Dengan kata lain, anda tidak hanya
mempelajari "Symfony", tapi anda mempelajari hakikat web, cara-cara terbaik
mengembangkan aplikasi, dan cara menggunakan berbagai pustaka PHP baru yang mempesona,
dengan atau tanpa Symfony2. Jadi, bersiaplah.

Bersesuaian dengan falsafah Symfony2, bab ini bermula dengan menjelaskan
hakikat konsep yang umum di bidang pengembangan web: HTTP. Tak peduli latar
belakang atau bahasa program kegemaran anda, bab ini menjadi hal
yang **harus-dibaca** untuk setiap orang.

HTTP itu Sederhana
------------------

HTTP (Hypertext Transfer Protocol, bagi penggemar komputer) adalah bahasa tulis
yang membuat dua mesin dapat berkomunikasi satu sama lain. Itu saja!
Misalnya, ketika menyelidik komik `xkcd`_, percakapan berikut (kira-kira) terjadi:

.. image:: /images/http-xkcd.png
   :align: center

Walaupun bahasa yang digunakan sedikit lebih formal, ia masih tetap sederhana.
HTTP adalah istilah yang digunakan untuk menggambarkan bahasa tulis ini.
Dan tak peduli bagaimana anda bekerja dengan web, tujuan akhir server anda adalah
*selalu* untuk memahami permintaan teks sederhana dan mengembalikan jawaban
sederhana.

Symfony2 diwujudkan berdasar dari kenyataan tersebut. Entah anda menyadari
atau tidak, HTTP adalah sesuatu yang anda gunakan tiap hari. Dengan Symfony2,
anda akan belajar cara memahaminya.

.. index::
   single: HTTP; Paradigma Permintaan-jawaban

Langkah 1: Klien Mengirim Permintaan
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Setiap percakapan di web bermula dari sebuah *permintaan*. Permintaan adalah
pesan teks yang dibuat oleh klien (misalnya peramban web, aplikasi iPhone, dll)
dalam format khusus yang dikenal sebagai HTTP. Klien mengirimkan
permintaan tersebut ke server lalu menunggu jawabannya.

Perhatikanlah bagian pertama dari interaksi (permintaan) antara peramban
dan server web xkcd:

.. image:: /images/http-xkcd-request.png
   :align: center

Dalam konteks pembicaraan HTTP, permintaan HTTP ini sebenarnya akan terlihat
seperti ini:

.. code-block:: text

    GET / HTTP/1.1
    Host: xkcd.com
    Accept: text/html
    User-Agent: Mozilla/5.0 (Macintosh)

Pesan lugas ini menyampaikan *semua hal* yang diperlukan, tepat berkaitan
dengan sumber daya yang sedang diminta klien. Baris pertama permintaan
HTTP adalah yang terpenting dan berisi dua hal: URI dan metode HTTP.

URI (misalnya ``/``, ``/kontak``, dll) adalah alamat unik atau lokasi
yang mengidentifikasi sumber daya yang diinginkan klien. Metode HTTP
(misalnya ``GET``) mendefinisikan apa yang ingin anda *lakukan* terhadap
sumber daya tersebut. Metode-metode HTTP adalah *kata-kerja* dari permintaan
tersebut dan mendefinisikan cara-cara umum yang bisa anda buat terhadap
sumber daya tersebut:

+----------+-----------------------------------------+
| *GET*    | Mengambil suatu sumber daya dari server |
+----------+-----------------------------------------+
| *POST*   | Membuat suatu sumber daya di server     |
+----------+-----------------------------------------+
| *PUT*    | Memperbarui suatu sumber daya di server |
+----------+-----------------------------------------+
| *DELETE* | Menghapus suatu sumber daya dari server |
+----------+-----------------------------------------+

Dengan menaruh informasi ini dalam benak, anda dapat membayangkan seperti
apa permintaan HTTP akan terlihat ketika akan menghapus suatu entri blog,
contohnya:

.. code-block:: text

    DELETE /blog/15 HTTP/1.1

.. note::

    Sebetulnya ada sembilan metode HTTP yang didefinisikan oleh
    spesifikasi HTTP, tapi sebagian besar dari mereka tidak banyak digunakan
    atau tidak didukung. Kenyataannya, banyak peramban web modern tidak
    mendukung metode ``PUT`` and ``DELETE``.

Selain baris pertama, permintaan HTTP selalu berisi informasi baris-baris
lainnya yang disebut tajuk permintaan (request headers). Tajuk dapat
menyediakan sejumlah besar informasi seperti ``Host`` yang diminta, format
jawaban yang diterima klien (``Accept``) dan aplikasi yang digunakan klien
untuk membuat permintaan tersebut (``User-Agent``). Ada banyak tajuk permintaan
dan dapat ditemukan di artikel Wikipedia `List of HTTP header fields`_.

Langkah 2: Server Memberikan Jawaban
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Ketika server telah menerima permintaan, ia akan tahu sumber daya yang dibutuhkan
klien (via URI) dan apa yang klien inginkan dengan sumber daya tersebut
(via metode). Misalnya, dalam kasus permintaan GET, server menyediakan sumber
daya dan membalasnya dalam bentuk jawaban HTTP. Perhatikan jawaban dari server
xkcd berikut:

.. image:: /images/http-xkcd.png
   :align: center

Jika diterjemahkan ke HTTP, jawaban yang dikirim ke peramban web akan terlihat
seperti berikut:

.. code-block:: text

    HTTP/1.1 200 OK
    Date: Sat, 02 Apr 2011 21:05:05 GMT
    Server: lighttpd/1.4.19
    Content-Type: text/html

    <html>
      <!-- ... isi HTML komic xkcd -->
    </html>

Jawaban HTTP berisi sumber daya yang diminta (dalam kasus ini adalah dokumen
HTML-nya), beserta informasi lain yang terkait jawaban tersebut. Baris pertama
sangatlah penting dan berisi kode status jawaban HTTP (dalam hal ini 200).
Kode status menyampaikan hasil keseluruhan jawaban tersebut ke klien. Apakah
permintaannya berhasil? Apakah terjadi kesalahan? Ada kode-kode status berbeda
yang menunjukkan keberhasilan, kesalahan, atau klien harus melakukan sesuatu
(misalnya, beralih ke halaman lain). Daftar lengkapnya dapat ditemukan di
artikel Wikipedia `List of HTTP status codes`_.

Sebagaimana permintaan, jawaban HTTP berisi serpihan informasi tambahan
yang dikenal sebagai tajuk HTTP (HTTP headers). Misalnya, suatu tajuk jawaban
HTTP yang penting adalah ``Content-Type``. Isi dari suatu sumber daya dapat
dikembalikan dalam bentuk berbeda-beda seperti HTML, XML, atau JSON, dan
tajuk ``Content-Type`` menggunakan Jenis-jenis Media Internet seperti
``text/html`` untuk memberitahu klien format apa yang diberikan. Daftar
jenis-jenis media yang lazim dipakai dapat ditemukan di artikel wikipedia
`List of common media types`_.

Banyak tajuk-tajuk lain tersedia, sebagiannya sangatlah digdaya. Misalnya, beberapa
tajuk dapat digunakan untuk membuat sistem tembolok (*cache*) yang ampuh.

Requests, Responses and Web Development
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This request-response conversation is the fundamental process that drives all
communication on the web. And as important and powerful as this process is,
it's inescapably simple.

The most important fact is this: regardless of the language you use, the
type of application you build (web, mobile, JSON API), or the development
philosophy you follow, the end goal of an application is **always** to understand
each request and create and return the appropriate response.

Symfony is architected to match this reality.

.. tip::

    To learn more about the HTTP specification, read the original `HTTP 1.1 RFC`_
    or the `HTTP Bis`_, which is an active effort to clarify the original
    specification. A great tool to check both the request and response headers
    while browsing is the `Live HTTP Headers`_ extension for Firefox.

.. index::
   single: Symfony2 Fundamentals; Requests and responses

Requests and Responses in PHP
-----------------------------

So how do you interact with the "request" and create a "response" when using
PHP? In reality, PHP abstracts you a bit from the whole process::

    $uri = $_SERVER['REQUEST_URI'];
    $foo = $_GET['foo'];

    header('Content-type: text/html');
    echo 'The URI requested is: '.$uri;
    echo 'The value of the "foo" parameter is: '.$foo;

As strange as it sounds, this small application is in fact taking information
from the HTTP request and using it to create an HTTP response. Instead of
parsing the raw HTTP request message, PHP prepares superglobal variables
such as ``$_SERVER`` and ``$_GET`` that contain all the information from
the request. Similarly, instead of returning the HTTP-formatted text response,
you can use the ``header()`` function to create response headers and simply
print out the actual content that will be the content portion of the response
message. PHP will create a true HTTP response and return it to the client:

.. code-block:: text

    HTTP/1.1 200 OK
    Date: Sat, 03 Apr 2011 02:14:33 GMT
    Server: Apache/2.2.17 (Unix)
    Content-Type: text/html

    The URI requested is: /testing?foo=symfony
    The value of the "foo" parameter is: symfony

Requests and Responses in Symfony
---------------------------------

Symfony provides an alternative to the raw PHP approach via two classes that
allow you to interact with the HTTP request and response in an easier way.
The :class:`Symfony\\Component\\HttpFoundation\\Request` class is a simple
object-oriented representation of the HTTP request message. With it, you
have all the request information at your fingertips::

    use Symfony\Component\HttpFoundation\Request;

    $request = Request::createFromGlobals();

    // the URI being requested (e.g. /about) minus any query parameters
    $request->getPathInfo();

    // retrieve GET and POST variables respectively
    $request->query->get('foo');
    $request->request->get('bar', 'default value if bar does not exist');

    // retrieve SERVER variables
    $request->server->get('HTTP_HOST');

    // retrieves an instance of UploadedFile identified by foo
    $request->files->get('foo');

    // retrieve a COOKIE value
    $request->cookies->get('PHPSESSID');

    // retrieve an HTTP request header, with normalized, lowercase keys
    $request->headers->get('host');
    $request->headers->get('content_type');

    $request->getMethod();          // GET, POST, PUT, DELETE, HEAD
    $request->getLanguages();       // an array of languages the client accepts

As a bonus, the ``Request`` class does a lot of work in the background that
you'll never need to worry about. For example, the ``isSecure()`` method
checks the *three* different values in PHP that can indicate whether or not
the user is connecting via a secured connection (i.e. HTTPS).

.. sidebar:: ParameterBags and Request Attributes

    As seen above, the ``$_GET`` and ``$_POST`` variables are accessible via
    the public ``query`` and ``request`` properties respectively. Each of
    these objects is a :class:`Symfony\\Component\\HttpFoundation\\ParameterBag`
    object, which has methods like
    :method:`Symfony\\Component\\HttpFoundation\\ParameterBag::get`,
    :method:`Symfony\\Component\\HttpFoundation\\ParameterBag::has`,
    :method:`Symfony\\Component\\HttpFoundation\\ParameterBag::all` and more.
    In fact, every public property used in the previous example is some instance
    of the ParameterBag.

    .. _book-fundamentals-attributes:

    The Request class also has a public ``attributes`` property, which holds
    special data related to how the application works internally. For the
    Symfony2 framework, the ``attributes`` holds the values returned by the
    matched route, like ``_controller``, ``id`` (if you have an ``{id}``
    wildcard), and even the name of the matched route (``_route``). The
    ``attributes`` property exists entirely to be a place where you can
    prepare and store context-specific information about the request.

Symfony also provides a ``Response`` class: a simple PHP representation of
an HTTP response message. This allows your application to use an object-oriented
interface to construct the response that needs to be returned to the client::

    use Symfony\Component\HttpFoundation\Response;
    $response = new Response();

    $response->setContent('<html><body><h1>Hello world!</h1></body></html>');
    $response->setStatusCode(200);
    $response->headers->set('Content-Type', 'text/html');

    // prints the HTTP headers followed by the content
    $response->send();

If Symfony offered nothing else, you would already have a toolkit for easily
accessing request information and an object-oriented interface for creating
the response. Even as you learn the many powerful features in Symfony, keep
in mind that the goal of your application is always *to interpret a request
and create the appropriate response based on your application logic*.

.. tip::

    The ``Request`` and ``Response`` classes are part of a standalone component
    included with Symfony called HttpFoundation. This component can be
    used entirely independently of Symfony and also provides classes for handling
    sessions and file uploads.

The Journey from the Request to the Response
--------------------------------------------

Like HTTP itself, the ``Request`` and ``Response`` objects are pretty simple.
The hard part of building an application is writing what comes in between.
In other words, the real work comes in writing the code that interprets the
request information and creates the response.

Your application probably does many things, like sending emails, handling
form submissions, saving things to a database, rendering HTML pages and protecting
content with security. How can you manage all of this and still keep your
code organized and maintainable?

Symfony was created to solve these problems so that you don't have to.

The Front Controller
~~~~~~~~~~~~~~~~~~~~

Traditionally, applications were built so that each "page" of a site was
its own physical file:

.. code-block:: text

    index.php
    contact.php
    blog.php

There are several problems with this approach, including the inflexibility
of the URLs (what if you wanted to change ``blog.php`` to ``news.php`` without
breaking all of your links?) and the fact that each file *must* manually
include some set of core files so that security, database connections and
the "look" of the site can remain consistent.

A much better solution is to use a :term:`front controller`: a single PHP
file that handles every request coming into your application. For example:

+------------------------+------------------------+
| ``/index.php``         | executes ``index.php`` |
+------------------------+------------------------+
| ``/index.php/contact`` | executes ``index.php`` |
+------------------------+------------------------+
| ``/index.php/blog``    | executes ``index.php`` |
+------------------------+------------------------+

.. tip::

    Using Apache's ``mod_rewrite`` (or equivalent with other web servers),
    the URLs can easily be cleaned up to be just ``/``, ``/contact`` and
    ``/blog``.

Now, every request is handled exactly the same way. Instead of individual URLs
executing different PHP files, the front controller is *always* executed,
and the routing of different URLs to different parts of your application
is done internally. This solves both problems with the original approach.
Almost all modern web apps do this - including apps like WordPress.

Stay Organized
~~~~~~~~~~~~~~

Inside your front controller, you have to figure out which code should be
executed and what the content to return should be. To figure this out, you'll
need to check the incoming URI and execute different parts of your code depending
on that value. This can get ugly quickly::

    // index.php
    use Symfony\Component\HttpFoundation\Request;
    use Symfony\Component\HttpFoundation\Response;
    $request = Request::createFromGlobals();
    $path = $request->getPathInfo(); // the URI path being requested

    if (in_array($path, array('', '/'))) {
        $response = new Response('Welcome to the homepage.');
    } elseif ($path == '/contact') {
        $response = new Response('Contact us');
    } else {
        $response = new Response('Page not found.', 404);
    }
    $response->send();

Solving this problem can be difficult. Fortunately it's *exactly* what Symfony
is designed to do.

The Symfony Application Flow
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When you let Symfony handle each request, life is much easier. Symfony follows
the same simple pattern for every request:

.. _request-flow-figure:

.. figure:: /images/request-flow.png
   :align: center
   :alt: Symfony2 request flow

   Incoming requests are interpreted by the routing and passed to controller
   functions that return ``Response`` objects.

Each "page" of your site is defined in a routing configuration file that
maps different URLs to different PHP functions. The job of each PHP function,
called a :term:`controller`, is to use information from the request - along
with many other tools Symfony makes available - to create and return a ``Response``
object. In other words, the controller is where *your* code goes: it's where
you interpret the request and create a response.

It's that easy! To review:

* Each request executes a front controller file;

* The routing system determines which PHP function should be executed based
  on information from the request and routing configuration you've created;

* The correct PHP function is executed, where your code creates and returns
  the appropriate ``Response`` object.

A Symfony Request in Action
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Without diving into too much detail, here is this process in action. Suppose
you want to add a ``/contact`` page to your Symfony application. First, start
by adding an entry for ``/contact`` to your routing configuration file:

.. configuration-block::

    .. code-block:: yaml

        # app/config/routing.yml
        contact:
            path:     /contact
            defaults: { _controller: AcmeDemoBundle:Main:contact }

    .. code-block:: xml

        <?xml version="1.0" encoding="UTF-8" ?>
        <routes xmlns="http://symfony.com/schema/routing"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://symfony.com/schema/routing
                http://symfony.com/schema/routing/routing-1.0.xsd">

            <route id="contact" path="/contact">
                <default key="_controller">AcmeDemoBundle:Main:contact</default>
            </route>
        </routes>

    .. code-block:: php

        // app/config/routing.php
        use Symfony\Component\Routing\RouteCollection;
        use Symfony\Component\Routing\Route;

        $collection = new RouteCollection();
        $collection->add('contact', new Route('/contact', array(
            '_controller' => 'AcmeDemoBundle:Main:contact',
        )));

        return $collection;

.. note::

   This example uses :doc:`YAML </components/yaml/yaml_format>` to define the routing
   configuration. Routing configuration can also be written in other formats
   such as XML or PHP.

When someone visits the ``/contact`` page, this route is matched, and the
specified controller is executed. As you'll learn in the :doc:`routing chapter </book/routing>`,
the ``AcmeDemoBundle:Main:contact`` string is a short syntax that points to a
specific PHP method ``contactAction`` inside a class called ``MainController``::

    // src/Acme/DemoBundle/Controller/MainController.php
    namespace Acme\DemoBundle\Controller;

    use Symfony\Component\HttpFoundation\Response;

    class MainController
    {
        public function contactAction()
        {
            return new Response('<h1>Contact us!</h1>');
        }
    }

In this very simple example, the controller simply creates a
:class:`Symfony\\Component\\HttpFoundation\\Response` object with the HTML
``<h1>Contact us!</h1>``. In the :doc:`controller chapter </book/controller>`,
you'll learn how a controller can render templates, allowing your "presentation"
code (i.e. anything that actually writes out HTML) to live in a separate
template file. This frees up the controller to worry only about the hard
stuff: interacting with the database, handling submitted data, or sending
email messages.

Symfony2: Build your App, not your Tools.
-----------------------------------------

You now know that the goal of any app is to interpret each incoming request
and create an appropriate response. As an application grows, it becomes more
difficult to keep your code organized and maintainable. Invariably, the same
complex tasks keep coming up over and over again: persisting things to the
database, rendering and reusing templates, handling form submissions, sending
emails, validating user input and handling security.

The good news is that none of these problems is unique. Symfony provides
a framework full of tools that allow you to build your application, not your
tools. With Symfony2, nothing is imposed on you: you're free to use the full
Symfony framework, or just one piece of Symfony all by itself.

.. index::
   single: Symfony2 Components

Standalone Tools: The Symfony2 *Components*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

So what *is* Symfony2? First, Symfony2 is a collection of over twenty independent
libraries that can be used inside *any* PHP project. These libraries, called
the *Symfony2 Components*, contain something useful for almost any situation,
regardless of how your project is developed. To name a few:

* :doc:`HttpFoundation </components/http_foundation/introduction>` - Contains
  the ``Request`` and ``Response`` classes, as well as other classes for handling
  sessions and file uploads;

* :doc:`Routing </components/routing/introduction>` - Powerful and fast routing system that
  allows you to map a specific URI (e.g. ``/contact``) to some information
  about how that request should be handled (e.g. execute the ``contactAction()``
  method);

* `Form`_ - A full-featured and flexible framework for creating forms and
  handling form submissions;

* `Validator`_ - A system for creating rules about data and then validating
  whether or not user-submitted data follows those rules;

* :doc:`ClassLoader </components/class_loader/introduction>` - An autoloading library that allows
  PHP classes to be used without needing to manually ``require`` the files
  containing those classes;

* :doc:`Templating </components/templating/introduction>` - A toolkit for rendering
  templates, handling template inheritance (i.e. a template is decorated with
  a layout) and performing other common template tasks;

* `Security`_ - A powerful library for handling all types of security inside
  an application;

* `Translation`_ - A framework for translating strings in your application.

Each and every one of these components is decoupled and can be used in *any*
PHP project, regardless of whether or not you use the Symfony2 framework.
Every part is made to be used if needed and replaced when necessary.

The Full Solution: The Symfony2 *Framework*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

So then, what *is* the Symfony2 *Framework*? The *Symfony2 Framework* is
a PHP library that accomplishes two distinct tasks:

#. Provides a selection of components (i.e. the Symfony2 Components) and
   third-party libraries (e.g. `Swift Mailer`_ for sending emails);

#. Provides sensible configuration and a "glue" library that ties all of these
   pieces together.

The goal of the framework is to integrate many independent tools in order
to provide a consistent experience for the developer. Even the framework
itself is a Symfony2 bundle (i.e. a plugin) that can be configured or replaced
entirely.

Symfony2 provides a powerful set of tools for rapidly developing web applications
without imposing on your application. Normal users can quickly start development
by using a Symfony2 distribution, which provides a project skeleton with
sensible defaults. For more advanced users, the sky is the limit.

.. _`xkcd`: http://xkcd.com/
.. _`HTTP 1.1 RFC`: http://www.w3.org/Protocols/rfc2616/rfc2616.html
.. _`HTTP Bis`: http://datatracker.ietf.org/wg/httpbis/
.. _`Live HTTP Headers`: https://addons.mozilla.org/en-US/firefox/addon/live-http-headers/
.. _`List of HTTP status codes`: http://en.wikipedia.org/wiki/List_of_HTTP_status_codes
.. _`List of HTTP header fields`: http://en.wikipedia.org/wiki/List_of_HTTP_header_fields
.. _`List of common media types`: http://en.wikipedia.org/wiki/Internet_media_type#List_of_common_media_types
.. _`Form`: https://github.com/symfony/Form
.. _`Validator`: https://github.com/symfony/Validator
.. _`Security`: https://github.com/symfony/Security
.. _`Translation`: https://github.com/symfony/Translation
.. _`Swift Mailer`: http://swiftmailer.org/
