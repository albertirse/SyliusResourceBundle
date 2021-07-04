# Configuring Your Resources

Теперь вам нужно настроить свой первый ресурс. Предположим, в вашем приложении есть сущность Book с простыми полями:

* id
* title
* author
* description

Вы можете увидеть полную примерную конфигурацию типичного ресурса.
[Как добавить кастомную модель?](https://docs.sylius.com/en/latest/cookbook/entities/custom-model.html)

## Реализуйте ResourceInterface в своем классе модели.

```php
namespace App\Entity;

use Sylius\Component\Resource\Model\ResourceInterface;

class Book implements ResourceInterface
{
    // Most of the time you have the code below already in your class.
    protected $id;

    public function getId()
    {
        return $this->id;
    }
}
```

## Настройте класс как ресурс.

In your ``config/packages/sylius_resource.yaml`` add:

```yaml
sylius_resource:
    resources:
        app.book:
            classes:
                model: App\Entity\Book
```
Готово! Ваша книга теперь зарегистрирована как Sylius Resource.

## Ваша книга теперь зарегистрирована как Sylius Resource.

Помните, что ``doctrine/orm`` драйвер используется по умолчанию.

```yaml
sylius_resource:
    drivers:
        - doctrine/orm
        - doctrine/phpcr-odm
    resources:
        app.book:
            classes:
                model: App\Entity\Book
        app.article:
            driver: doctrine/phpcr-odm
            classes:
                model: App\Document\ArticleDocument
```
## Сгенерируйте маршрутизацию API.

Узнайте больше об использовании Sylius REST API в этих статьях:
[How to use Sylius API? - Cookbook](https://docs.sylius.com/en/latest/cookbook/api/api.html)

Добавьте следующие строки в ``config/routes.yaml``:

```yaml
app_book:
    resource: |
        alias: app.book
    type: sylius.resource_api
```
После этого полный JSON / XML CRUD API готов к использованию. 
Звучит безумно? Запустите встроенный сервер и попробуйте:

```bash
php bin/console server:run
```
Вы должны увидеть что-то вроде:

```bash
Server running on http://127.0.0.1:8000

Quit the server with CONTROL-C.
```
Теперь в отдельном окне Терминала вызовите эти команды:

```bash
curl -i -X POST -H "Content-Type: application/json" -d '{"title": "Lord of The Rings", "author": "J. R. R. Tolkien", "description": "Amazing!"}' http://localhost:8000/books/
curl -i -X GET -H "Accept: application/json" http://localhost:8000/books/
```
Как вы можете догадаться, другие действия CRUD доступны через этот API.

## Создание веб-маршрутизации.

Что делать, если вы хотите отображать HTML-страницы? Это просто! Обновите конфигурацию маршрутизации:

```yaml
app_book:
    resource: |
        alias: app.book
    type: sylius.resource
```
Это сгенерирует маршрутизацию для представлений HTML.

Запустите ``debug:router`` команду, чтобы увидеть доступные маршруты:

```bash
php bin/console debug:router
```
```
------------------------ --------------- -------- ------ -------------------------
Name                     Method          Scheme   Host   Path
------------------------ --------------- -------- ------ -------------------------
app_book_show            GET             ANY      ANY    /books/{id}
app_book_index           GET             ANY      ANY    /books/
app_book_create          GET|POST        ANY      ANY    /books/new
app_book_update          GET|PUT|PATCH   ANY      ANY    /books/{id}/edit
app_book_delete          DELETE          ANY      ANY    /books/{id}
```

Вам нужны **представления** для вашей вновь созданной сущности? Узнайте больше о
[Grids](https://docs.sylius.com/en/latest/components_and_bundles/bundles/SyliusGridBundle/index.html),
которые являются отдельным пакетом Sylius, но могут быть очень полезны для генерации представлений.


##
Вы можете настроить больше параметров для генерации маршрутизации, но вы также можете определить каждый маршрут вручную, чтобы полностью настроить его. Продолжайте читать, чтобы узнать больше!

**[Вернитесь к началу документации](index.md)**
