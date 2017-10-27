#Swagger

####💥Sections
A typical swagger file is composed of three general sections:
- API General Info - Configuration
- Paths
- Definitions

####💥API General Info - configuration

This section provides general information about your API and
how it is configured within the HTTP protocol. Version number, license, and overall descriptions of the API is housed within this section.

These are the required fields for Swagger to work:

```
swagger: '2.0'
info:
  title:
  description:
host:
basePath:
schemes:
  - http
  - https
consumes:
  - application/json
produces:
  - application/json
  ```
- info ==> we can add arbitrary keys/values
- host ==> host where API lives
- basepath ==> the path where Swagger will hook into
- schemes, consumes, produces ==> HTTP configurations

#####💥Definitions
In this section of our YAML file, we can specify schema definitions
so that we do not clutter the paths section with this information; we can just reference them.

We reference as schema defintion using the `$ref` property.
We prefix the name of our defintion with this text: `'#/definitions/'`
Below the Path section, we open up a definition section and actually
provide schema information.

>DEFINITION EXAMPLE
```
schema:
  $ref: '#/definitions/errorModel'
...
...
definitions:
  errorModel:
    type: object
    required:
      - code
      - message
    properties:
      code:
        type: integer
        format: int32
      message:
        type: string
```

####💥Schema Types

👉 Strings
```
schema:
  type: string
```

👉 Numbers
```
schema:
  type: integer
  format: int64
```

👉 Arrays
```
schema:
  type: array
  items:
    $ref: '#/definitions/some-object'
```

👉 Objects
```
schema:
  type: object
  required:
    - id
  properties:
    id:
      type: integer
      format: int64
    name:
      type: string
  ```


####💥Paths
In this section, we specify the routes our API accepts, which methods it supports. We must provide schema definitions for the API data.

>GET REQUEST EXAMPLE
```
/routefoo:
  get:
    description:
    produces:
    responses:
      '200':
        description:
        schema:
          type: array
          items:
            $ref: '#/definitions/some-object'
      default:
        description: unexpected error
        schema:
          $ref: '#/definitions/errorModel'

/routefoo/{id}:
    get:
      description:
      produces:
      parameters:
        - name:
          in:
          description:
          required: true
          type: integer
          format: int64
      responses:
        '200':
          description:
          schema:
            $ref: '#/definitions/some-object'
        default:
          description: unexpected error
          schema:
            $ref: '#/definitions/errorModel'
```


> POST EXAMPLE
```
/routesfoo:
    post:
        description:
        produces:
        parameters:
          - name:
            in: body
            description:
            required: true
            schema:
              $ref: '#/definitions/newObj'
        responses:
          '200':
            description:
            schema:
              $ref: '#/definitions/some-object'
          default:
            description: unexpected error
            schema:
              $ref: '#/definitions/errorModel'
```

###Incorporating Swagger into Existing Project

- Create a YAML file in the public directory
  - `public/swagger.yaml`
- Follow proper YAML syntax to write out Swagger file
- Once you’ve documented your API endpoints, check your schema using the Swagger online editor:
  > https://editor.swagger.io/

- Make necessary corrections using the online editor and update your swagger file in the public directory of your project.
- Download Swagger-UI zip-file from
  > https://github.com/swagger-api/swagger-ui

- Open Project directory
  - on root level, open swagger-config.YAML
- Highlight the url, find-replace-all with the url to your swagger file (that lives in public/)

  ```
  url: "http://petstore.swagger.io/v2/swagger.json" --> url: "http://localhost:8000/swagger.yaml"
  ```
- Rebuild Swagger-UI project and start server

- Gotchas:
  - Make sure you allow **cors** (in your project) when working localhost
  - make sure you update the url to point to the swagger file, not just the root URL.
