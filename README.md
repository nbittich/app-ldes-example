# LDES example

### Installation

- `cp .env.example .env`
- `docker-composer up -d` (or `docker compose up -d`)

### Test

- Run the following:

```
curl -v   POST http://localhost/example-stream?resource=https://example.org/person/80325 \
   -H "Content-Type: application/ld+json" \
   -d '{"@context":"https://json-ld.org/contexts/person.jsonld","@id":"https://example.org/person/80325","@type":"Person","name":"John Lennon","born":"2040-10-09","spouse":"http://dbpedia.org/resource/Cynthia_Lennon"}'
```

- Wait for like 30 seconds (or a minute if nothing shows yet)

- Go to `http://localhost:8890/sparql`

- Run the following sparql query (you should have john lennon & his wife in resource):

```sparql
select * where {
   graph <http://mu.semte.ch/application>{
       ?s ?p ?o
   }
}
```

- Now let's make an update:

```
curl -v   POST http://localhost/example-stream?resource=https://example.org/person/80325 \
   -H "Content-Type: text/turtle" \
   -d '<https://example.org/person/80325> <http://xmlns.com/foaf/0.1/name> "Bob Dylan".'
```

- Wait for a minute or so

- Execute the same query as above, you should get Bob Dylan now

## Troubleshoot

You may need to restart the consumer, if there's no data it seems it can get stuck sometimes.
