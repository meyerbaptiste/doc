# Configuration

## Symfony bundle

Here's the complete configuration of the Symfony bundle with including default values:

```yaml
# app/config/config.yml

api_platform:

    # The title of the API.
    title: ''

    # The description of the API.
    description: ''

    # The version of the API.
    version: '0.0.0'

    # Specify the default operation path resolver to use for generating resources operations path.
    default_operation_path_resolver: 'api_platform.operation_path_resolver.underscore'

    # Specify a name converter to use.
    name_converter: ~

    eager_loading:
        # To enable or disable eager loading.
        enabled: true

        # Max number of joined relations before EagerLoading throws a RuntimeException.
        max_joins: 30

        # Force join on every relation. If disabled, it will only join relations having the EAGER fetch mode.
        force_eager: true

    # Enable the FOSUserBundle integration.
    enable_fos_user: false

    # Enable the Nelmio Api doc integration.
    enable_nelmio_api_doc: false

    # Enable the Swagger documentation and export.
    enable_swagger: true

    # Enable Swagger ui.
    enable_swagger_ui: true

    collection:
        # The default order of results.
        order: ~

        # The name of the query parameter to order results.
        order_parameter_name: 'order'

        pagination:
            # To enable or disable pagination for all resource collections by default.
            enabled: true

            # To allow the client to enable or disable the pagination.
            client_enabled: false

            # To allow the client to set the number of items per page.
            client_items_per_page: false

            # The default number of items per page.
            items_per_page: 30

            # The maximum number of items per page.
            maximum_items_per_page: ~

            # The default name of the parameter handling the page number.
            page_parameter_name: 'page'

            # The name of the query parameter to enable or disable pagination.
            enabled_parameter_name: 'pagination'

            # The name of the query parameter to set the number of items per page.
            items_per_page_parameter_name: 'itemsPerPage'

    # The list of exceptions mapped to their HTTP status code.
    exception_to_status:
        # With a status code.
        Symfony\Component\Serializer\Exception\ExceptionInterface: 400

        # Or with a constant defined in the 'Symfony\Component\HttpFoundation\Response' class.
        ApiPlatform\Core\Exception\InvalidArgumentException: 'HTTP_BAD_REQUEST'

        # ...

    # The list of enabled formats. The first one will be the default.
    formats:
        jsonld:
            mime_types: ['application/ld+json']

        json:
            mime_types: ['application/json']

        html:
            mime_types: ['text/html']

        # ...

    # The list of enabled error formats. The first one will be the default.
    error_formats:
        jsonproblem:
            mime_types: ['application/problem+json']
            
        jsonld:
            mime_types: ['application/ld+json']
            
        # ...
```

## Resources and properties

Resources and their properties can be configured in many ways. Let's focus on annotations, XML and YAML files with the
same example for each of them. This one contains all possible attributes you may encounter.

### Annotations

Two annotations are provided by API Platform:
* `@ApiResource` which can be imported from the `ApiPlatform\Core\Annotation\ApiResource` class: configure a resource at
class level;
* `@ApiProperty` which can be imported from the `ApiPlatform\Core\Annotation\ApiProperty` class: configure a resource's
property at property and method level.

