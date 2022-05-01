# Start
```bash
# wait until it started
docker-compose exec cassandra cqlsh
```

# Create Keyspace
```bash
CREATE KEYSPACE "spotify" WITH replication = {'class': 'SimpleStrategy', 'replication_factor' : '1'};
# use it
USE spotify;
```

# Create Datatypes
```bash
CREATE TYPE playlist (
  name text,
  description text
);
CREATE TYPE title (
  id UUID,
  name text,
  album text,
  interpreter text
);
```

# Create Tables
```bash
CREATE TABLE interpreter (
  name text,
  description text,
  PRIMARY KEY (name)
);
CREATE TABLE title (
  name text,
  interpreter text,
  lyrics text,
  id UUID,
  genre list<text>,
  album text,
  PRIMARY KEY (id)
);
CREATE TABLE playlist (
  name text,
  creator text,
  description text,
  picture text,
  titles list<frozen<title>>,
  PRIMARY KEY (name, creator)
) WITH CLUSTERING ORDER BY (creator DESC);
CREATE TABLE playlistfolder (
  name text,
  user text,
  playlists list<frozen<playlist>>,
  PRIMARY KEY (name, user)
) WITH CLUSTERING ORDER BY (user DESC);
CREATE TABLE user (
  name text,
  email text,
  premium boolean,
  PRIMARY KEY (name)
);
CREATE TABLE listenedtitles (
  user text,
  startdate timestamp,
  seconds double,
  title_id UUID,
  title_name text,
  album text,
  interpreter text,
  PRIMARY KEY ((title_id), user, startdate, seconds)
) WITH CLUSTERING ORDER BY (user DESC, startdate DESC, seconds DESC);
CREATE TABLE IF NOT EXISTS listenedtitles_aggregation (
  user text,
  title_id UUID,
  title_name text,
  album text,
  interpreter text,
  summed_seconds double,
  clicks int,
  week date,
  month date,
  PRIMARY KEY ((week, month), summed_seconds, clicks, title_id, user)
) WITH CLUSTERING ORDER BY (summed_seconds DESC, clicks DESC, title_id DESC, user DESC);
CREATE TABLE IF NOT EXISTS listenedtitles_aggregation_clicks (
  user text,
  title_id UUID,
  title_name text,
  album text,
  interpreter text,
  summed_seconds double,
  clicks int,
  week date,
  month date,
  PRIMARY KEY ((week, month), clicks, summed_seconds, title_id, user)
) WITH CLUSTERING ORDER BY (clicks DESC, summed_seconds DESC, title_id DESC, user DESC);
CREATE TABLE IF NOT EXISTS listenedtitles_aggregation_interpreter (
  user text,
  title_id UUID,
  title_name text,
  album text,
  interpreter text,
  summed_seconds double,
  clicks int,
  week date,
  month date,
  PRIMARY KEY ((interpreter), clicks, summed_seconds, title_id, user, week, month)
);
CREATE TABLE genre (
  name text,
  description text,
  PRIMARY KEY (name)
);
# show tables
describe tables;
```

# Insert some Data
## Interpreter
```bash
INSERT INTO interpreter (name, description)
VALUES ('gzuz', 'gzuz');
INSERT INTO interpreter (name, description)
VALUES ('kollegah', 'kollegah');
INSERT INTO interpreter (name, description)
VALUES ('187 Strassenbande', '187 Strassenbande');
INSERT INTO interpreter (name, description)
VALUES('farid bang', 'farid bang');
INSERT INTO interpreter (name, description)
VALUES ('kontra k', 'kontra k');
INSERT INTO interpreter (name, description)
VALUES ('bushido', 'bushido');
```

