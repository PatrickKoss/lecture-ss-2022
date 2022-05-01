# Example
## Stations
```bash
CREATE (n: Station{name: "München Hbf", address: "München 1"});
CREATE (n: Station{name: "München Underground", address: "München 2"});
CREATE (n: Station{name: "Augsburg Hbf", address: "Augsburg 3"});
CREATE (n: Station{name: "Ulm Hbf", address: "Ulm 3"});
CREATE (n: Station{name: "Stuttgart Hbf", address: "Stuttgart 3"});
CREATE (n: Station{name: "Ludwigsburg Hbf", address: "Ludwigsburg 3"});
CREATE (n: Station{name: "Heilbronn Hbf", address: "Heilbronn 3"});
CREATE (n: Station{name: "Mannheim Hbf", address: "Mannheim 3"});
CREATE (n: Station{name: "Frankfurt Hbf", address: "Frankfurt 3"});
CREATE (n: Station{name: "Würzburg Hbf", address: "Würzburg 3"});
CREATE (n: Station{name: "Nürnberg Hbf", address: "Nürnberg 3"});
CREATE (n: Station{name: "Regensburg Hbf", address: "Regensburg 3"});
CREATE (n: Station{name: "Ingolstadt Hbf", address: "Ingolstadt 3"});
```

## Routes Between Stations
```bash
MATCH (s1: Station), (s2: Station) WHERE s1.name = "München Hbf" AND s2.name = "München Underground" CREATE (s1)-[:route]->(s2);
MATCH (s1: Station), (s2: Station) WHERE s2.name = "München Hbf" AND s1.name = "München Underground" CREATE (s1)-[:route]->(s2);
MATCH (s1: Station), (s2: Station) WHERE s1.name = "Augsburg Hbf" AND s2.name = "München Hbf" CREATE (s1)-[:route]->(s2);
MATCH (s1: Station), (s2: Station) WHERE s2.name = "Augsburg Hbf" AND s1.name = "München Hbf" CREATE (s1)-[:route]->(s2);
MATCH (s1: Station), (s2: Station) WHERE s1.name = "Augsburg Hbf" AND s2.name = "Ulm Hbf" CREATE (s1)-[:route]->(s2);
MATCH (s1: Station), (s2: Station) WHERE s2.name = "Augsburg Hbf" AND s1.name = "Ulm Hbf" CREATE (s1)-[:route]->(s2);
MATCH (s1: Station), (s2: Station) WHERE s1.name = "Stuttgart Hbf" AND s2.name = "Ulm Hbf" CREATE (s1)-[:route]->(s2);
MATCH (s1: Station), (s2: Station) WHERE s2.name = "Stuttgart Hbf" AND s1.name = "Ulm Hbf" CREATE (s1)-[:route]->(s2);
MATCH (s1: Station), (s2: Station) WHERE s1.name = "Stuttgart Hbf" AND s2.name = "Ludwigsburg Hbf" CREATE (s1)-[:route]->(s2);
MATCH (s1: Station), (s2: Station) WHERE s2.name = "Stuttgart Hbf" AND s1.name = "Ludwigsburg Hbf" CREATE (s1)-[:route]->(s2);
MATCH (s1: Station), (s2: Station) WHERE s1.name = "Heilbronn Hbf" AND s2.name = "Ludwigsburg Hbf" CREATE (s1)-[:route]->(s2);
MATCH (s1: Station), (s2: Station) WHERE s2.name = "Heilbronn Hbf" AND s1.name = "Ludwigsburg Hbf" CREATE (s1)-[:route]->(s2);
MATCH (s1: Station), (s2: Station) WHERE s1.name = "Stuttgart Hbf" AND s2.name = "Mannheim Hbf" CREATE (s1)-[:route]->(s2);
MATCH (s1: Station), (s2: Station) WHERE s2.name = "Stuttgart Hbf" AND s1.name = "Mannheim Hbf" CREATE (s1)-[:route]->(s2);
MATCH (s1: Station), (s2: Station) WHERE s1.name = "Frankfurt Hbf" AND s2.name = "Mannheim Hbf" CREATE (s1)-[:route]->(s2);
MATCH (s1: Station), (s2: Station) WHERE s2.name = "Frankfurt Hbf" AND s1.name = "Mannheim Hbf" CREATE (s1)-[:route]->(s2);
MATCH (s1: Station), (s2: Station) WHERE s1.name = "Frankfurt Hbf" AND s2.name = "Würzburg Hbf" CREATE (s1)-[:route]->(s2);
MATCH (s1: Station), (s2: Station) WHERE s2.name = "Frankfurt Hbf" AND s1.name = "Würzburg Hbf" CREATE (s1)-[:route]->(s2);
MATCH (s1: Station), (s2: Station) WHERE s1.name = "Heilbronn Hbf" AND s2.name = "Würzburg Hbf" CREATE (s1)-[:route]->(s2);
MATCH (s1: Station), (s2: Station) WHERE s2.name = "Heilbronn Hbf" AND s1.name = "Würzburg Hbf" CREATE (s1)-[:route]->(s2);
MATCH (s1: Station), (s2: Station) WHERE s1.name = "Nürnberg Hbf" AND s2.name = "Würzburg Hbf" CREATE (s1)-[:route]->(s2);
MATCH (s1: Station), (s2: Station) WHERE s2.name = "Nürnberg Hbf" AND s1.name = "Würzburg Hbf" CREATE (s1)-[:route]->(s2);
MATCH (s1: Station), (s2: Station) WHERE s1.name = "Nürnberg Hbf" AND s2.name = "Regensburg Hbf" CREATE (s1)-[:route]->(s2);
MATCH (s1: Station), (s2: Station) WHERE s2.name = "Nürnberg Hbf" AND s1.name = "Regensburg Hbf" CREATE (s1)-[:route]->(s2);
MATCH (s1: Station), (s2: Station) WHERE s1.name = "Nürnberg Hbf" AND s2.name = "Ingolstadt Hbf" CREATE (s1)-[:route]->(s2);
MATCH (s1: Station), (s2: Station) WHERE s2.name = "Nürnberg Hbf" AND s1.name = "Ingolstadt Hbf" CREATE (s1)-[:route]->(s2);
MATCH (s1: Station), (s2: Station) WHERE s1.name = "München Hbf" AND s2.name = "Ingolstadt Hbf" CREATE (s1)-[:route]->(s2);
MATCH (s1: Station), (s2: Station) WHERE s2.name = "München Hbf" AND s1.name = "Ingolstadt Hbf" CREATE (s1)-[:route]->(s2);
MATCH (s1: Station), (s2: Station) WHERE s1.name = "München Hbf" AND s2.name = "Regensburg Hbf" CREATE (s1)-[:route]->(s2);
MATCH (s1: Station), (s2: Station) WHERE s2.name = "München Hbf" AND s1.name = "Regensburg Hbf" CREATE (s1)-[:route]->(s2);
```

