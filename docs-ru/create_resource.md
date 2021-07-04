# Создание ресурсов

Чтобы отобразить форму, обрабатывать его представление или создать новый ресурс через API, вы должны использовать **createAction** вашего **app.controller.book** службы.

```yaml
# config/routes.yaml

app_book_create:
    path: /books/new
    methods: [GET, POST]
    defaults:
        _controller: app.controller.book:createAction
```
Сделано! Теперь, когда вы перейдете к ``/books/new``,  ResourceController будет использовать factory (``app.factory.book``) для создания нового экземпляра книги. Затем он попытается создать ``app_book`` форму и установить вновь созданную книгу в качестве данных.

## Отправка формы

Вы можете использовать тот же самый маршрут для обработки отправки формы и создания книги.

```html
    <form method="post" action="{{ path('app_book_create') }}">
```
При отправке действие create с методом POST привяжет запрос к форме, и, если он действителен, он будет использовать правильный менеджер для сохранения ресурса. Затем по умолчанию он перенаправляется на ```app_book_show``` отображение созданной книги, но вы можете легко изменить это поведение - вы увидите это в следующих разделах.

При отправке действие create с методом POST привяжет запрос к форме, и, если он действителен, он будет использовать правильный менеджер для сохранения ресурса. Затем по умолчанию он перенаправляется на app_book_showотображение созданной книги, но вы можете легко изменить это поведение - вы увидите это в следующих разделах.

## Изменение шаблона

Так же, как для действий **show** и **index**  , вы можете настроить шаблон для каждого маршрута.

```yaml
# config/routes.yaml

app_book_create:
    path: /books/new
    methods: [GET, POST]
    defaults:
        _controller: app.controller.book:createAction
        _sylius:
            template: Book/create.html.twig
```
## Использование специальной формы

Вы также можете использовать настраиваемый тип формы для каждого маршрута. Следуя соглашениям Symfony3,  [forms types](http://symfony.com/doc/current/forms.html#building-the-form) разрешаются FQCN. Ниже вы можете увидеть, как указать настраиваемую форму.

```yaml
# config/routes.yaml

app_book_create:
    path: /books/new
    methods: [GET, POST]
    defaults:
        _controller: app.controller.book:createAction
        _sylius:
            form: App\Form\BookType
```
## Передача пользовательских параметров в форму

Что происходит, когда вам нужно передать в форму некоторые параметры? 
Что ж, для этого есть конфигурация!

Ниже вы можете увидеть использование для указания настраиваемых параметров в этом случае ```validation_groups```, но вы можете передать любой параметр, принятый формой.

```yaml
# config/routes.yaml

app_book_create:
    path: /books/new
    methods: [GET, POST]
    defaults:
        _controller: app.controller.book:createAction
        _sylius:
            form:
                type: App\Form\BookType
                options:
                    validation_groups: [sylius, my_custom_group]
```
## Использование собственного фабричного метода

По умолчанию ```ResourceController``` будет использовать ```createNew``` метод без аргументов для создания нового экземпляра вашего объекта. Однако это поведение можно изменить. Чтобы использовать другой метод вашей фабрики, вы можете просто настроить эту ```factory``` опцию.

```yaml
# config/routes.yaml

app_book_create:
    path: /books/new
    methods: [GET, POST]
    defaults:
        _controller: app.controller.book:createAction
        _sylius:
            factory: createNewWithAuthor
```
Кроме того, если вы хотите предоставить своему пользовательскому методу аргументы из запроса, вы можете сделать это, добавив дополнительные параметры.

```yaml
# config/routes.yaml

app_book_create:
    path: /books/{author}/new
    methods: [GET, POST]
    defaults:
        _controller: app.controller.book:createAction
        _sylius:
            factory:
                method: createNewWithAuthor
                arguments: [$author]
```
С этой конфигурацией, ``$factory->createNewWithAuthor($request->get('author'))`` будет вызываться для создания нового ресурса в ``createAction``.

## Использование кастомной фабричной службы

Если вы хотите использовать свой собственный сервис для создания ресурса, попробуйте следующую конфигурацию:

```yaml
# config/routes.yaml

app_book_create:
    path: /{authorId}/books/new
    methods: [GET, POST]
    defaults:
        _controller: app.controller.book:createAction
        _sylius:
            factory:
                method: ["expr:service('app.factory.custom_book_factory')", "createNewByAuthorId"]
                arguments: $authorId
```
В этой конфигурации будет вызываться служба с идентификатором app.factory.custom_book_factory для создания нового ресурса внутри ```createNewByAuthorId``` метода и идентификатора автора из URL-адреса в качестве аргумента.

## Пользовательское перенаправление после успеха

По умолчанию контроллер будет пытаться получить идентификатор вновь созданного ресурса и перенаправить его на «показать» маршрут.
Вы можете легко изменить это поведение. 
Например, для перенаправления в список индексов после успешного создания нового ресурса вы можете использовать следующую конфигурацию.

```yaml
# config/routes.yaml

app_book_create:
    path: /books/new
    methods: [GET, POST]
    defaults:
        _controller: app.controller.book:createAction
        _sylius:
            redirect: app_book_index
```
Вы также можете выполнять более сложные перенаправления с параметрами. Например:

```yaml
# config/routes.yaml

app_book_create:
    path: /genre/{genreId}/books/new
    methods: [GET, POST]
    defaults:
        _controller: app.controller.book:createAction
        _sylius:
            redirect:
                route: app_genre_show
                parameters: { id: $genreId }
```
Помимо параметров запроса, вы можете получить доступ к некоторым свойствам вновь созданных объектов, используя ``resource.`` префикс.

```yaml
# config/routes.yaml

app_book_create:
    path: /books/new
    methods: [GET, POST]
    defaults:
        _controller: app.controller.book:createAction
        _sylius:
            redirect:
                route: app_book_show
                parameters: { title: resource.title }
```
В этой конфигурации ```title``` параметр маршрута ```app_book_show``` будет получен из вашей вновь созданной книги.

## Свое имя для настраиваемого события

По умолчанию во время создания ресурса отправляются два события: одно перед его добавлением в базу данных, другое - после успешного добавления. Шаблон всегда один и тот же ```- {applicationName}.{resourceName}.pre/post_create```. Однако вы можете настроить последнюю часть события, чтобы указать собственное имя действия.

```yaml
# config/routes.yaml

app_book_customer_create:
    path: /customer/books/new
    methods: [GET, POST]
    defaults:
        _controller: app.controller.book:createAction
        _sylius:
            event: customer_create
```
Таким образом, вы можете слушать ```app.book.pre_customer_create``` и a```pp.book.post_customer_create```  события. Это особенно полезно, когда вы используете ```ResourceController:createAction``` более одного маршрута.

## Справочник по конфигурации

```yaml
# config/routes.yaml

app_genre_book_add:
    path: /{genreName}/books/add
    methods: [GET, POST]
    defaults:
        _controller: app.controller.book:createAction
        _sylius:
            template: Book/addToGenre.html.twig
            form: App\Form\BookType
            event: book_create
            factory:
                method: createForGenre
                arguments: [$genreName]
            criteria:
                group.name: $genreName
            redirect:
                route: app_book_show
                parameters: { title: resource.title }
```

Помните, что вы можете использовать полностью определенное имя класса контроллера ```( App\Controller\BookController)``` вместо ```id.app.controller.book```

**[Вернитесь к началу документации](index.md)**
