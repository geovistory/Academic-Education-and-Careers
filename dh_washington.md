# Geovistory, a LOD Research Infrastructure for Historical Sciences

Digital Humanities Conference, Washington, 06.08.2024

# General links

Geovistory website: <https://www.geovistory.org/>
Registering: <https://toolbox.geovistory.org/login>

## Introduction Slides

<https://docs.google.com/presentation/d/1ZW165b91vAQzIBy1junsE_VXxNr5n4cyGbEsa3RvYGY/edit?usp=sharing>

## Hands-on

List of persons to add:
<https://docs.google.com/document/d/1KeCX1yzNFHhkmeKTdPvqOtIfRYKjUEhOE6RJRcCz3VM/edit?usp=sharing>

Step by step slides:
<https://docs.google.com/presentation/d/1zBfhKs6E-sXKtIo2K9qvh0HxTEGPvAt5B03N67HI_RU/edit>

## SPARQL queries:

### birth place of tagged entities

```
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX ontome: <https://ontome.net/ontology/>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>

SELECT ?label ?long ?lat (count(?sv) * 0.5 as ?radius) (count(?sv) as ?number) ("Birth place" as ?type) ?link
WHERE {

    # Geographical Place -had presence-> Presence -was at-> Place (lat/long)
    ?s ontome:p147i/ontome:p148 ?place.

    # Geographical Place -label-> label
    ?s rdfs:label ?label.

    # Geographical Place -is place of-> Birth
    ?s ontome:p7i ?sv.
  
    # ?sv ontome:p86 ?person.
  
      # ?person ontome:p1440 <http://geovistory.org/resource/i13669562>.
  
    ?sv ontome:p86 / ontome:p1440 <http://geovistory.org/resource/i13669562>.

    # Extract lat and long from WKT
    bind(replace(str(?place), '<http://www.opengis.net/def/crs/EPSG/0/4326>', "", "i") as ?rep)
    bind(xsd:float(replace(str(?rep), "^[^0-9\\.-]*([-]?[0-9\\.]+) .*$", "$1" )) as ?long )
    bind(xsd:float(replace( str(?rep), "^.* ([-]?[0-9\\.]+)[^0-9\\.]*$", "$1" )) as ?lat )

    # Append the project query param to the URI
    bind(concat(str(?s), "?p=1483135") as ?link )
}
GROUP BY ?label ?long ?lat ?type ?link
```

### federated querry
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

## To go further

Données ouvertes liées et recherche historique : un changement de paradigme. Beretta, F. Humanités numériques, July, 2023. Number: 7 Publisher: Humanistica <https://doi.org/10.4000/revuehn.3349>

Francesco Beretta. Semantic Data for Humanities and Social Sciences (SDHSS): an Ecosystem of CIDOC CRM Extensions for Research Data Production and Reuse. Thomas Riechert; Hartmut Beyer; Jennifer Blanke; Edgard Marx. Professorale Karrieremuster Reloaded. Entwicklung einer wissenschaftlichen Methode zur Forschung auf online verfügbaren und verteilten Forschungsdaten- banken der Universitätsgeschichte., HTWK Leipzig / OA-HVerlag, pp.73-102, 2024 <https://shs.hal.science/halshs-04450546>