## Region Tickets
```bash
CREATE (n: RegionTicket{name: "Baden-Württemberg-Ticket"});
CREATE (n: RegionTicket{name: "Bayern-Ticket"});
```

## Region Tickets Allowed Stations
```bash
MATCH (s: Station), (t: RegionTicket) WHERE s.name = "München Hbf" AND t.name = "Bayern-Ticket" CREATE (t)-[:isAllowed]->(s);
MATCH (s: Station), (t: RegionTicket) WHERE s.name = "München Underground" AND t.name = "Bayern-Ticket" CREATE (t)-[:isAllowed]->(s);
MATCH (s: Station), (t: RegionTicket) WHERE s.name = "Augsburg Hbf" AND t.name = "Bayern-Ticket" CREATE (t)-[:isAllowed]->(s);
MATCH (s: Station), (t: RegionTicket) WHERE s.name = "Ulm Hbf" AND t.name = "Bayern-Ticket" CREATE (t)-[:isAllowed]->(s);
MATCH (s: Station), (t: RegionTicket) WHERE s.name = "Würzburg Hbf" AND t.name = "Bayern-Ticket" CREATE (t)-[:isAllowed]->(s);
MATCH (s: Station), (t: RegionTicket) WHERE s.name = "Regensburg Hbf" AND t.name = "Bayern-Ticket" CREATE (t)-[:isAllowed]->(s);
MATCH (s: Station), (t: RegionTicket) WHERE s.name = "Ingolstadt Hbf" AND t.name = "Bayern-Ticket" CREATE (t)-[:isAllowed]->(s);
MATCH (s: Station), (t: RegionTicket) WHERE s.name = "Stuttgart Hbf" AND t.name = "Baden-Württemberg-Ticket" CREATE (t)-[:isAllowed]->(s);
MATCH (s: Station), (t: RegionTicket) WHERE s.name = "Ludwigsburg Hbf" AND t.name = "Baden-Württemberg-Ticket" CREATE (t)-[:isAllowed]->(s);
MATCH (s: Station), (t: RegionTicket) WHERE s.name = "Heilbronn Hbf" AND t.name = "Baden-Württemberg-Ticket" CREATE (t)-[:isAllowed]->(s);
MATCH (s: Station), (t: RegionTicket) WHERE s.name = "Mannheim Hbf" AND t.name = "Baden-Württemberg-Ticket" CREATE (t)-[:isAllowed]->(s);
```