## Title
```bash
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Späti', 'gzuz', dc244eda-3a70-485e-a529-bde6bea2a687, ['rap'], 'Späti');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Kokain', 'gzuz', 1917a132-e5b4-4d15-b06b-a5754367d053, ['rap'], 'Palmen aus Plastik 2');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Donuts', 'gzuz', df8404bb-c7e8-452c-acf3-fd8c518c7234, ['rap'], 'Gzuz');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Riesenjoints', 'gzuz', 9c1f0615-e1f7-48d0-ac6e-39a46f886d68, ['rap'], 'Riesenjoints');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Mörder', 'gzuz', f7a131d0-47f6-479b-9cb9-52b469a8735f, ['rap'], 'Palmen aus Plastik');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('CL500', 'gzuz', ffc4720a-f9d2-4f1f-9f6f-b6a2c01e1226, ['rap'], 'Ebbe & Flut');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Kontrollieren', 'gzuz', b1791aa2-0c19-400a-911f-9dc17f8f78af, ['rap'], 'Kontrollieren');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Du bist Boss', 'kollegah', 8edec1a1-28d1-4890-a47f-764569162470, ['rap'], 'King');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('John Gotti', 'kollegah', d798addc-95e0-4198-bd1a-63be6ab3e82c, ['rap'], 'Zuhältertape 4');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Infinitum', 'kollegah', 17f63d19-f668-4ef6-bd66-21d0cc203d9a, ['rap'], 'Alphagene 2');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Bossaura', 'kollegah', dcad137b-db86-4932-849e-c82c3417bbbe, ['rap'], 'Bossaura');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('King', 'kollegah', bae55123-688e-4f1b-81ba-d36476b1362f, ['rap'], 'King');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Wie ein Alpha', 'kollegah', e9dc93d7-5bba-40f8-8f01-f261c7d99eaf, ['rap'], 'Wie ein Alpha');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Ave Maria', 'kollegah', ac4347ce-d41e-41a5-a9d4-7626d2171dc9, ['rap'], 'JBG 3');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Vielen Dank', '187 Strassenbande', ec23b63a-96d9-45f7-b1d0-60705cf3330f, ['rap'], 'Vielen Dank');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Alles nach Plan', '187 Strassenbande', 4daf4401-eb3f-4e7a-8547-35ee12e1c020, ['rap'], 'Sampler 5');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Extasy', '187 Strassenbande', 435dd893-dc48-4db1-bf47-ec8248478709, ['rap', 'pop'], 'Extasy');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Verpennt', '187 Strassenbande', 60092bbe-0d38-474f-a656-11746bab5895, ['rap', 'pop'], 'Verpennt');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Paradies', '187 Strassenbande', e039e41d-6295-425e-b269-281a05be2a48, ['rap', 'pop'], 'Paradies');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Ihr Hobby', '187 Strassenbande', e9679ef4-56c7-4618-9332-fc9fc3044d5d, ['rap', 'gangster rap'], 'Holloywood');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Mit den Jungz', '187 Strassenbande', acebc9a8-b784-4151-9897-1223b08ed341, ['rap', 'gangster rap'], 'Mit den Jungz');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('INTERNATIONAL GANGSTAS', 'farid bang', 07441d0a-3576-4113-a9ca-78bf3a7a95ae, ['deutsch rap'], 'INTERNATIONAL GANGSTAS');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('BA3T', 'farid bang', 021a3f55-692f-4825-aac9-e71a43432f73, ['deutsch rap'], 'BA3T');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Zieh den Rucksack aus', 'farid bang', 7b6a7340-624f-4b95-ad37-6b3d5c7468fd, ['deutsch rap'], 'Zieh den Rucksack aus');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('BITTE SPITTE X', 'farid bang', 6e626e82-929a-44cb-9aab-a5e4b3fcca99, ['deutsch rap'], 'X');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Scarface', 'farid bang', 0a5f3c1f-10c1-4c0a-bb58-b4807c1b7703, ['deutsch rap'], 'Genkidama');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Sturmmaske auf', 'farid bang', a6a0ddee-3848-4e2e-84f1-2657b32c1d60, ['deutsch rap'], 'JBG 3');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Bitcoins', 'farid bang', 05201070-2d7f-4bd4-9a7b-f5d33e43090f, ['deutsch rap'], '8');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Für den Himmel durch die Hölle', 'kontra k', f87a5419-d9ac-408a-b217-078bf4eb6c2f, ['legenden rap'], 'Für den Himmel durch die Hölle');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Erfolg ist kein Glück', 'kontra k', 027252d7-051d-41f0-9ff7-04ac48a9a806, ['legenden rap'], 'Aus dem Schatten ins Licht');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Social Media', 'kontra k', 0335129a-16d3-4470-ab78-384d2aaf5bb2, ['legenden rap'], 'Social Media');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Letzte Träne', 'kontra k', fcc8ec7f-bac1-4603-9fb1-c59cdc6d1ea1, ['legenden rap'], 'Sie wollten Wasser doch kriegen Benzin');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Fame', 'kontra k', 64f450f4-d171-4dc8-9873-88c32c429903, ['legenden rap'], 'Erde & Knochen');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Adrenalin', 'kontra k', 74e13267-4957-4660-984e-dd2bd7224c5b, ['legenden rap'], 'Wölfe');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Blei', 'kontra k', aeec32ff-411d-4aaf-93a5-3e69bfb68261, ['legenden rap'], 'Sie wollten Wasser doch kriegen Benzin');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Sterne', 'bushido', 0a1d7ed2-8f3c-4692-b4a3-c856e3f17a0a, ['oldschool rap'], 'FVCKB!TCHE$GETMONEY');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Alles wird gut', 'bushido', 034e0d10-8b59-4e2f-865e-bcf9de5d8900, ['oldschool rap'], 'Zeiten ändern dich');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Statements', 'bushido', 19a04770-0894-412b-b5e7-fc7e07e9a6ec, ['oldschool rap'], 'DREAMS');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Für immer jung', 'bushido', 4451b7d3-63f2-426f-84c1-ad52733f6053, ['oldschool rap'], 'Heavy Metal Payback');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Sonnenbank Flavor', 'bushido', 34983d76-a142-4713-af8b-8a08fecd14c7, ['oldschool rap'], 'Von der Skyline zum Bordstein zurück');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Zeiten ändern dich', 'bushido', 87e1d6bd-4714-4c0c-ba3e-6216b8632b46, ['oldschool rap'], 'Zeiten ändern dich');
INSERT INTO title (name, interpreter, id, genre, album)
VALUES ('Narben', 'bushido', 741db92a-05bf-4a0d-8d26-16a5d75994e2, ['oldschool rap'], 'Sonny Black 2');
```

