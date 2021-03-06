## API Spec and the implications

### operationId

```yaml
paths:
  /super/long/unintutive/path:
    post:
      operationId: newPet
      requestBody:
        required: true
        content:
          application/json:
            schema:
              \$ref: '#/components/schemas/PostPetsRequest'
```

if the *operationId* is defined, it will be used to generate the endpoint name that is the folder of the generated code.
For the given example the folder will be `PostNewPet`. So the Verb and the *operationId* are combined. 
The *operationId* will overrule the path.

### allOF

```yaml
components:
  schemas:
    PostPetsRequest:
      type: object
      properties:
        name:
          type: string
        listOfAandB:
          type: array
          items:
            allOf:
              - $ref: '#/components/schemas/TypeA'
              - $ref: '#/components/schemas/TypeB'
```

the array `listOfAandB` will lead to a DTO `ListOfAandBDto` that is the collection. The collection will hold items of type `ListOfAandBItemDto` that is because the type is not unique rather it is a composed type out of `TypeA` and `TypeB` that builds the Item data type.

### default

```yaml
components:
  schemas:
    NewPet:
      required:
      - name
      properties:
        name:
          type: string
        tag:
          type: string
          default: "someType"
```

the `default` value `"someType"` will be assign to the `NewPetDto::tag` in case the field is is absent in the payload. 

### nullable

```yaml
components:
  schemas:
    NewPet:
      required:
      - name
      properties:
        name:
          type: string
        tag:
          type: string
          nullable: true
```

`null` is allowed to be assign to the `NewPetDto::tag`. When the field in the payload is null then the further checks about data type and minimum will be skipped. Note that this can be combined with `default: null` to initialize the value. 