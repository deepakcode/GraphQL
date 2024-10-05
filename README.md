#### Old way (X)
https://github.com/LEMUBIT/LaptopGraphQLServer

### Apollo Graphql 
https://www.apollographql.com/docs/kotlin/v2/essentials/get-started-java

### Federated Schemas
https://www.apollographql.com/docs/graphos/schema-design/federated-schemas/federation

### Postman New (GraphQL)
https://github.com/graphql/graphql-playground

### Graphql Playground
https://github.com/graphql/graphql-playground

### New
https://github.com/spring-guides/gs-graphql-server/tree/main
https://github.com/graphql/graphiql/tree/main/packages/graphiql#readme
https://github.com/apollographql/spotify-showcase
https://www.youtube.com/watch?v=h9IsV0AlFqE


### Example DGS+Apollo: 
https://netflix.github.io/dgs/examples/

### DGS Example
https://github.com/Netflix/dgs-examples-java


# Practice Example

[graphql-playground](https://github.com/graphql/graphql-playground)

`$ brew install --cask graphql-playground`

<details>
  <summary>01. GraphQL+Srping</summary></summary>

https://github.com/LEMUBIT/LaptopGraphQLServer

Library/Dependency
```
dependencies {
	implementation 'com.graphql-java:graphql-java:11.0'
	implementation 'com.graphql-java:graphql-java-spring-boot-starter-webmvc:1.0'
	implementation 'com.google.guava:guava:26.0-jre'
	implementation 'org.springframework.boot:spring-boot-starter-web'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```

```java
@Component
public class GraphQLProvider {

    @Autowired
    DataFetcher laptopDataFetcher;

    private GraphQL graphQL;

    @Bean
    public GraphQL graphQL()
    {
        return graphQL;
    }

    @PostConstruct
    public void init() throws IOException
    {
      URL url = Resources.getResource("schema.graphqls");
      String schemaString = Resources.toString(url, Charsets.UTF_8);
      GraphQLSchema graphQLSchema = buildSchema(schemaString);
      this.graphQL = GraphQL.newGraphQL(graphQLSchema).build();
    }


    private GraphQLSchema buildSchema(String schemaString)
    {
        TypeDefinitionRegistry typeDefinitionRegistry = new SchemaParser().parse(schemaString);
        RuntimeWiring runtimeWiring = buildWiring();
        SchemaGenerator schemaGenerator = new SchemaGenerator();
        return  schemaGenerator.makeExecutableSchema(typeDefinitionRegistry, runtimeWiring);
    }

    private RuntimeWiring buildWiring()
    {
        return RuntimeWiring.newRuntimeWiring()
                .type(mutationBuilder())
                .type(queryBuilder())
                .build();
    }

    private TypeRuntimeWiring.Builder mutationBuilder() {
        return TypeRuntimeWiring.newTypeWiring("Mutation")
                .dataFetcher("deleteLaptop", laptopDataFetcher.deleteLaptop());
    }

    private TypeRuntimeWiring.Builder queryBuilder()
    {
        return TypeRuntimeWiring.newTypeWiring("Query")
                .dataFetcher("getAllLaptops", laptopDataFetcher.getAllLaptops())
                .dataFetcher("getLaptopByID", laptopDataFetcher.getLaptopByID())
                .dataFetcher("getLaptopsLessThan", laptopDataFetcher.getLaptopsLessThan());

    }

}
```
</details>


<details>
  <summary>02. GraphQL+Srping+ GraphiQL</summary></summary>

https://github.com/spring-guides/gs-graphql-server

Library/Dependency
```
dependencies {

    implementation 'org.springframework.boot:spring-boot-starter-web'

    //Graph QL
    implementation 'org.springframework.boot:spring-boot-starter-graphql'
    testImplementation 'org.springframework.graphql:spring-graphql-test'

    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    testImplementation 'org.springframework:spring-webflux'

}
```

```java
package com.example.graphqlserver.graphql;

import com.example.graphqlserver.domain.Author;
import com.example.graphqlserver.domain.Book;
import org.springframework.graphql.data.method.annotation.Argument;
import org.springframework.graphql.data.method.annotation.QueryMapping;
import org.springframework.graphql.data.method.annotation.SchemaMapping;
import org.springframework.stereotype.Controller;

@Controller
public class BookController {
    @QueryMapping
    public Book bookById(@Argument String id) {
        return Book.getById(id);
    }

    @SchemaMapping
    public Author author(Book book) {
        return Author.getById(book.authorId());
    }
}
```
</details>


<details>
  <summary>03. GraphQL+Srping+ GraphiQL</summary></summary>

https://github.com/eugenp/tutorials/tree/master/graphql-modules/graphql-dgs

</details>
