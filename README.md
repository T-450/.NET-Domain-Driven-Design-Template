# .NET Domain Driven Design implemented with Clean Architecture and Vertical slices.
This template aims to facilitate the development of highly decoupled monolithic .NET applications, with the flexibility to transition to microservices if your business needs evolve.

**Key Features**:
- **Bounded Contexts Separation**: Each bounded context is isolated in its own project, significantly minimizing the risk of domain coupling. This approach allows for independent domain development within a monolithic structure.
- **Streamlined Development**: By consolidating all contexts into the StartupProject, the solution avoids the complexity of managing multiple microservice deployments. This enables rapid development akin to a monolith while maintaining strict separation between domains.

<div align="center">
  <img src="./architecture-diagram.png" alt="Description of Image" style="width:90%;">
</div>

### Running the solution:
- Migrations - execute the bash script to create the project migrations - ./run_migrations.sh
- Set a connection string for your database

## :bulb: Core Philosophy
Most DDD project templates fail to prevent unintentional coupling of bounded contexts. Those that do address this issue often separate bounded contexts into independent projects. While this approach avoids coupling, it introduces significant DevOps overhead, such as microservice orchestration, service discovery, and common NuGet package management. This template resolves coupling issues by separating contexts into individual projects, but ensures they all run within a single binary, thus avoiding excessive DevOps complexity.

### Domain Modeling and Development Process
Begin by extensively communicating with your customers to gain a thorough understanding of the business domain and use cases. Use this insight to divide the domains or vertical slices in a way that allows them to operate independently. If you frequently find two domains being used together, consider merging them. Conversely, if you regularly use only specific use cases from one domain independently of others, consider splitting them into separate domains.

### CQRS & Repositories
In .NET, repositories primarily act as anti-corruption layers for aggregate roots or for implementing CQRS, especially if you're looking to decouple from Entity Framework (such as moving READ operations to Dapper). If your project doesn't leverage these benefits, using DbContext directly can simplify development.
Consider organizing your repositories into Query and Domain repositories: Query repositories return response objects and are typically housed in the Application project, while Domain repositories return Domain objects and are defined within the Domain project.

### Anti-corruption layers and validation
Factories and Repositories serve as crucial anti-corruption layers, complementing fluent validations. Domain objects are internal and must only be created through Factories. Validation is implemented across all layers, with a particular emphasis on the domain layer. Ensuring the core domain is properly validated and bug-free is essential, as invalid state or bugs at this level will propagate to the rest of the layers.

### Data Storage
You have two primary options for data storage:
- (Recommended) Use a single database for all domains, with each domain having its own bounded context. This approach simplifies development and speeds up the process. Transitioning to microservices later will require only a migration script for the data.
- Use a separate database for each domain. This simplifies the transition to microservices since you only need to split the domain into a separate repository. However, managing multiple databases from the beginning can slow down development somewhat.reduce development speed a bit.

### Communication Between Bounded Contexts
Bounded contexts communicate either through event sourcing or API calls. If you encounter a use case that spans across two bounded contexts and doesn't fit into an existing one, consider creating a new Aggregator bounded context to handle it effectively.

## :construction_worker: Built with

- [.NET Core 8](https://github.com/dotnet/core) 
- [ASP.NET Core 8](https://github.com/dotnet/aspnetcore)
- [Entity Framework Core 8](https://github.com/dotnet/efcore)
- [MediatR](https://github.com/jbogard/MediatR)
- [FluentValidation](https://github.com/FluentValidation/FluentValidation)
- [AutoMapper](https://github.com/AutoMapper/AutoMapper)
- [Scrutor](https://github.com/khellang/Scrutor)
- [xUnit](https://github.com/xunit/xunit)
- [FluentAssertions](https://github.com/fluentassertions/fluentassertions)