## Playlists
```bash
INSERT INTO playlist (name, creator, titles)
VALUES ('Shishabar', 'patrick', [{id: dc244eda-3a70-485e-a529-bde6bea2a687, name: 'Späti', album: 'Späti', interpreter: 'gzuz'}, {id: e9dc93d7-5bba-40f8-8f01-f261c7d99eaf, name: 'Wie ein Alpha', album: 'Wie ein Alpha', interpreter: 'kollegah'}, {id: 034e0d10-8b59-4e2f-865e-bcf9de5d8900, name: 'Alles wird gut', album: 'Zeiten ändern dich', interpreter: 'bushido'}, {id: dc244eda-3a70-485e-a529-bde6bea2a687, name: 'Späti', album: 'Späti', interpreter: 'gzuz'}, {id: 027252d7-051d-41f0-9ff7-04ac48a9a806, name: 'Erfolg ist kein Glück', album: 'Aus dem Schatten ins Licht', interpreter: 'kontra k'}, {id: 7b6a7340-624f-4b95-ad37-6b3d5c7468fd, name: 'Zieh den Rucksack aus', album: 'Zieh den Rucksack aus', interpreter: 'farid bang'}, {id: 05201070-2d7f-4bd4-9a7b-f5d33e43090f, name: 'Bitcoins', album: '8', interpreter: 'farid bang'}]);
INSERT INTO playlist (name, creator, titles)
VALUES ('Deutsch Rap Royal', 'patrick', [{id: df8404bb-c7e8-452c-acf3-fd8c518c7234, name: 'Donuts', album: 'Gzuz', interpreter: 'gzuz'}, {id: 741db92a-05bf-4a0d-8d26-16a5d75994e2, name: 'Narben', album: 'Sonny Black 2', interpreter: 'bushido'}, {id: 87e1d6bd-4714-4c0c-ba3e-6216b8632b46, name: 'Zeiten ändern dich', album: 'Zeiten ändern dich', interpreter: 'bushido'}, {id: 34983d76-a142-4713-af8b-8a08fecd14c7, name: 'Sonnenbank Flavor', album: 'Von der Skyline zum Bordstein zurück', interpreter: 'bushido'}, {id: 17f63d19-f668-4ef6-bd66-21d0cc203d9a, name: 'Infinitum', album: 'Alphagene 2', interpreter: 'kollegah'}, {id: 9c1f0615-e1f7-48d0-ac6e-39a46f886d68, name: 'Riesenjoints', album: 'Riesenjoints', interpreter: 'gzuz'}, {id: b1791aa2-0c19-400a-911f-9dc17f8f78af, name: 'Kontrollieren', album: 'Kontrollieren', interpreter: 'gzuz'}]);
INSERT INTO playlist (name, creator, titles)
VALUES ('Rap', 'patrick', [{id: 07441d0a-3576-4113-a9ca-78bf3a7a95ae, name: 'INTERNATIONAL GANGSTAS', album: 'INTERNATIONAL GANGSTAS', interpreter: 'farid bang'}, {id: 74e13267-4957-4660-984e-dd2bd7224c5b, name: 'Adrenalin', album: 'Wölfe', interpreter: 'kontra k'}, {id: e039e41d-6295-425e-b269-281a05be2a48, name: 'Paradies', album: 'Paradies', interpreter: '187 Strassenbande'}, {id: dcad137b-db86-4932-849e-c82c3417bbbe, name: 'Bossaura', album: 'Bossaura', interpreter: 'kollegah'}, {id: d798addc-95e0-4198-bd1a-63be6ab3e82c, name: 'John Gotti', album: 'Zuhältertape 4', interpreter: 'kollegah'}, {id: f7a131d0-47f6-479b-9cb9-52b469a8735f, name: 'Mörder', album: 'Palmen aus Plastik', interpreter: 'gzuz'}, {id: 8edec1a1-28d1-4890-a47f-764569162470, name: 'Du bist Boss', album: 'King', interpreter: 'kollegah'}]);
```

## Insert Playlist of Playlists
```bash
INSERT INTO playlistfolder (name, user, playlists)
VALUES ('Deutsch Rap', 'patrick', [{name: 'Shishabar', description: 'shisha'}, {name: 'Rap', description: 'rap'}]);
```

## Insert User
```bash
INSERT INTO user (name, email, premium)
VALUES ('patrick', 'patrick@patrick.de', true);
INSERT INTO user (name, email, premium)
VALUES ('tobias', 'tobias@tobias.de', true);
INSERT INTO user (name, email, premium)
VALUES ('selina', 'selina@selina.de', false);
```

