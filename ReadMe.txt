DDD (https://www.youtube.com/watch?v=lE6Hxz4yomA)
https://github.com/mastreeno/Merp/tree/master/src

01) Crunching knowledge about the domain
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
	Modules = .NET Namespace, Values Objects (class described by attributes, immutable) and Entities (when uniqueness is important)
	"public int OutsideTemperature" would be "public Temperature OutsideTemperature".
	This new value object can contain: constructor, min/max constants, ad hoc setters with built-in validation.
	Entities need an identity, uniqueness is important, typically made of data and behavior, contains domain logic but not persistence logic
	Aggregates are a few individual entities constantly used and referenced together. Cluster of associated objects treated as one for data changes.
	Aggregate roots are the public endpoints that the outside world uses to query or command. Perserves transactional integrity.
14) Domain Layer: logic invariant to use-cases, domain model, domain services (takes care of persistence tasks)	
15) Domain services: contains pieces of domain ligic that don't fit into any of the existing entities. Classes that group logically related behaviors.
	Typically operating on multiple domain entities. Implementation of processes that require access to the persistence layer for reads and writes.
	Require access to external services. Repositories, proxies. Coordinates the activity of aggregates and repositories with the purpose of implementing a business action.
	May consume services from the infrastructure, such as when sending an email or a text message is necessary. Actions in domain services come from requirements
	and are approved by domain experts. Names used in domain services are strictly part of the ubitiquous language.
16) Fundamental facilities of software systems: Persistence > Security > Logging/Tracing > Inversion of control > Caching > Networks	
17) Persistence Model: objected oriented model 1:1 with underlying relational data. Reliable and familiar to most developers. Doesn't include business logic
18) Domain model: models for the business domain, objected-oriented entity model, functional model. Classes expected to expose data and behavior. 
	Object oriented model for business logic. Persistable model, no persistence logic inside.
19) What is behavior? Methods that validate the state of the object. Methods that invoke business actions to perform on the object.
	Methods that express business processes involving the object.
20) Aggregate: work with fewer objects and coarse grained and with fewer relationships. Protect as much as possible the graph of entities from
	outsider access. Ensure the state of child entities is always consistent. Actual boundaries of aggregates are determined by business rules.
	Customer (distinct aggregate), Address (distinct aggregate), CustomerAddress (aggregate? perhaps, if address solely exists to be an attribute of customer).
21) Aggregate Root: ensure encapsulated objects are always in a consistent state. Take care of persistence for all encapsulated objects. Cascade updates and
	deletions through the encapsulated objects. Access to encapsulated objects must alsways happen by navigation. One repository per aggregate.
22) Repository: in DDD, a repository is just the class that handles persistence on behalf of entities and ideally aggregate roots. Most popular type of domain  service.
	Take care of persisting aggregates. One repository per aggregate root. Assembly with repositories has a direct dependency on data stores.
	A repository is where you deal with connection strings and use SQL commands.
23) Why should you consider events in Domain Layer? Not strictly necessary but just a more effective and resilient way to express the intricacy of some
	real-world business domains. Benefits include: non monolithic code, no need to touch service for future changes, no need to have all handling code in one place,
	raise of the event distinct from handling the event, can handle the same event in multiple places
24) Event Bus: external element, part of the infrastructure, notify listeners (NServiceBus). More flexibility, can record events, can have any number of handlers.
	Events help significantly to coordinate actions within a workflow and to make use-case workflows a lot more resilient to changes.
	You can write your own bus if you want but not recommended, see NService Bus, Rebus, MassTransit, Azure Service Bus.
25) Anemic Domain Model: anti-pattern because "it takes behavior away from domain objects". Entities only made of properties. Required logic placed in service components.
	Services orchestrate the application logic.

CQRS (Command Query Responsiblity Segregation)
01) Command: alter state, doesn't return data
02) Query: returns data, doesn't alter state
03) Benefits include distinct optimization, scalability potential, simplified design, hassle-free stacks enhancement.
04) Use a read-only wrapper for the DbContext instance you use in the read stack if using LINQ or EF.
	public class Database : IDisposable {
		private readonly QueryDbContext _db = new QueryDbContext();
		public IQueryable<Customer> Customers { get { return _db.Customers; } }
		public void Dispose() { _db.Dispose(); }
	}
05) In MVC/Web.API project, we would have references to persistence libraries (i.e., ADO.NET, EF, or Dapper). We would have an ApplicationLayer folder with services (AdminService.cs).
	In query flow, presentation > application (MVC/Web.API project) > read stack (different assembly) > view model
	In command flow, presentation > application (MVC/Web.API project) > command stack (different assembly) > redirect to read stack (if needed, post redirect get pattern, ensures last action is get)
06) Storage synchronization: how do you keep write and read DBs in sync? How would you define stale data? How critical is stale data for the app? There are many approaches:
	Synchronous, every command triggers sync updates, automatically up-to-date
	Asynchronous, every command triggers async updates, eventually up-to-date
	Scheduled, a job runs periodically and updates the read storage, controlled staleness
	On-demand, updates triggered by requests (if older enough), controlled up-to-date
07) Representing messages: usually starts with base class (Message)
	public class Message {
		public DateTime Timestamp { get; protected set; }
		public string SagaId { get; protected set; }
	}
	public class Command: Message {
		public string Name { get; protected set; }
	}
	public class Event: Message {
		// Any properties that may help retrieving and persisting events.
	}
08) Deluxe CQRS is task and message based. Message based orchestration of business tasks, incorporate an EventBus.
09) Saga (a long, involved story, account, or series of incidents): command or event that starts the associated busines process. List of commands the saga can handle. 
	List of events the saga is interested in. Sagas might be persistent and stateful. Persistence is care of the bus. State of associated aggregate must be persisted.
	Sages might be stateless as well. Helps extend solution when new scenarios or features arise. Write a new sage or handler and register it with the bus, that's it.
	Sagas must be identified by a unique ID. Can be a GUID, can be the ID of the aggregate the saga is all about. Can be combo of values that is unique in the context.	
	public class CheckoutSaga : Saga<CheckoutSagaData>,
		IStartWith<StartCheckoutCommand>,
		ICanHandle<CancelCheckoutCommand>,
		ICanHandle<PaymentCompletedEvent>,
		ICanHandle<PaymentDeniedEvent>,
		ICanHandle<DeliveryRequestRefusedEvent>,
		ICanHandle<DeliveryRequestApprovedEvent> {
		public void Handle(StartCheckoutCommand message) {}
	}

Event Sourcing
01) It's about ensuring that all changes made to the application state during the entire lifetime of the application are stored as a sequence of events.
02) Instead of modeling a shopping cart for example you can represent the state of the shopping cart by event representation. Add item #1 > Add item #2 >
	Add payment info > Update item #2 > Remove item #1 > Adding shipping info
03) CRUD = current state storage (snapshot), Event Sourcing = event-based representation of data
04) Event is something that has happened in the past. Events are expression of the ubiquitous language. Events are not imperative and are named using past tense verbs.
	Have a persistent store for events. Append-only, no delete. Replay the (related) events to get to the last known state of an entity. Once stored, events are immutable.
	Can be duplicated and replicated (for scalability reasons). Any behavior associated with he event has been performed. Replaying the event doesn't require to repeat the behavior.
	You don't miss a thing. Track everything that happened at the time it happened. Regardless of the effects it produced.
05) Event-based Data Stores, Event Store (http://geteventstore.com)