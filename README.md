# Neo4j
Technical homework assignment 

## Create Graph Database from  
The CREATE statement was exported from [arrows.app](https://arrows.app/#/local/id=SOGAXPZOO3ZddOIpkT7T) using [Investigation for Public Safety - Social Network Data](https://gist.github.com/maruthiprithivi/10b456c74ba99a35a52caaffafb9d3dc) data
```bash
CREATE ({nameoforganization: "", designation: "", startyear: "", `endyear `: ""})<-[:WORKED_AT]-(`Person ` {passportnumber: "", name: ""})-[:TRAVELED_TO]->({name: ""})<-[:IS_FROM]-(`Person `)-[:STUDIED_AT]->({nameofinstitution: ""}),
({cardnumber: ""})<-[:HAS_CARD]-(`Person `)-[:MADE_TRANSACTION]->({name: ""}),
({name: ""})
```

## Import CSV data 
```bash
LOAD CSV WITH HEADERS FROM 'file:///I:/Data/sng_education.csv' AS row
MERGE (person:Person {passportnumber: row.passportnumber, name: row.name})
MERGE (institution:Institution {nameofinstitution: row.nameofinstitution})
MERGE (course:Course {name: row.course})
MERGE (person)-[:STUDIED_AT {startyear: row.startyear, endyear: row.endyear}]->(institution)
MERGE (person)-[:ENROLLED_IN]->(course);

LOAD CSV WITH HEADERS FROM 'file:///I:/Data/sng_transaction.csv' AS row
MERGE (person:Person {passportnumber: row.passportnumber, name: row.name})
MERGE (merchant:Merchant {name: row.merchant})
MERGE (person)-[:MADE_TRANSACTION {cardnumber: row.cardnumber, transactiondate: row.transactiondate, amount: row.amount}]->(merchant);

LOAD CSV WITH HEADERS FROM 'file:///I:/Data/sng_trips.csv' AS row
MERGE (person:Person {passportnumber: row.passportnumber, name: row.name})
MERGE (departure:Country {name: row.departurecountry})
MERGE (arrival:Country {name: row.arrivalcountry})
MERGE (person)-[:TRAVELED_TO {departuredate: row.departuredate, arrivaldate: row.arrivaldate}]->(arrival);

LOAD CSV WITH HEADERS FROM 'file:///I:/Data/sng_work.csv' AS row
MERGE (person:Person {passportnumber: row.passportnumber, name: row.name})
MERGE (organization:Organization {nameoforganization: row.nameoforganization})
MERGE (person)-[:WORKED_AT {designation: row.designation, startyear: row.startyear, endyear: row.endyear}]->(organization);
```

## Cypher Query
```bash
MATCH (person:Person)-[:STUDIED_AT]->(institution:Institution {nameofinstitution: "Smart National University of Vietnam"})
RETURN person.name, person.passportnumber;
```
