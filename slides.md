---
title: '**Communcication Our APIs:** Enhance Provider and Consumer Interaction'
layout: center
theme: seriph
---

<img src="/OpenValue Square.jpg" class="m-5 h-70 p-2 rounded bg-white" />

---
layout: intro
---

## **Communicating Our APIs:**
## Enhance Provider and Consumer Interaction

<br>

Enis Spahi (@enisspahi)

---
clicks: 1
---

# APIs

<div grid="~ cols-2 gap-10">

<div>

<br>
<br>
<br>

## "An application programming interface (API) is a way for two or more computer programs to communicate with each other."​

Source: Wikipedia, "API"​

</div>

<div>

<br>
<br>
<br>

![Local Image](/api-client-server.drawio.png)

<arrow v-click="1" x1="550" y1="370" x2="550" y2="290" color="#564" width="2" arrowSize="1" />
<arrow v-click="1" x1="900" y1="370" x2="900" y2="290" color="#564" width="2" arrowSize="1" />
<br>
<br>
<div grid="~ cols-2">

<div v-click="1" color="#564">

**Consumer**

</div>

<div v-click="1" class="right-0 flex justify-end"  color="#564">

**Provider**

</div>

</div>



</div>

</div>

---

# Communication Boundaries

<br>

- Private​ APIs
  - Provider and consumer are developers in the same team or same organisation​

<br>

- Partner Facing​ APIs
  - Serving partners (i.e.: Payment Service Providers)​
  - Provider and consumer might not communicate directly

<br>

- Public APIs
  - Publicly available (i.e.. Geo-Location services)​
  - Communication at scale: Many Consumers ↔ 1 Provider

---

# Communication Challenges

Top 3 Obstacles to consuming APIs


<div grid="~ cols-2 gap-4">

<div>

<br>

<br>

- Lack of documentation​
- Difficulty in discovering APIs​
- Lack of time​
</div>

<div>

![Obstacles](/postman_soar_obstacles.png)
Source: Postman, "2023 State of the API Report"​​​
</div>


</div>

---
layout: center
---

# Enhancing API Discoverability

<div class="flex justify-center items-center h-full">
<img src="/ibet.png" class="m-5 h-80 p-2 rounded bg-white" />
</div>


---

# API Specifications

<div grid="~ cols-2 gap-10">

<div>

<img src="/api-human2human.drawio.png" class="m-5 h-80 p-2 rounded bg-white" />

</div>

<div>

<br>


- Specification language for the APIs
  - OpenAPI/Swagger for REST APIs
  - AsyncAPI for message-driven APIs
  - WSDL for SOAP Web Services
  - GraphQL Schema for GraphQL
  - GRPC
- Standard for API discovery
- Foundation for documentation

</div>

</div>

<!-- API Spec define the functions and the expected results of an API -->

---



# OpenAPI Specification

<br>
<br>

