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


## Practice Example

# graphql-playground

https://github.com/graphql/graphql-playground

`$ brew install --cask graphql-playground`

<details>
  <summary>GraphQL+Srping</summary></summary>

Library/Dependency
```
	implementation 'com.graphql-java:graphql-java:11.0'
	implementation 'com.graphql-java:graphql-java-spring-boot-starter-webmvc:1.0'
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
