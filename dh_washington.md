# Geovistory, a LOD Research Infrastructure for Historical Sciences

Digital Humanities Conference, Washington, 06.08.2024

# General links

Geovistory website: <https://www.geovistory.org/>
Registering: <https://toolbox.geovistory.org/login>

## Slides

<https://docs.google.com/presentation/d/1ZW165b91vAQzIBy1junsE_VXxNr5n4cyGbEsa3RvYGY/edit?usp=sharing>

## Working document

Link to the Google Doc:
https://docs.google.com/document/d/1KeCX1yzNFHhkmeKTdPvqOtIfRYKjUEhOE6RJRcCz3VM/edit?usp=sharing

## Federated SPARQL query:
```
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX ontome: <https://ontome.net/ontology/>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
prefix owl: <http://www.w3.org/2002/07/owl#>


SELECT ?s ?sLabel ?idRef ?biblio
WHERE {
          {
          SELECT ?s ?idRef ?sLabel
            WHERE 
            {
             ?s a ontome:c21;
                 ontome:p1440 <http://geovistory.org/resource/i13669562>;
                 owl:sameAs ?idRef;
                 rdfs:label ?sLabel.
              FILTER (CONTAINS(STR(?idRef), 'idref')) 
            }
    LIMIT 100
      }
  SERVICE <https://data.idref.fr/sparql> {
  ?si <http://id.loc.gov/vocabulary/relators/aut> ?idRef;
     <http://purl.org/dc/terms/bibliographicCitation> ?biblio} 
}
```
