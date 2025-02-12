# Geovistory, a Linked Open Data research environment for the Humanities

Graph and Network Conference, Mendrisio, 13-14.02.2025

This page is dedicated to the Geovistory Workshop of the Graph Conference 2025 in Mendrisio.

## Useful links

Geovistory: https://www.geovistory.org/

Community project: https://www.geovistory.org/project/1483135

Introduction: https://docs.google.com/presentation/d/1H2KmfPSqQAsanbpOfjibYstW_HVHtcZtH8Zjj0Boqn8/edit?usp=sharing

LOD 101: https://docs.google.com/presentation/d/1Rs-dq79Ap8stMlLkd0RymPiVZ4BrkjF5-y5N3LIOZeg/edit?usp=sharing

Hands-on: https://docs.google.com/presentation/d/15Tr-fC1T5EawtYiM5Nxx6SgrKnpSQMMEw-Nn8AGjXVc/edit?usp=sharing

## SPARQL queries

### Request finding all the persons born in Paris

```
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX ontome: <https://ontome.net/ontology/>
PREFIX geov: <http://geovistory.org/resource/>

SELECT DISTINCT ?person ?personLabel
WHERE { ?person a ontome:c21 ;
            rdfs:label ?personLabel ;
            ontome:p86i ?birth .
        ?birth ontome:p7 geov:i81770 . }
LIMIT 200
```

### Request for the persons born in Paris and finding their IdRef URIs, then the BNF URIs and finding their publication dates

```
PREFIX geo: <http://www.opengis.net/ont/geosparql#>
PREFIX ontome: <https://ontome.net/ontology/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX foaf: <http://xmlns.com/foaf/0.1/>
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX geov: <http://geovistory.org/resource/>

SELECT DISTINCT ?geovPerson ?geovPersname ?BNFUri_corr ?work ?date
WHERE {
  {SELECT *
    {?geovPerson a ontome:c21 .
     ?geovPerson ontome:p86i ?birth .
     ?birth ontome:p7 geov:i81770 .
    ?geovPerson rdfs:label ?geovPersname;
    ontome:p1943 / ontome:p1843 ?uri .
      FILTER ( strstarts(?uri,"http://www.idref"))
      BIND (URI(?uri) as ?idRefUri)
  }
  LIMIT 50}
  SERVICE <https://data.idref.fr/sparql> {
      ?idRefUri owl:sameAs ?BNFUri.
OPTIONAL {
    ?idRefUri skos:prefLabel ?prefLabel .
    }
          }
        FILTER( CONTAINS(STR(?BNFUri), 'bnf'))

    BIND(URI(CONCAT((STRBEFORE(STR(?BNFUri), "#foaf:Person")), '#about')) AS ?BNFUri_corr)
  
  OPTIONAL {
  
  SERVICE <https://data.bnf.fr/sparql> {
    ?work dcterms:creator ?BNFUri_corr .
    ?work dcterms:date ?date .
    }
  }
  }
  LIMIT 1000
```

## To go further

Données ouvertes liées et recherche historique : un changement de paradigme. Beretta, F. Humanités numériques, July, 2023. Number: 7 Publisher: Humanistica <https://doi.org/10.4000/revuehn.3349>

Francesco Beretta. Semantic Data for Humanities and Social Sciences (SDHSS): an Ecosystem of CIDOC CRM Extensions for Research Data Production and Reuse. Thomas Riechert; Hartmut Beyer; Jennifer Blanke; Edgard Marx. Professorale Karrieremuster Reloaded. Entwicklung einer wissenschaftlichen Methode zur Forschung auf online verfügbaren und verteilten Forschungsdaten- banken der Universitätsgeschichte., HTWK Leipzig / OA-HVerlag, pp.73-102, 2024 <https://shs.hal.science/halshs-04450546>