- Technology agnostic standard to describe Rest APIs
- Formerly Swagger, OpenAPI as of version 3
- Written as JSON or YAML
- Great tooling for code and documentation generation
- [https://openapi.tools/](https://openapi.tools/)

---
clicks: 5
---

# OpenAPI Specification

<div grid="~ cols-2 gap-4">

<div>

```yaml {all|1|2-4|5-6|7-22|23-}  {maxHeight:'400px'} 
openapi: 3.0.3
info:
  title: Recipes API
  ...
servers:
- url: http://localhost:8080
paths:
  /recipes:
    get:
      summary: List all recipes
      ...
      responses:
        ...
        "200":
          description: OK
          content:
            'application/json':
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Recipe'
        ...
components:
  schemas:
    Recipe:
      type: object
      ...
```
</div>

<div class="p-3">

<v-clicks at="0">

- **Openapi:** Spec Version
- **Info:** General API information as metadata
- **Servers:** Connectivity information about target servers
- **Paths:** Paths to the endpoints with their expected request, response and errors. 
- **Components:** Holds the schemas for the request, response and errors for referencing

</v-clicks>

</div>

</div>


---

# AsyncAPI Specification

<br>
<br>

- Technology agnostic standard to describe message-driven APIs​
- An adaptation of the OpenAPI specification
- Written as JSON or YAML
- Protocols: AMQP, HTTP, JMS, Kafka, but not only
- [https://www.asyncapi.com/tools](https://www.asyncapi.com/tools)


---
clicks: 5
---

# AsyncAPI Specification

<div grid="~ cols-2 gap-4">

<div>

```yaml {all|1|2-4|6-10|12-24|26-}  {maxHeight:'400px'} 
asyncapi: 2.0.0
info:
  title: Ping Service
  ...

servers:
  localhost:
    url: localhost:9092
    protocol: kafka
    protocolVersion: '1.0.0'

channels:
  ping.topic:
    description: Kafka topic for ping messages
    publish:
      operationId: pingSent
      message:
        $ref : '#/components/messages/Ping'
  pong.topic:
    description: Kafka topic for pong messages
    subscribe:
      operationId: pongReceived
      message:
        $ref : '#/components/messages/Pong'      

components:
  messages:
    Ping:
      name: Ping
      ...
      payload:
        $ref: '#/components/schemas/PingPayload'
    Pong:
      name: Pong
      ...
      payload:
        $ref: '#/components/schemas/PongPayload'

  schemas:
    PingPayload:
      type: object
      ...
    PongPayload:
      type: object       
      ...
```
</div>

<div class="p-3">

<v-clicks at="0">

- **Asyncapi:** Spec Version
- **Info:** Metadata information about the API
- **Servers:** Connectivity information about servers (i.e. Kafka brokers)
- **Channels:** Messages exchange between provider and consumer
- **Components:** Defines the reusable objects such as schemas or messages which could be referenced.

</v-clicks>

</div>

</div>


---
layout: center
---

<div class="flex justify-center items-center h-full">

# Enhancing API development
</div>

<div class="flex justify-center items-center h-full">
<img src="/nicolascage_pedropascal.gif" class="m-5 h-70 p-2 rounded bg-white" />
</div>


---

# Recipes API

Let's build an API

<div grid="~ cols-2 gap-2" m="-t-2">

<div>

#### Client

</div>

<div>

#### Server

</div>

<div class="max-h-87 overflow-auto">

<img src="/client-ui.png" class="h-60 rounded border-2 border-gray-400" />

<img src="/client-ui-response.png" class="h-130 rounded border-2 border-gray-400" />

</div>

<div>


```shell
curl 'http://localhost:8080/recipes?title=Pumpkin&nutritionFacts=LOW_CALORIE'
```

```json {all} {maxHeight:'300px'} 
[
    {
        "title": "Pumpkin Soup",
        "ingredients": [
            {
                "name": "Pumpkin",
                "quantity": 1000.0,
                "unit": "grams"
            },
            {
                "name": "Onion",
                "quantity": 1.0,
                "unit": "unit"
            },
            {
                "name": "Vegetable broth",
                "quantity": 750.0,
                "unit": "ml"
            },
            {
                "name": "Cream",
                "quantity": 100.0,
                "unit": "ml"
            },
            {
                "name": "Nutmeg",
                "quantity": 1.0,
                "unit": "gram"
            },
            {
                "name": "Salt",
                "quantity": 1.0,
                "unit": "tablespoon"
            }
        ],
        "preparationTime": 15,
        "cookingTime": 30,
        "servings": 4,
        "instructions": [
            "Sauté onion until translucent.",
            "Add pumpkin cubes, vegetable broth, and salt, bring to a boil.",
            "Simmer until pumpkin is tender.",
            "Blend until smooth, stir in cream and nutmeg.",
            "Heat through and serve."
        ],
        "nutritionFacts": [
            "LOW_CALORIE",
            "CARBS"
        ]
    }
]
```

</div>


</div>



---

# API Development - Code First

Communicate API specification once coding has been done

<div grid="~ cols-2 gap-10">

<div>

<div v-click="1">

- **Advantages:**
  - Focus on coding
  - Flexibility to change the API design

</div>

<div v-click="2">

- **Disadvantages:**
  - Late communication with the consumer
  - Does not enable development in parallel
  - Annotations
</div>

</div>

<div>

<img src="/api-code-first.drawio.png" class="p-2 rounded bg-white" />

</div>

</div>

---

# API Development - API First

Communicate API specification before coding. Prioritizes API design over implementation.

<div grid="~ cols-2 gap-10">

<div>

<div v-click="1">

- **Advantages:**
  - Early communication with the consumer
  - Documentation thought ahead
  - Enables development in parallel

</div>

<div v-click="2">

- **Disadvantages:**
  - Less flexibility to change the API design
  - Sometimes bureaucratic for providers
</div>

</div>

<div>

<img src="/api-api-first.drawio.png" class="p-2 rounded bg-white" />

</div>

</div>

---

# API Development - Consumer First

Consumer dictates the expected API behavior to the provider

<div grid="~ cols-2 gap-10">

<div>

<div v-click="1">

**Pact:** A Code-first consumer contract testing tool that enables consumer driven API development.

</div>


<div v-click="2">

**Process:**
- Consumer produces a pact
- Provider verifies it's API implementation
- Server / Client deployments synced

</div>

</div>

<div>

<img src="/api-consumer-contracts.drawio.png" class="p-2 rounded bg-white" />

</div>

</div>

---

# API Development

|     |     |
| --- | --- |
| When to use Code first? | Provider initially focuses on coding speed <br> Flexibility to change the design |
| When to use API first? | API design over implementation <br> Early communication with the consumer → Documentation <br> Utilize code generation <br> Large number of consumers |
| When to use Consumer first? | Provider should conform to consumer needs <br> API consumer and provider test their applications independently <br> To sync provider and consumer deployments <br> Small number of consumers |
| When to mix & match? | When API first alone is not sufficient to match consumer needs |

---
layout: center
---

# Enhancing API Documentation

<div class="flex justify-center items-center h-full">
<img src="/anakin_padme.png" class="m-5 h-80 p-2 rounded bg-white" />
</div>

---

# API Documentation

API Specifications can be leveraged to generate more human readable forms of documentation

<div grid="~ cols-2 gap-2" m="-t-2">

<div>

#### API Reference

</div>

<div>

#### API Documentation

</div>

<div>

<img src="/swagger-ui.png" class="h-50 rounded border-2 border-gray-300" />

</div>

<div>

<img src="/api-documentation.png" class="h-50 rounded border-2 border-gray-300" />

</div>


<div>

<Transform :scale="0.9">

- APIs described as web pages
- Code samples, try-it-out
- Auto generated
- **Swagger-ui:** The most popular

</Transform>

</div>

<div>

<Transform :scale="0.9">
  
- API Reference incorporated in a bigger ecosystem
- Conceptual technical documentation
- Docs as code: Markdown, Technical Writing
- Continuous documentation
- [Demo](https://enisspahi.github.io/contract-first-api-example/)

</Transform>

</div>


</div>


---

# Summary

|     |     |
| --- | --- |
| Enhancing API Discoverability | Standardized language, common understanding <br> <ul><li>OpenAPI, AsyncAPI, ...</li></ul>  |
| Enhancing API development | API Development methodology <br> <ul><li>Code first, API first, Consumer first, mix & match</li></ul>  Speed up development <br> <ul><li>OpenAPI Generator, Pact, Swagger-validator</li></ul> |
| Enhancing API Documentation | API Specification → API Reference → API Docs <br> Stay up-to-date with Continuous Documentation |

---
layout: statement
---

## **OpenValue Blog**

[https://openvalue.blog/posts/2023/11/25/communicating_our_apis_part1/](https://openvalue.blog/posts/2023/11/25/communicating_our_apis_part1/)

[https://openvalue.blog/posts/2023/11/26/communicating_our_apis_part2/](https://openvalue.blog/posts/2023/11/26/communicating_our_apis_part2/)

## **Code Samples**

[https://github.com/enisspahi/code-first-api-example](https://github.com/enisspahi/code-first-api-example)

[https://github.com/enisspahi/contract-first-api-example](https://github.com/enisspahi/contract-first-api-example)

[https://github.com/enisspahi/consumer-first-api-example](https://github.com/enisspahi/consumer-first-api-example)

[https://github.com/enisspahi/async-api-example](https://github.com/enisspahi/async-api-example)


---
layout: statement
---

# Q&A
