
# Enabling Discoverability and Navigability for Node.js APIs with HAL and JSON:API
As a backend developer focused on API design, at [Hybrid Web Agency](https://hybridwebagency.com/) I continually explore new techniques to strengthen my skills and knowledge. For a personal project, I decided to develop a hypermedia-driven Node.js API from the ground up using emerging standards.

My goal was to implement the HATEOAS style comprehensively following the HAL and JSON:API specifications through extensive testing. This hands-on experience would allow me to evaluate these approaches in a real-world context.

Over six months, I built out a full-featured order management system using hypermedia as its architectural foundation. Analyzing how each spec structured the relationships between order, product, and customer resources revealed insightful differences in their approaches.

In this article, I will discuss the key findings from my research and practical implementation. Code samples demonstrate modeling interconnected entities as hyperlinks.

By sharing learnings from this in-depth process, I aim to benefit other architects evaluating API design techniques. It also helps advance my expertise in crafting robust, evolvable services aligned with modern best practices.

Through continuous learning experiences like this personal project, I hone my abilities to deliver high-quality solutions for clients.








### What is HATEOAS?

HATEOAS defines an architectural approach for building RESTful APIs based on hypermedia. It describes how an API client can use links embedded in responses to dynamically interact with that API.

Rather than requiring clients to hardcode knowledge of endpoints and relationships, HATEOAS APIs provide the necessary information to propel the client through the API interface. Links contained within or pointed to by responses teach clients which actions or resources are valid next steps.

For example, returning an order resource may include hyperlinks to associated products, customers or invoices. This allows clients flexible navigation across resources as opposed to static sequencing. Changes to the API structure, such as adding new entities or relations, can be utilized by any client following links in responses.

Adopting HATEOAS separates the API interface from any specific client implementation. APIs following this style become inherently explorable through responses guiding request progress. As resources or links are augmented over time, pre-existing clients face minimal impact since they rely on hypermedia to understand valid transitions.

In essence, HATEOAS establishes APIs driven by the exchange of hyperlinked data rather than through external client configuration. This architectural tactic builds discoverability, evolvability and robustness by decoupling how clients apprehend an API interface from how that interface is constructed.


### Advantages of the HATEOAS Approach

By centering on hypermedia as the representation of application state, REST APIs that follow HATEOAS principles realize several key strengths:

**Self-Documenting Dialogs** - Each response cryptically outlines the valid transitions for the given context through embedded links, removing the need for out-of-band documentation.

**Organic Extensibility** - The service can unveil new capabilities and refactor internally over time through hyperlink augmentations, independently of clients.

**Resiliency to Change** - Modifying the domain model through structural changes becomes transparent to all consumers simply by updating relational prompts.

**Post-Deployment Discovery** - Hypermedia responses facilitate unveiling expanded functionality after launch through an inside-out growth methodology. 

**Exploratory Interfaces** - Clients can uncover overlooked interactions by dynamically traversing embedded affinity relationships within responses. 

**Reactive Traversal** - HATEOAS grants clients the freedom to prioritize navigational paths tailored to each unique operational situation.

Fundamentally, focusing on hypermedia as the driver produces self-educating, long-lasting and adaptable REST APIs able to flexibly evolve and guide their own comprehension.



## Choosing a Hypermedia Format

When deciding how to implement HATEOAS in a Node.js API, two leading specification options are Hypertext Application Language (HAL) and JSON:API.

### Hypertext Application Language 

HAL provides a simple format for representing hyperlinks between resources using a top-level "_links" property. For example:

```json
{
  "_links": {
    "self": { "href": "/orders/1" },
    "items": { "href": "/orders/1/items" }
  }
}
```

It is lightweight and intuitive for both machines and humans. However, additional context cannot be included.

### JSON:API

JSON:API defines a full JSON API specification including serialization of resources/relationships. Resources explicitly connect through a "relationships" object listing related IDs and links.

For example:

```
"relationships": {
  "items": {
    "links": {
      "related": "/orders/1/relationships/items"
    },
    "data": [
      {"id": "5", "type": "item"}, 
      {"id": "3", "type": "item"}
    ]
  }
} 
```

It provides a richer standard but at the cost of more complex implementation versus HAL.

### Choosing Between Them

HAL is best for simpler use cases needing only basic hypermedia support. JSON:API acts as a complete REST framework ideal for robust APIs. Consider project scope/needs to determine the right level of standardization. Both enable the HATEOAS approach.





## Developing a Hypermedia-Driven API in Node.js

This guide demonstrates how to create an API that leverages the Hypertext Application Language (HAL) specification using Express.

### Incorporating Necessary Libraries

The `express-hal-builder` module is installed to generate HAL-compliant resource representations and linked relationships.

```
npm install express-hal-builder
```

### Modeling API Resources

JavaScript object schemas are defined for Order and OrderItem resources. 

```js
const Order = {};
const OrderItem = {};
```

### Generating HAL Responses

The builder facilitates mapping entities to hypermedia by:

1. Representing a resource 

2. Embedding child links

```js
app.get('/orders/:id', (req, res) => {

  const order = //...

  const builder = new HalBuilder();

  builder.representEntity(order)
   .childLink(order.items, '/orders/'+id+'/items');

  res.json(builder.get());

});
```

This provides the framework to develop a RESTful API according to the HAL spec, connecting resources through hypermedia links.



## Consuming the HAL API

Making requests to explore the API interfaces and discover linked resources.

- Send GETs to endpoints to obtain resource representations  
- Parse responses to identify available relation types
- Follow embedded links by extracting URL values
- Dynamically build clients based on link exploration

## Developing a JSON:API in Node.js

This section outlines creating a JSON:API compliant backend using Express.

### Incorporating Dependencies 

The `jsonapi-serializer` module is used for response generation.

```
npm install jsonapi-serializer
```

### Modeling Core Resources

Schemas are defined for resources like Order and Item.

### Generating JSON:API Documents

Responses include related data and referenced links:

```js
app.get('/orders/:id', async (req, res) => {

  const order = await Order.findById(req.params.id);

  res.json(order.serialize({
    include: 'items'
  }));

});
```

This provides the basis for developing a RESTful API adhering to the JSON:API specification through linked entity representations.
# Conclusion

To summarize, adhering to established API specifications provides structure, discoverability and future-proofs applications. Alignment ensures intuitiveness.

Developing complex, related backends according to spec requires diverse expertise across technical domains. Partnering with an experienced Node.js firm streamlines this.

As a full-service Node.js development company located in Austin, we offer [Node.js Development Services in Austin](https://hybridwebagency.com/austin-tx/custom-laravel-development-services/) to help with building robust, specification-aligned APIs. Our team has extensive experience implementing standards like HAL and JSON:API for resource relationships. Whether prototypes, scaling or full lifecycles, we deliver production-ready REST services on schedule and budget.

By capitalizing on our Node.js and framework know-how combined with architectural best practices, teams can prioritize core work while we ensure technical success meeting quality and performance. Contact us to discuss how our Austin-based services can expedite your goals.

## References

- https://www.halspec.org/
The official specification website for HAL (Hypertext Application Language), detailing the standard.

- https://jsonapi.org/
The official website for the JSON:API specification, including documentation on compliant API design. 

- https://expressjs.com/en/advanced/best-practice-performance.html
Best practices guide from the Express.js website covering performance, security, and standard compliance for Node.js APIs.  

- https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-design
Documentation from Microsoft on API design best practices, including modeling relationships and using standards.

- https://swagger.io/resources/building-blocks/hypermedia/
Swagger overview of hypermedia APIs and industry standards like HAL and JSON:API for enabling discoverability.