## Insert Listened Titles
```bash
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-10T08:59:39Z', 119.67, 'Infinitum', 'kollegah', 'Alpha Gene 2', 17f63d19-f668-4ef6-bd66-21d0cc203d9a);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('selina', '2022-04-13T03:50:05Z', 121.64, 'Riesenjoints', 'gzuz', 'Riesenjoints', 17f63d19-f668-4ef6-bd66-21d0cc203d9a);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('patrick', '2022-04-14T17:05:50Z', 85.88, 'Zieh den Rucksack aus', 'farid bang', 'Zieh den Rucksack aus', 7b6a7340-624f-4b95-ad37-6b3d5c7468fd);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-13T05:37:41Z', 24.5, 'Bitcoins', 'gzuz', '8', 05201070-2d7f-4bd4-9a7b-f5d33e43090f);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-11T08:02:32Z', 41.18, 'Vielen Dank', '187 Strassenbande', 'Vielen Dank', ec23b63a-96d9-45f7-b1d0-60705cf3330f);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('selina', '2022-04-13T11:34:51Z', 36.12, 'Zieh den Rucksack aus', 'farid bang', 'Zieh den Rucksack aus', 7b6a7340-624f-4b95-ad37-6b3d5c7468fd);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('patrick', '2022-04-08T13:23:42Z', 59.53, 'Zieh den Rucksack aus', 'farid bang', 'Zieh den Rucksack aus', 7b6a7340-624f-4b95-ad37-6b3d5c7468fd);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('patrick', '2022-04-16T19:05:46Z', 131.59, 'Statements', 'bushido', 'DREAMS', 19a04770-0894-412b-b5e7-fc7e07e9a6ec);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-16T02:32:33Z', 155.83, 'Ave Maria', 'kollegah', 'JBG 3', ac4347ce-d41e-41a5-a9d4-7626d2171dc9);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('patrick', '2022-04-13T08:27:58Z', 137.83, 'Ave Maria', 'kollegah', 'JBG 3', ac4347ce-d41e-41a5-a9d4-7626d2171dc9);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('selina', '2022-04-15T21:54:42Z', 57.98, 'Späti', 'gzuz', 'Späti', dc244eda-3a70-485e-a529-bde6bea2a687);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-12T07:38:28Z', 132.19, 'Narben', 'bushido', 'Sonny Black 2', 741db92a-05bf-4a0d-8d26-16a5d75994e2);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('selina', '2022-04-13T09:39:11Z', 95.57, 'Donuts', 'gzuz', 'Gzuz', df8404bb-c7e8-452c-acf3-fd8c518c7234);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('selina', '2022-04-12T15:12:53Z', 21.4, 'Späti', 'gzuz', 'Späti', dc244eda-3a70-485e-a529-bde6bea2a687);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-14T14:07:44Z', 22.19, 'Vielen Dank', '187 Strassenbande', 'Vielen Dank', ec23b63a-96d9-45f7-b1d0-60705cf3330f);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-12T10:26:45Z', 14.25, 'Donuts', 'gzuz', 'Gzuz', df8404bb-c7e8-452c-acf3-fd8c518c7234);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-10T00:30:43Z', 23.71, 'Vielen Dank', '187 Strassenbande', 'Vielen Dank', ec23b63a-96d9-45f7-b1d0-60705cf3330f);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-17T12:19:48Z', 127.73, 'Zieh den Rucksack aus', 'farid bang', 'Zieh den Rucksack aus', 7b6a7340-624f-4b95-ad37-6b3d5c7468fd);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('selina', '2022-04-16T03:03:08Z', 26.58, 'Späti', 'gzuz', 'Späti', dc244eda-3a70-485e-a529-bde6bea2a687);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('selina', '2022-04-14T13:20:28Z', 92.16, 'Zieh den Rucksack aus', 'farid bang', 'Zieh den Rucksack aus', 7b6a7340-624f-4b95-ad37-6b3d5c7468fd);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-10T00:15:31Z', 69.3, 'Riesenjoints', 'gzuz', 'Riesenjoints', 17f63d19-f668-4ef6-bd66-21d0cc203d9a);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('selina', '2022-04-09T07:42:06Z', 14.59, 'Erfolg ist kein Glück', 'kontra k', 'Aus dem Schatten ins Licht', 027252d7-051d-41f0-9ff7-04ac48a9a806);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('patrick', '2022-04-07T21:38:59Z', 39.18, 'Vielen Dank', '187 Strassenbande', 'Vielen Dank', ec23b63a-96d9-45f7-b1d0-60705cf3330f);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-14T01:03:18Z', 53.79, 'Erfolg ist kein Glück', 'kontra k', 'Aus dem Schatten ins Licht', 027252d7-051d-41f0-9ff7-04ac48a9a806);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-10T06:14:29Z', 38.42, 'Späti', 'gzuz', 'Späti', dc244eda-3a70-485e-a529-bde6bea2a687);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('selina', '2022-04-11T13:02:34Z', 111.64, 'Zeiten ändern dich', 'bushido', 'Zeiten ändern dich', 87e1d6bd-4714-4c0c-ba3e-6216b8632b46);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('selina', '2022-04-13T16:34:40Z', 53.53, 'Späti', 'gzuz', 'Späti', dc244eda-3a70-485e-a529-bde6bea2a687);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('selina', '2022-04-13T05:58:49Z', 121.84, 'Scarface', 'farid bang', 'Genkidama', 0a5f3c1f-10c1-4c0a-bb58-b4807c1b7703);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('patrick', '2022-04-08T13:24:53Z', 82.11, 'Scarface', 'farid bang', 'Genkidama', 0a5f3c1f-10c1-4c0a-bb58-b4807c1b7703);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('selina', '2022-04-16T09:45:36Z', 61.45, 'Erfolg ist kein Glück', 'kontra k', 'Aus dem Schatten ins Licht', 027252d7-051d-41f0-9ff7-04ac48a9a806);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('selina', '2022-04-10T12:57:40Z', 30.76, 'Statements', 'bushido', 'DREAMS', 19a04770-0894-412b-b5e7-fc7e07e9a6ec);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('patrick', '2022-04-10T12:45:01Z', 103.55, 'Donuts', 'gzuz', 'Gzuz', df8404bb-c7e8-452c-acf3-fd8c518c7234);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-15T15:51:56Z', 121.43, 'Ave Maria', 'kollegah', 'JBG 3', ac4347ce-d41e-41a5-a9d4-7626d2171dc9);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-16T23:42:53Z', 49.56, 'Erfolg ist kein Glück', 'kontra k', 'Aus dem Schatten ins Licht', 027252d7-051d-41f0-9ff7-04ac48a9a806);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-11T23:47:07Z', 44.36, 'Ave Maria', 'kollegah', 'JBG 3', ac4347ce-d41e-41a5-a9d4-7626d2171dc9);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('patrick', '2022-04-11T07:26:17Z', 143.8, 'Infinitum', 'kollegah', 'Alpha Gene 2', 17f63d19-f668-4ef6-bd66-21d0cc203d9a);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('patrick', '2022-04-14T16:45:34Z', 39.63, 'Alles wird gut', 'bushido', 'Zeiten ändern dich', 034e0d10-8b59-4e2f-865e-bcf9de5d8900);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('selina', '2022-04-14T10:27:51Z', 125.93, 'Alles wird gut', 'bushido', 'Zeiten ändern dich', 034e0d10-8b59-4e2f-865e-bcf9de5d8900);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('patrick', '2022-04-11T21:14:04Z', 139.58, 'Scarface', 'farid bang', 'Genkidama', 0a5f3c1f-10c1-4c0a-bb58-b4807c1b7703);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('selina', '2022-04-12T18:25:30Z', 125.91, 'Späti', 'gzuz', 'Späti', dc244eda-3a70-485e-a529-bde6bea2a687);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('selina', '2022-04-15T23:40:46Z', 91.28, 'Wie ein Alpha', 'kollegah', 'Wie ein Alpha', e9dc93d7-5bba-40f8-8f01-f261c7d99eaf);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('selina', '2022-04-09T11:12:53Z', 87.41, 'Donuts', 'gzuz', 'Gzuz', df8404bb-c7e8-452c-acf3-fd8c518c7234);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('patrick', '2022-04-17T09:46:25Z', 10.59, 'Donuts', 'gzuz', 'Gzuz', df8404bb-c7e8-452c-acf3-fd8c518c7234);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('selina', '2022-04-15T14:11:10Z', 14.43, 'Wie ein Alpha', 'kollegah', 'Wie ein Alpha', e9dc93d7-5bba-40f8-8f01-f261c7d99eaf);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('patrick', '2022-04-11T01:02:47Z', 78.9, 'Bitcoins', 'gzuz', '8', 05201070-2d7f-4bd4-9a7b-f5d33e43090f);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-14T07:49:29Z', 34.49, 'Riesenjoints', 'gzuz', 'Riesenjoints', 17f63d19-f668-4ef6-bd66-21d0cc203d9a);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-10T20:58:19Z', 23.88, 'Wie ein Alpha', 'kollegah', 'Wie ein Alpha', e9dc93d7-5bba-40f8-8f01-f261c7d99eaf);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('selina', '2022-04-10T10:08:34Z', 142.57, 'Ave Maria', 'kollegah', 'JBG 3', ac4347ce-d41e-41a5-a9d4-7626d2171dc9);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-11T22:35:02Z', 146.38, 'Statements', 'bushido', 'DREAMS', 19a04770-0894-412b-b5e7-fc7e07e9a6ec);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('patrick', '2022-04-08T23:58:32Z', 91.85, 'Wie ein Alpha', 'kollegah', 'Wie ein Alpha', e9dc93d7-5bba-40f8-8f01-f261c7d99eaf);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('selina', '2022-04-09T17:00:05Z', 148.7, 'Bitcoins', 'gzuz', '8', 05201070-2d7f-4bd4-9a7b-f5d33e43090f);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-09T08:34:26Z', 35.44, 'Riesenjoints', 'gzuz', 'Riesenjoints', 17f63d19-f668-4ef6-bd66-21d0cc203d9a);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-10T08:27:24Z', 97.98, 'Vielen Dank', '187 Strassenbande', 'Vielen Dank', ec23b63a-96d9-45f7-b1d0-60705cf3330f);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('selina', '2022-04-08T00:14:46Z', 125.5, 'Alles wird gut', 'bushido', 'Zeiten ändern dich', 034e0d10-8b59-4e2f-865e-bcf9de5d8900);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-08T16:50:03Z', 150.23, 'Scarface', 'farid bang', 'Genkidama', 0a5f3c1f-10c1-4c0a-bb58-b4807c1b7703);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('selina', '2022-04-12T05:29:08Z', 87.22, 'Bitcoins', 'gzuz', '8', 05201070-2d7f-4bd4-9a7b-f5d33e43090f);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('patrick', '2022-04-16T08:18:10Z', 154.25, 'Bitcoins', 'gzuz', '8', 05201070-2d7f-4bd4-9a7b-f5d33e43090f);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-14T20:27:21Z', 50.06, 'Vielen Dank', '187 Strassenbande', 'Vielen Dank', ec23b63a-96d9-45f7-b1d0-60705cf3330f);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('patrick', '2022-04-11T14:41:40Z', 135.84, 'Donuts', 'gzuz', 'Gzuz', df8404bb-c7e8-452c-acf3-fd8c518c7234);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-13T23:18:27Z', 58.97, 'Donuts', 'gzuz', 'Gzuz', df8404bb-c7e8-452c-acf3-fd8c518c7234);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-10T01:30:44Z', 108.34, 'Donuts', 'gzuz', 'Gzuz', df8404bb-c7e8-452c-acf3-fd8c518c7234);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('patrick', '2022-04-10T02:33:24Z', 99.79, 'Alles wird gut', 'bushido', 'Zeiten ändern dich', 034e0d10-8b59-4e2f-865e-bcf9de5d8900);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('patrick', '2022-04-12T09:37:46Z', 89.01, 'Ave Maria', 'kollegah', 'JBG 3', ac4347ce-d41e-41a5-a9d4-7626d2171dc9);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-11T04:10:19Z', 50.8, 'Vielen Dank', '187 Strassenbande', 'Vielen Dank', ec23b63a-96d9-45f7-b1d0-60705cf3330f);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('selina', '2022-04-09T19:11:46Z', 42.2, 'Scarface', 'farid bang', 'Genkidama', 0a5f3c1f-10c1-4c0a-bb58-b4807c1b7703);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('patrick', '2022-04-16T04:32:35Z', 86.34, 'Vielen Dank', '187 Strassenbande', 'Vielen Dank', ec23b63a-96d9-45f7-b1d0-60705cf3330f);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-08T06:18:04Z', 119.71, 'Wie ein Alpha', 'kollegah', 'Wie ein Alpha', e9dc93d7-5bba-40f8-8f01-f261c7d99eaf);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('selina', '2022-04-17T12:28:01Z', 68.43, 'Narben', 'bushido', 'Sonny Black 2', 741db92a-05bf-4a0d-8d26-16a5d75994e2);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('selina', '2022-04-09T08:07:55Z', 19.42, 'Riesenjoints', 'gzuz', 'Riesenjoints', 17f63d19-f668-4ef6-bd66-21d0cc203d9a);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-12T13:46:08Z', 68.02, 'Statements', 'bushido', 'DREAMS', 19a04770-0894-412b-b5e7-fc7e07e9a6ec);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-10T07:10:00Z', 30.41, 'Bitcoins', 'gzuz', '8', 05201070-2d7f-4bd4-9a7b-f5d33e43090f);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('patrick', '2022-04-08T08:44:58Z', 148.59, 'Bitcoins', 'gzuz', '8', 05201070-2d7f-4bd4-9a7b-f5d33e43090f);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('patrick', '2022-04-16T21:52:15Z', 33.06, 'Ave Maria', 'kollegah', 'JBG 3', ac4347ce-d41e-41a5-a9d4-7626d2171dc9);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('patrick', '2022-04-12T22:07:37Z', 70.54, 'Ave Maria', 'kollegah', 'JBG 3', ac4347ce-d41e-41a5-a9d4-7626d2171dc9);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-12T00:24:41Z', 64.71, 'Narben', 'bushido', 'Sonny Black 2', 741db92a-05bf-4a0d-8d26-16a5d75994e2);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-14T21:13:20Z', 119.65, 'Alles wird gut', 'bushido', 'Zeiten ändern dich', 034e0d10-8b59-4e2f-865e-bcf9de5d8900);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('patrick', '2022-04-17T06:35:54Z', 76.66, 'Alles wird gut', 'bushido', 'Zeiten ändern dich', 034e0d10-8b59-4e2f-865e-bcf9de5d8900);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-09T11:51:21Z', 110.83, 'Ave Maria', 'kollegah', 'JBG 3', ac4347ce-d41e-41a5-a9d4-7626d2171dc9);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('selina', '2022-04-15T19:42:26Z', 66.75, 'Narben', 'bushido', 'Sonny Black 2', 741db92a-05bf-4a0d-8d26-16a5d75994e2);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('patrick', '2022-04-10T10:21:58Z', 66.54, 'Ave Maria', 'kollegah', 'JBG 3', ac4347ce-d41e-41a5-a9d4-7626d2171dc9);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('selina', '2022-04-13T11:42:24Z', 115.4, 'Scarface', 'farid bang', 'Genkidama', 0a5f3c1f-10c1-4c0a-bb58-b4807c1b7703);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('patrick', '2022-04-11T17:33:22Z', 61.14, 'Zeiten ändern dich', 'bushido', 'Zeiten ändern dich', 87e1d6bd-4714-4c0c-ba3e-6216b8632b46);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('patrick', '2022-04-08T12:52:22Z', 65.23, 'Narben', 'bushido', 'Sonny Black 2', 741db92a-05bf-4a0d-8d26-16a5d75994e2);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-09T02:15:26Z', 118.49, 'Scarface', 'farid bang', 'Genkidama', 0a5f3c1f-10c1-4c0a-bb58-b4807c1b7703);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-11T12:00:53Z', 156.69, 'Erfolg ist kein Glück', 'kontra k', 'Aus dem Schatten ins Licht', 027252d7-051d-41f0-9ff7-04ac48a9a806);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-08T03:58:39Z', 32.52, 'Statements', 'bushido', 'DREAMS', 19a04770-0894-412b-b5e7-fc7e07e9a6ec);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('patrick', '2022-04-12T20:07:04Z', 104.56, 'Scarface', 'farid bang', 'Genkidama', 0a5f3c1f-10c1-4c0a-bb58-b4807c1b7703);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-12T04:50:47Z', 67.46, 'Zeiten ändern dich', 'bushido', 'Zeiten ändern dich', 87e1d6bd-4714-4c0c-ba3e-6216b8632b46);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-07T23:02:00Z', 81.09, 'Riesenjoints', 'gzuz', 'Riesenjoints', 17f63d19-f668-4ef6-bd66-21d0cc203d9a);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('patrick', '2022-04-15T18:07:17Z', 36.4, 'Späti', 'gzuz', 'Späti', dc244eda-3a70-485e-a529-bde6bea2a687);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('patrick', '2022-04-16T18:13:32Z', 60.85, 'Ave Maria', 'kollegah', 'JBG 3', ac4347ce-d41e-41a5-a9d4-7626d2171dc9);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('selina', '2022-04-16T11:05:52Z', 85.23, 'Zieh den Rucksack aus', 'farid bang', 'Zieh den Rucksack aus', 7b6a7340-624f-4b95-ad37-6b3d5c7468fd);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('patrick', '2022-04-17T00:53:27Z', 30.54, 'Bitcoins', 'gzuz', '8', 05201070-2d7f-4bd4-9a7b-f5d33e43090f);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('patrick', '2022-04-10T14:25:50Z', 151.73, 'Zeiten ändern dich', 'bushido', 'Zeiten ändern dich', 87e1d6bd-4714-4c0c-ba3e-6216b8632b46);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('patrick', '2022-04-14T23:24:27Z', 38.21, 'Ave Maria', 'kollegah', 'JBG 3', ac4347ce-d41e-41a5-a9d4-7626d2171dc9);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('patrick', '2022-04-09T08:46:43Z', 27.46, 'Wie ein Alpha', 'kollegah', 'Wie ein Alpha', e9dc93d7-5bba-40f8-8f01-f261c7d99eaf);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('selina', '2022-04-15T02:31:24Z', 154.66, 'Donuts', 'gzuz', 'Gzuz', df8404bb-c7e8-452c-acf3-fd8c518c7234);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('patrick', '2022-04-17T13:27:09Z', 16.52, 'Vielen Dank', '187 Strassenbande', 'Vielen Dank', ec23b63a-96d9-45f7-b1d0-60705cf3330f);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-13T13:08:59Z', 132.08, 'Späti', 'gzuz', 'Späti', dc244eda-3a70-485e-a529-bde6bea2a687);
INSERT INTO listenedtitles (user, startdate, seconds, title_name, interpreter, album, title_id)
VALUES ('tobias', '2022-04-08T09:00:18Z', 155.7, 'Ave Maria', 'kollegah', 'JBG 3', ac4347ce-d41e-41a5-a9d4-7626d2171dc9);
```

