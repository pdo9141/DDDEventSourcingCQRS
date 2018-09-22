DDD (https://www.youtube.com/watch?v=lE6Hxz4yomA)
https://github.com/mastreeno/Merp/tree/master/src

01) Crunchg knowledge about the domain
02) Recognize subdomains
03) Design a rich domain model
04) Code by telling objects in the domain model what to do
05) Ubiquitous Language: vocabulary of domain specific terms, nouns, verbs, adjectives, idiomatic expressions, adverbs.
	Shared by all parties involved in the project. Primary goal of avoiding misunderstandings.
	Used in all forms of spoken and written communication. Universal language of the business as done in the organization.
06) Bounded Context: Delimited space where an element has a well-defined meaning. Any elements of the ubiquitous language.
	Beyond the boundaries of the context, the language changes. Each bounded context has its own ubiquitous language.
	Business domain split in a web of interconnected contexts. Each context has its own architecture and implementation.
07) Context Map: diagram that provides a comprehensive view of the system being designed (core domain, backoffice, club site, weather forecasts)
08) Devs/Domain Experts (Try to achive no ambiguity, no synonyms):
	Delete the booking > Cancel the booking
	Submit the order > Checkout
	Update the job order > Extend the job order
	Create the invoice > Register/Accept the invoice
	Set state of game > Start/Pause the game
09) Design from starting from UI (top, down approach)
10) Application logic: dependent on use-cases, application entities, application workflow
	Date transfer objects, application services				
11) Domain logic: invariant to use-cases, business entities, business workflow components. About baking business rules into the code
	Domain model, domain services
12) Business rule: statements that detail the implementation of a business process or describe a business policy to be taken into account.
	Business logic worflows, business logic components, business rule engine, domain model (rules encapsulated in the model)
13) Domain Model Pattern: aggregated objects, data and behavior. Classes in domain model should be persistence agnostic. Paired with domain services.
14) Domain Layer: logic invariant to use-cases, domain model, domain services (takes care of persistence tasks)	
15) Domain model: models for the business domain, objected-oriented entity model, functional model. Classes expected to expose data and behavior. 
16) Domain services: contains pieces of domain ligic that don't fit into any of the existing entities. Classes that group logically related behaviors.
	Typically operating on multiple domain entities. Implementation of processes that require access to the persistence layer for reads and writes.
	Require access to external services.
17) Fundamental facilities of software systems: Persistence > Security > Logging/Tracing > Inversion of control > Caching > Networks	