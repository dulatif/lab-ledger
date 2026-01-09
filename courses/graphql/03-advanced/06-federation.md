# Lesson 6: Schema Federation

## Learning Goals

- Microservices.
- The Supergraph.

## 1. The Concept

Instead of one Monolith schema, you have multiple Subgraphs.

- **User Service**: Defines `User` type.
- **Product Service**: Defines `Product` type.
- **Review Service**: Extends `User` and `Product` to add `reviews`.

## 2. The Gateway

The Gateway fetches the schemas from all services and composes a "Supergraph".
The client queries the Gateway, which routes requests to the appropriate services.

## Key Takeaways

- Allows teams to work independently on their own slice of the Graph.
- **Apollo Federation** is the industry standard.