## Insert aggregation table
```bash
INSERT INTO listenedtitles_aggregation (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', e9dc93d7-5bba-40f8-8f01-f261c7d99eaf, 'Wie ein Alpha', 'Wie ein Alpha', 'kollegah', 119.31, 2, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', 034e0d10-8b59-4e2f-865e-bcf9de5d8900, 'Alles wird gut', 'Zeiten ändern dich', 'bushido', 216.08, 3, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', dc244eda-3a70-485e-a529-bde6bea2a687, 'Späti', 'Späti', 'gzuz', 36.4, 1, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', 7b6a7340-624f-4b95-ad37-6b3d5c7468fd, 'Zieh den Rucksack aus', 'Zieh den Rucksack aus', 'farid bang', 145.41, 2, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', 05201070-2d7f-4bd4-9a7b-f5d33e43090f, 'Bitcoins', '8', 'gzuz', 412.28, 4, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', df8404bb-c7e8-452c-acf3-fd8c518c7234, 'Donuts', 'Gzuz', 'gzuz', 249.98, 3, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', 741db92a-05bf-4a0d-8d26-16a5d75994e2, 'Narben', 'Sonny Black 2', 'bushido', 65.23, 1, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', 87e1d6bd-4714-4c0c-ba3e-6216b8632b46, 'Zeiten ändern dich', 'Zeiten ändern dich', 'bushido', 212.87, 2, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', 17f63d19-f668-4ef6-bd66-21d0cc203d9a, 'Infinitum', 'Alpha Gene 2', 'kollegah', 143.04, 1, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', ec23b63a-96d9-45f7-b1d0-60705cf3330f, 'Vielen Dank', 'Vielen Dank', '187 Strassenbande', 142.04, 3, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', 19a04770-0894-412b-b5e7-fc7e07e9a6ec, 'Statements', 'DREAMS', 'bushido', 131.59, 1, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', 0a5f3c1f-10c1-4c0a-bb58-b4807c1b7703, 'Scarface', 'Genkidama', 'farid bang', 326.25, 3, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', ac4347ce-d41e-41a5-a9d4-7626d2171dc9, 'Ave Maria', 'JBG 3', 'kollegah', 496.04, 7, '2022-04-01', '2022-04-24');

INSERT INTO listenedtitles_aggregation (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', e9dc93d7-5bba-40f8-8f01-f261c7d99eaf, 'Wie ein Alpha', 'Wie ein Alpha', 'kollegah', 143.59, 2, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', 034e0d10-8b59-4e2f-865e-bcf9de5d8900, 'Alles wird gut', 'Zeiten ändern dich', 'bushido', 119.65, 1, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', dc244eda-3a70-485e-a529-bde6bea2a687, 'Späti', 'Späti', 'gzuz', 170.50, 2, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', 7b6a7340-624f-4b95-ad37-6b3d5c7468fd, 'Zieh den Rucksack aus', 'Zieh den Rucksack aus', 'farid bang', 127.73, 1, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', 05201070-2d7f-4bd4-9a7b-f5d33e43090f, 'Bitcoins', '8', 'gzuz', 54.91, 2, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', df8404bb-c7e8-452c-acf3-fd8c518c7234, 'Donuts', 'Gzuz', 'gzuz', 181.56, 3, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', 741db92a-05bf-4a0d-8d26-16a5d75994e2, 'Narben', 'Sonny Black 2', 'bushido', 196.90, 2, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', 87e1d6bd-4714-4c0c-ba3e-6216b8632b46, 'Zeiten ändern dich', 'Zeiten ändern dich', 'bushido', 67.46, 5, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', ec23b63a-96d9-45f7-b1d0-60705cf3330f, 'Vielen Dank', 'Vielen Dank', '187 Strassenbande', 285.92, 6, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', 19a04770-0894-412b-b5e7-fc7e07e9a6ec, 'Statements', 'DREAMS', 'bushido', 246.92, 3, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', 0a5f3c1f-10c1-4c0a-bb58-b4807c1b7703, 'Scarface', 'Genkidama', 'farid bang', 268.72, 2, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', ac4347ce-d41e-41a5-a9d4-7626d2171dc9, 'Ave Maria', 'JBG 3', 'kollegah', 588.15, 5, '2022-04-01', '2022-04-24');

INSERT INTO listenedtitles_aggregation_clicks (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', e9dc93d7-5bba-40f8-8f01-f261c7d99eaf, 'Wie ein Alpha', 'Wie ein Alpha', 'kollegah', 119.31, 2, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_clicks (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', 034e0d10-8b59-4e2f-865e-bcf9de5d8900, 'Alles wird gut', 'Zeiten ändern dich', 'bushido', 216.08, 3, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_clicks (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', dc244eda-3a70-485e-a529-bde6bea2a687, 'Späti', 'Späti', 'gzuz', 36.4, 1, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_clicks (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', 7b6a7340-624f-4b95-ad37-6b3d5c7468fd, 'Zieh den Rucksack aus', 'Zieh den Rucksack aus', 'farid bang', 145.41, 2, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_clicks (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', 05201070-2d7f-4bd4-9a7b-f5d33e43090f, 'Bitcoins', '8', 'gzuz', 412.28, 4, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_clicks (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', df8404bb-c7e8-452c-acf3-fd8c518c7234, 'Donuts', 'Gzuz', 'gzuz', 249.98, 3, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_clicks (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', 741db92a-05bf-4a0d-8d26-16a5d75994e2, 'Narben', 'Sonny Black 2', 'bushido', 65.23, 1, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_clicks (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', 87e1d6bd-4714-4c0c-ba3e-6216b8632b46, 'Zeiten ändern dich', 'Zeiten ändern dich', 'bushido', 212.87, 2, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_clicks (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', 17f63d19-f668-4ef6-bd66-21d0cc203d9a, 'Infinitum', 'Alpha Gene 2', 'kollegah', 143.04, 1, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_clicks (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', ec23b63a-96d9-45f7-b1d0-60705cf3330f, 'Vielen Dank', 'Vielen Dank', '187 Strassenbande', 142.04, 3, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_clicks (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', 19a04770-0894-412b-b5e7-fc7e07e9a6ec, 'Statements', 'DREAMS', 'bushido', 131.59, 1, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_clicks (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', 0a5f3c1f-10c1-4c0a-bb58-b4807c1b7703, 'Scarface', 'Genkidama', 'farid bang', 326.25, 3, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_clicks (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', ac4347ce-d41e-41a5-a9d4-7626d2171dc9, 'Ave Maria', 'JBG 3', 'kollegah', 496.04, 7, '2022-04-01', '2022-04-24');

INSERT INTO listenedtitles_aggregation_clicks (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', e9dc93d7-5bba-40f8-8f01-f261c7d99eaf, 'Wie ein Alpha', 'Wie ein Alpha', 'kollegah', 143.59, 2, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_clicks (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', 034e0d10-8b59-4e2f-865e-bcf9de5d8900, 'Alles wird gut', 'Zeiten ändern dich', 'bushido', 119.65, 1, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_clicks (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', dc244eda-3a70-485e-a529-bde6bea2a687, 'Späti', 'Späti', 'gzuz', 170.50, 2, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_clicks (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', 7b6a7340-624f-4b95-ad37-6b3d5c7468fd, 'Zieh den Rucksack aus', 'Zieh den Rucksack aus', 'farid bang', 127.73, 1, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_clicks (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', 05201070-2d7f-4bd4-9a7b-f5d33e43090f, 'Bitcoins', '8', 'gzuz', 54.91, 2, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_clicks (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', df8404bb-c7e8-452c-acf3-fd8c518c7234, 'Donuts', 'Gzuz', 'gzuz', 181.56, 3, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_clicks (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', 741db92a-05bf-4a0d-8d26-16a5d75994e2, 'Narben', 'Sonny Black 2', 'bushido', 196.90, 2, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_clicks (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', 87e1d6bd-4714-4c0c-ba3e-6216b8632b46, 'Zeiten ändern dich', 'Zeiten ändern dich', 'bushido', 67.46, 5, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_clicks (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', ec23b63a-96d9-45f7-b1d0-60705cf3330f, 'Vielen Dank', 'Vielen Dank', '187 Strassenbande', 285.92, 6, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_clicks (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', 19a04770-0894-412b-b5e7-fc7e07e9a6ec, 'Statements', 'DREAMS', 'bushido', 246.92, 3, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_clicks (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', 0a5f3c1f-10c1-4c0a-bb58-b4807c1b7703, 'Scarface', 'Genkidama', 'farid bang', 268.72, 2, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_clicks (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', ac4347ce-d41e-41a5-a9d4-7626d2171dc9, 'Ave Maria', 'JBG 3', 'kollegah', 588.15, 5, '2022-04-01', '2022-04-24');

INSERT INTO listenedtitles_aggregation_interpreter (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', e9dc93d7-5bba-40f8-8f01-f261c7d99eaf, 'Wie ein Alpha', 'Wie ein Alpha', 'kollegah', 119.31, 2, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_interpreter (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', 034e0d10-8b59-4e2f-865e-bcf9de5d8900, 'Alles wird gut', 'Zeiten ändern dich', 'bushido', 216.08, 3, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_interpreter (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', dc244eda-3a70-485e-a529-bde6bea2a687, 'Späti', 'Späti', 'gzuz', 36.4, 1, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_interpreter (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', 7b6a7340-624f-4b95-ad37-6b3d5c7468fd, 'Zieh den Rucksack aus', 'Zieh den Rucksack aus', 'farid bang', 145.41, 2, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_interpreter (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', 05201070-2d7f-4bd4-9a7b-f5d33e43090f, 'Bitcoins', '8', 'gzuz', 412.28, 4, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_interpreter (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', df8404bb-c7e8-452c-acf3-fd8c518c7234, 'Donuts', 'Gzuz', 'gzuz', 249.98, 3, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_interpreter (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', 741db92a-05bf-4a0d-8d26-16a5d75994e2, 'Narben', 'Sonny Black 2', 'bushido', 65.23, 1, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_interpreter (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', 87e1d6bd-4714-4c0c-ba3e-6216b8632b46, 'Zeiten ändern dich', 'Zeiten ändern dich', 'bushido', 212.87, 2, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_interpreter (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', 17f63d19-f668-4ef6-bd66-21d0cc203d9a, 'Infinitum', 'Alpha Gene 2', 'kollegah', 143.04, 1, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_interpreter (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', ec23b63a-96d9-45f7-b1d0-60705cf3330f, 'Vielen Dank', 'Vielen Dank', '187 Strassenbande', 142.04, 3, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_interpreter (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', 19a04770-0894-412b-b5e7-fc7e07e9a6ec, 'Statements', 'DREAMS', 'bushido', 131.59, 1, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_interpreter (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', 0a5f3c1f-10c1-4c0a-bb58-b4807c1b7703, 'Scarface', 'Genkidama', 'farid bang', 326.25, 3, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_interpreter (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('patrick', ac4347ce-d41e-41a5-a9d4-7626d2171dc9, 'Ave Maria', 'JBG 3', 'kollegah', 496.04, 7, '2022-04-01', '2022-04-24');

INSERT INTO listenedtitles_aggregation_interpreter (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', e9dc93d7-5bba-40f8-8f01-f261c7d99eaf, 'Wie ein Alpha', 'Wie ein Alpha', 'kollegah', 143.59, 2, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_interpreter (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', 034e0d10-8b59-4e2f-865e-bcf9de5d8900, 'Alles wird gut', 'Zeiten ändern dich', 'bushido', 119.65, 1, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_interpreter (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', dc244eda-3a70-485e-a529-bde6bea2a687, 'Späti', 'Späti', 'gzuz', 170.50, 2, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_interpreter (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', 7b6a7340-624f-4b95-ad37-6b3d5c7468fd, 'Zieh den Rucksack aus', 'Zieh den Rucksack aus', 'farid bang', 127.73, 1, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_interpreter (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', 05201070-2d7f-4bd4-9a7b-f5d33e43090f, 'Bitcoins', '8', 'gzuz', 54.91, 2, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_interpreter (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', df8404bb-c7e8-452c-acf3-fd8c518c7234, 'Donuts', 'Gzuz', 'gzuz', 181.56, 3, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_interpreter (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', 741db92a-05bf-4a0d-8d26-16a5d75994e2, 'Narben', 'Sonny Black 2', 'bushido', 196.90, 2, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_interpreter (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', 87e1d6bd-4714-4c0c-ba3e-6216b8632b46, 'Zeiten ändern dich', 'Zeiten ändern dich', 'bushido', 67.46, 5, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_interpreter (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', ec23b63a-96d9-45f7-b1d0-60705cf3330f, 'Vielen Dank', 'Vielen Dank', '187 Strassenbande', 285.92, 6, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_interpreter (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', 19a04770-0894-412b-b5e7-fc7e07e9a6ec, 'Statements', 'DREAMS', 'bushido', 246.92, 3, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_interpreter (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', 0a5f3c1f-10c1-4c0a-bb58-b4807c1b7703, 'Scarface', 'Genkidama', 'farid bang', 268.72, 2, '2022-04-01', '2022-04-24');
INSERT INTO listenedtitles_aggregation_interpreter (user, title_id, title_name, album, interpreter, summed_seconds, clicks, week, month)
VALUES ('tobias', ac4347ce-d41e-41a5-a9d4-7626d2171dc9, 'Ave Maria', 'JBG 3', 'kollegah', 588.15, 5, '2022-04-01', '2022-04-24');
```