# Neo4j
Technical homework assignment 

## Create Graph Database  
The CREATE statement was exported from [arrows.app](https://arrows.app/#/local/id=SOGAXPZOO3ZddOIpkT7T) using [Investigation for Public Safety - Social Network Data](https://gist.github.com/maruthiprithivi/10b456c74ba99a35a52caaffafb9d3dc) data
```bash
CREATE ({nameoforganization: "", designation: "", startyear: "", `endyear `: ""})<-[:WORKED_AT]-(`Person ` {passportnumber: "", name: ""})-[:TRAVELED_TO]->({name: ""})<-[:IS_FROM]-(`Person `)-[:STUDIED_AT]->({nameofinstitution: ""}),
({cardnumber: ""})<-[:HAS_CARD]-(`Person `)-[:MADE_TRANSACTION]->({name: ""}),
({name: ""})
```

## Import CSV data 
```bash
// Create Person nodes and their educational history relationships
LOAD CSV WITH HEADERS FROM 'file:///I:/Data/sng_education.csv' AS row
CREATE (p:Person {passportnumber: row.passportnumber, name: row.name})
MERGE (i:Institution {nameofinstitution: row.nameofinstitution})
CREATE (p)-[:STUDIED_AT {course: row.course, startyear: row.startyear, endyear: row.endyear}]->(i)
SET p.country = row.country;

// Create Person nodes and their transaction history relationships
LOAD CSV WITH HEADERS FROM 'file:///I:/Data/sng_transaction.csv' AS row
MATCH (p:Person {passportnumber: row.passportnumber})
CREATE (c:Card {cardnumber: row.cardnumber})
MERGE (m:Merchant {name: row.merchant})
CREATE (p)-[:HAS_CARD]->(c)
CREATE (p)-[:MADE_TRANSACTION {transactiondate: row.transactiondate, amount: row.amount}]->(m);

// Create Person nodes and their travel history relationships
LOAD CSV WITH HEADERS FROM 'file:///I:/Data/sng_trips.csv' AS row
MATCH (p:Person {passportnumber: row.passportnumber})
MERGE (c:Country {name: row.departurecountry})
MERGE (d:Country {name: row.arrivalcountry})
CREATE (p)-[:DEPARTED_FROM {departuredate: row.departuredate}]->(c)
CREATE (p)-[:ARRIVED_AT {arrivaldate: row.arrivaldate}]->(d);

// Create Person nodes and their work history relationships
LOAD CSV WITH HEADERS FROM 'file:///I:/Data/sng_work.csv' AS row
MATCH (p:Person {passportnumber: row.passportnumber})
CREATE (o:Organization {nameoforganization: row.nameoforganization, designation: row.designation, startyear: row.startyear, endyear: row.endyear})
CREATE (p)-[:WORKED_AT]->(o);
```

## Cypher Query
```bash
MATCH (person:Person)-[:STUDIED_AT]->(institution:Institution {nameofinstitution: "Smart National University of Vietnam"})
RETURN person.name, person.passportnumber;
```
