## RMLTC0002d-PostgreSQL

**Title**: "Two columns mapping, generation of a BlankNode subject by using a SQL Query that concatenates two columns"

**Description**: "Tests (1) two column mapping, no primary key; (2) generation of a BlankNode subject by using a SQL Query that concatenates two columns; (3) one column to one property"

**Error expected?** No

**Input**
```
DROP TABLE IF EXISTS student CASCADE ;
CREATE TABLE student (
  ID INTEGER,
  Name VARCHAR(50)
);
INSERT INTO student values ('10', 'Venus');

```

**Mapping**
```
@prefix d2rq: <http://www.wiwiss.fu-berlin.de/suhl/bizer/D2RQ/0.1#> .
@prefix foaf: <http://xmlns.com/foaf/0.1/> .
@prefix rml: <http://w3id.org/rml/> .

<http://example.com/base/TriplesMap1> a rml:TriplesMap;
  rml:logicalSource [ a rml:LogicalSource;
      rml:iterator "SELECT ('Student' || ID) AS StudentId, ID, Name FROM student";
      rml:referenceFormulation rml:SQL2008Query;
      rml:source <http://example.com/base/#DB_source>
    ];
  rml:predicateObjectMap [
      rml:objectMap [
          rml:reference "name"
        ];
      rml:predicate foaf:name
    ];
  rml:subjectMap [
      rml:reference "studentid";
      rml:termType rml:BlankNode
    ] .

<http://example.com/base/#DB_source> a d2rq:Database;
  d2rq:jdbcDSN "CONNECTIONDSN";
  d2rq:jdbcDriver "org.postgresql.Driver";
  d2rq:password "";
  d2rq:username "postgres" .

```

**Output**
```
_:Student10 <http://xmlns.com/foaf/0.1/name> "Venus".

```