## Trains
```bash
CREATE (n: Train{name: "RB10"});
CREATE (n: Train{name: "RB11"});
CREATE (n: Train{name: "RB12"});
CREATE (n: Train{name: "ICE0"});
CREATE (n: Train{name: "ICE1"});
CREATE (n: Train{name: "ICE2"});
```

## Routes
```bash
CREATE (n: Route{name: "Stuttgart", startTime: datetime("2022-03-27T18:40:32.000+0100")});

CREATE (n: Route{name: "Stuttgart", startTime: datetime("2022-05-28T18:40:32.000+0100")});
CREATE (n: Route{name: "München", startTime: datetime("2022-05-28T20:50:32.000+0100")});
```

## Routes Trains Connection
```bash
MATCH (r: Route), (t: Train) WHERE r.name = "Stuttgart" AND t.name = "RB10" AND r.startTime = datetime("2022-03-27T18:40:32.000+0100") CREATE (r)-[:drivenBy]->(t);

MATCH (r: Route), (t: Train) WHERE r.name = "Stuttgart" AND t.name = "ICE0" AND r.startTime = datetime("2022-05-28T18:40:32.000+0100") CREATE (r)-[:drivenBy]->(t);
MATCH (r: Route), (t: Train) WHERE r.name = "München" AND t.name = "ICE1" AND r.startTime = datetime("2022-05-28T20:50:32.000+0100") CREATE (r)-[:drivenBy]->(t);
```

## Routes Stations Connection
```bash
MATCH (r: Route), (s: Station) WHERE r.name = "Stuttgart" AND s.name = "Stuttgart Hbf" CREATE (r)-[:stopsAt{timestamp: datetime("2022-03-27T18:40:32.000+0100")}]->(s);
MATCH (r: Route), (s: Station) WHERE r.name = "Stuttgart" AND s.name = "Mannheim Hbf" CREATE (r)-[:stopsAt{timestamp: datetime("2022-03-27T19:40:32.000+0100")}]->(s);

MATCH (r: Route), (s: Station) WHERE r.name = "Stuttgart" AND s.name = "Stuttgart Hbf" CREATE (r)-[:stopsAt{timestamp: datetime("2022-05-28T20:40:32.000+0100")}]->(s);
MATCH (r: Route), (s: Station) WHERE r.name = "Stuttgart" AND s.name = "Frankfurt Hbf" CREATE (r)-[:stopsAt{timestamp: datetime("2022-05-28T18:40:32.000+0100")}]->(s);
MATCH (r: Route), (s: Station) WHERE r.name = "Stuttgart" AND s.name = "Mannheim Hbf" CREATE (r)-[:stopsAt{timestamp: datetime("2022-05-28T19:40:32.000+0100")}]->(s);

MATCH (r: Route), (s: Station) WHERE r.name = "München" AND s.name = "München Hbf" CREATE (r)-[:stopsAt{timestamp: datetime("2022-05-28T23:50:32.000+0100")}]->(s);
MATCH (r: Route), (s: Station) WHERE r.name = "München" AND s.name = "Ulm Hbf" CREATE (r)-[:stopsAt{timestamp: datetime("2022-05-28T21:50:32.000+0100")}]->(s);
MATCH (r: Route), (s: Station) WHERE r.name = "München" AND s.name = "Augsburg Hbf" CREATE (r)-[:stopsAt{timestamp: datetime("2022-05-28T22:50:32.000+0100")}]->(s);
MATCH (r: Route), (s: Station) WHERE r.name = "München" AND s.name = "Stuttgart Hbf" CREATE (r)-[:stopsAt{timestamp: datetime("2022-05-28T20:50:32.000+0100")}]->(s);
```

## User
```bash
CREATE (n:User{name: "Patrick", email: "patrick@gmail.com"})
```