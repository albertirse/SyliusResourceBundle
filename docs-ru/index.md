# SyliusResourceBundle

Есть много вещей, которые вам нужно обработать для каждого отдельного ресурса в вашем веб-приложении.

Для Symfony доступно несколько «генераторов администратора», но нам нужно было что-то действительно простое, что позволило бы нам иметь многоразовые контроллеры, но сохранило бы производительность и стандартный рабочий процесс Symfony. Мы не хотели генерировать какой-либо код или писать определения классов «Admin» на PHP. Большая цель заключалась в том, чтобы иметь точно такой же рабочий процесс, как при написании контроллеров вручную, но без их фактического создания!

Другая идея заключалась в том, чтобы не ограничиваться одним бэкендом с постоянством. ```Resource``` Компонент предоставляет нам универсальные службы сохраняемости, и вы можете использовать этот пакет с несколькими бэкэндами сохраняемости. Пока мы поддерживаем:

* Doctrine ORM
* Doctrine MongoDB ODM
* Doctrine PHPCR ODM

## Resource system for Symfony applications.

* [Установка](installation.md)
* [Настройка ваших ресурсов](configuration.md)
* [Services](services.md)
* [Routing](routing.md)
* [Forms](forms.md)
* [Получение одного ресурса](show_resource.md)
* [Получение коллекции ресурсов](index_resources.md)
* [Создание ресурсов](create_resource.md)
* [Обновление ресурсов](update_resource.md)
* [Удаление ресурсов](delete_resource.md)
* [Справочник по конфигурации](reference.md)

## Learn more

* [Resource Layer in the Sylius platform](https://docs.sylius.com/en/latest/book/architecture/resource_layer.html) - concept documentation