```php
<?php
// src/AppBundle/Entity/Foo.php

namespace AppBundle\Entity;

use ApiPlatform\Core\Annotation\ApiResource;
use ApiPlatform\Core\Annotation\ApiProperty;

/**
 * @ApiResource(
 *     shortName="Foo",
 *     description="A foo resource.",
 *     iri="http://schema.org/Foo",
 *     itemOperations={
 *         "foo"={
 *             "method"="GET",
 *             "route_name="foo_item_route",
 *             "path"="/foo/{bar}",
 *             "controller"="app.action.foo_item",
 *             "normalization_context"={ 
 *                 "groups"={"foo", "bar"},
 *                 "foo"="bar"
 *             },
 *             "denormalization_context={
 *                 "groups"={"foo", "bar"},
 *                 "foo"="bar"
 *             },
 *             "hydra_context"={
 *                 "foo"="bar"
 *             },
 *             "swagger_context"={
 *                 "foo"="bar"
 *             },
 *             "validation_groups"={"foo", "bar"},
 *             "force_eager"=false,
 *             "foo"="bar"
 *         }    
 *     },
 *     collectionOperations={
 *         "foo"={
 *             "method"="GET",
 *             "route_name"="foo_collection_route",
 *             "path"="/foo/baz",
 *             "controller"="app.action.foo_collection",
 *             "normalization_context"={
 *                 "groups"={"foo", "bar"},
 *                 "foo"="bar"
 *             },
 *             "denormalization_context"={
 *                 "groups"={"foo", "bar"},
 *                 "foo"="bar"
 *             },
 *             "hydra_context"={
 *                 "foo"="bar"
 *             },
 *             "swagger_context"={
 *                 "foo"="bar"
 *             },
 *             "validation_groups"={"foo", "bar"},
 *             "force_eager"=true,
 *             "filters"={"foo.search", "foo.boolean"},
 *             "pagination_enabled"=true,
 *             "pagination_items_per_page"=30,
 *             "pagination_client_enabled"=true,
 *             "pagination_client_items_per_page"=30,
 *             "foo"="bar"
 *         }
 *     },
 *     attributes={
 *         "force_eager"=false,
 *         "filters"={"foo.date"},
 *         "normalization_context"={ 
 *             "groups"={"foo", "bar"},
 *             "foo"="bar",
 *         },
 *         "denormalization_context"={
 *             "groups"={"foo", "bar"},
 *             "foo"="bar"
 *         },
 *         "pagination_enabled"=true,
 *         "pagination_items_per_page"=30,
 *         "pagination_client_enabled"=true,
 *         "pagination_client_items_per_page"=30,
 *         "foo"="bar"
 *     }
 * }
 */
class Foo
{
    /**
     * @ApiProperty(
     *     description="A bar property.",
     *     iri="http://schema.org/Bar",
     *     readable=true,
     *     writable=true,
     *     readableLink=false,
     *     writableLink=false,
     *     required=true,
     *     identifier=false,
     *     attributes={
     *         "jsonld_context"={
     *             "foo"="bar"
     *         },
     *         "foo"="bar"      
     *     }
     * )
     */
    public $bar;
}
```

N.B. If you use the Symfony bundle, all entities in your `Entity/` directory of your bundle directories (e.g.
`src/AppBundle/Entity/`) will be automatically loaded and parsed.

### YAML

```yaml
# src/AppBundle/Resources/config/api_resources/foo.yml

resources:
    AppBundle\Entity\Foo:
        shortName: 'Foo'
        description: 'A foo resource.'
        iri: 'http://schema.org/Foo'
        itemOperations:
            foo:
                method: 'GET'
                route_name: 'foo_item_route'
                path: '/foo/{bar}'
                controller: 'app.action.foo_item'
                normalization_context:
                    groups: ['foo', 'bar']
                    foo: 'bar'
                denormalization_context:
                    groups: ['foo', 'bar']
                    foo: 'bar'
                hydra_context:
                    foo: 'bar'
                swagger_context:
                    foo: 'bar'
                validation_groups: ['foo', 'bar']
                force_eager: false
                foo: 'bar'
        collectionOperations:
            foo:
                method: 'GET'
                route_name: 'foo_collection_route'
                path: '/foo/baz'
                controller: 'app.action.foo_collection'
                normalization_context:
                    groups: ['foo', 'bar']
                    foo: 'bar'
                denormalization_context:
                    groups: ['foo', 'bar']
                    foo: 'bar'
                hydra_context:
                    foo: 'bar'
                swagger_context:
                    foo: 'bar'
                validation_groups: ['foo', 'bar']
                force_eager: true
                filters: ['foo.search', 'foo.boolean']
                pagination_enabled: true
                pagination_items_per_page: 30
                pagination_client_enabled: true
                pagination_client_items_per_page: 30
                foo: 'bar'
        attributes:
            force_eager: false
            filters: ['foo.date']
            normalization_context:
                groups: ['foo', 'bar']
                foo: 'bar'
            denormalization_context:
                groups: ['foo', 'bar']
                foo: 'bar'
            pagination_enabled: true
            pagination_items_per_page: 30
            pagination_client_enabled: true
            pagination_client_items_per_page: 30
            foo: 'bar'
        properties:
            bar:
                description: 'A bar property.'
                iri: 'http://schema.org/Bar'
                readable: true
                writable: true
                readableLink: false
                writableLink: false
                required: true
                identifier: false
                attributes:
                    jsonld_context:
                        foo: 'bar'
                    foo: 'bar'
```

N.B. If you use the Symfony bundle, your YAML config files must be located in the `Resources/config/api_resources/`
directory of your bundle directory (e.g. `src/AppBundle/Resources/config/api_resources/`) to be automatically loaded and
parsed.

