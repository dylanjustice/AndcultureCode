# Onion Architecture

---

## Introduction

At andculture our recommended backend application architecture is Clean Architecture. The clean architecure seeks to clearly separate concerns to prevent tightly coupled code.


## Resources

* [Common web application architectures](https://docs.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/common-web-application-architectures)
* [The Onion Architecture: part 1](https://jeffreypalermo.com/2008/07/the-onion-architecture-part-1/)


## Team decisions

While we should always strive to follow best practices from clean architecture, there will always be areas the team can customize those design patterns to their liking.

### Domain Services: Conductors & Use Cases

(Aka managers, services, interactors)

Within the business layers of the onion, the team has decided to standardize on what those domain services are called depending upon the variant of the clean architecture.

- Hexagonal Architecture: Use Cases
- Onion Architecture: Conductors

Typically these services are broken out by "aspect" (aka oeration, concern, or action). Understanding that typically a request is concerned with a given domain (entity) and aspect (operation). Instead of having a monolithic service for an entity (ie. UserConductor), you have multiple by concern (ie. UserReadConductor, UserImportConductor, etc...).

While there could be other classifications that evolve, the two primary scenarios are "Aspect" and "Domain" services.

#### Aspect

(Aka concerns, actions, operations)

Aspect services are agnostic of domain/entity and perform shared logic of that type of aspect. With a nod to AOP (aspect oriented programming), these are your business' cross cutting concerns. Almost all applications have CRUD (create, read, update, delete) aspects for example.

The exact implementation details of these services is not strict. They can come in the form of a base class or collection of dependency injected classes. Either way, at their simpliest form, they are shared logic around common business concerns.

Why not call them `Shared`? Just as your domain/entity naming should be well thought out, aspects should be as well. This will promote consistency, code sharing and much more. If you have a new aspect in the system, that should be talked about, have requirements, flows, automated tests and so on so everyone should agree on terminology.

Examples
- Clone
- Create
- Delete
- Import
- Read

When it comes to folder structure, typically these will just be a flat directory of services...

- Business
    - Conductors
        - Aspects
            - CreateConductor
            - ImportConductor
        - Domain
            - ...

#### Domain

(Aka entity)

This classification of services contain logic specific to this entity/domain that can't be handled more generically (with an aspect). These are the most commonly occurring service. They should mirror your entities in their naming. If an observer opens a given entity's folder they can quickly discern what all the system can do related to that domain.

- Business
    - Conductors
        - Aspects
            - ...
        - Domain
            - Users
                - UserCreateConductor
                - UserImportConductor

##### Do "pure" domain services exist? (ie. UserConductor)

Logic contained in these services should be state management related operations for that entity/domain. Perhaps the best way to think of it is a state pattern (aka strategy pattern).

Code in these actors should not intersect with aspects/concerns and purely pertain to state. For example, it should not set an entity's state AND save. It should just set the state and leave the 'save' to the respective service (ie. 'create' or 'update').

WARNING: Tread lightly. It is very easy to want ot just jam everything related to that entity/domain into these services, which is an anti-pattern.