### XML

```xml
<!-- src/AppBundle/Resources/config/api_resources/foo.xml -->

<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <resource
        class="AppBundle\Entity\Foo"
        shortName="Foo"
        description="A foo resource."
        iri="http://schema.org/Foo"
    >
        <itemOperation name="foo">
            <attribute name="method">GET</attribute>
            <attribute name="route_name">foo_item_route</attribute>
            <attribute name="path">/foo/{bar}</attribute>
            <attribute name="controller">app.action.foo_item</attribute>
            <attribute name="normalization_context">
                <attribute name="groups">
                    <attribute>foo</attribute>
                    <attribute>bar</attribute>
                </attribute>
                <attribute name="foo">bar</attribute>
            </attribute>
            <attribute name="denormalization_context">
                <attribute name="groups">
                    <attribute>foo</attribute>
                    <attribute>bar</attribute>
                </attribute>
                <attribute name="foo">bar</attribute>
            </attribute>
            <attribute name="hydra_context">
                <attribute name="foo">bar</attribute>
            </attribute>
            <attribute name="swagger_context">
                <attribute name="foo">bar</attribute>
            </attribute>
            <attribute name="validation_groups">
                <attribute>foo</attribute>
                <attribute>bar</attribute>
            </attribute>
            <attribute name="force_eager">false</attribute>
            <attribute name="foo">bar</attribute>
        </itemOperation>
        <collectionOperation name="foo">
            <attribute name="method">GET</attribute>
            <attribute name="route_name">foo_collection_route</attribute>
            <attribute name="path">/foo/baz</attribute>
            <attribute name="controller">app.action.foo_collection</attribute>
            <attribute name="normalization_context">
                <attribute name="groups">
                    <attribute>foo</attribute>
                    <attribute>bar</attribute>
                </attribute>
                <attribute name="foo">bar</attribute>
            </attribute>
            <attribute name="denormalization_context">
                <attribute name="groups">
                    <attribute>foo</attribute>
                    <attribute>bar</attribute>
                </attribute>
                <attribute name="foo">bar</attribute>
            </attribute>
            <attribute name="hydra_context">
                <attribute name="foo">bar</attribute>
            </attribute>
            <attribute name="swagger_context">
                <attribute name="foo">bar</attribute>
            </attribute>
            <attribute name="validation_groups">
                <attribute>foo</attribute>
                <attribute>bar</attribute>
            </attribute>
            <attribute name="force_eager">true</attribute>
            <attribute name="filters">
                <attribute>foo.search</attribute>
                <attribute>foo.boolean</attribute>
            </attribute>
            <attribute name="pagination_enabled">true</attribute>
            <attribute name="pagination_items_per_page">30</attribute>
            <attribute name="pagination_client_enabled">true</attribute>
            <attribute name="pagination_client_items_per_page">30</attribute>
            <attribute name="foo">bar</attribute>
        </collectionOperation>
        <attribute name="force_eager">false</attribute>
        <attribute name="filters">
            <attribute>foo.date</attribute>
        </attribute>
        <attribute name="normalization_context">
            <attribute name="groups">
                <attribute>foo</attribute>
                <attribute>bar</attribute>
            </attribute>
            <attribute name="foo">bar</attribute>
        </attribute>
        <attribute name="denormalization_context">
            <attribute name="groups">
                <attribute>foo</attribute>
                <attribute>bar</attribute>
            </attribute>
            <attribute name="foo">bar</attribute>
        </attribute>
        <attribute name="pagination_enabled">true</attribute>
        <attribute name="pagination_items_per_page">30</attribute>
        <attribute name="pagination_client_enabled">true</attribute>
        <attribute name="pagination_client_items_per_page">30</attribute>
        <attribute name="foo">bar</attribute>
        <property
            name="bar"
            description="A bar property."
            iri="http://schema.org/Bar"
            readable="true"
            writable="true"
            readableLink="false"
            writableLink="false"
            required="true"
            identifier="false"
        >
            <attribute name="jsonld_context">
                <attribute name="foo">bar</attribute>
            </attribute>
            <attribute name="foo">bar</attribute>
        </property>
    </resource>
</resources>
```

N.B. If you use the Symfony bundle, your XML config files must be located in the `Resources/config/api_resources/`
directory of your bundle directory (e.g. `src/AppBundle/Resources/config/api_resources/`) to be automatically loaded and
parsed.

Previous chapter: [Getting Started](getting-started.md)

Next chapter: [Operations](operations.md)
