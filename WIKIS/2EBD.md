# EBD: Database Specification Component
> To facilitate events’ management and choices for people by making them organized, simple, and intuitive.

## A4: Conceptual Data Model

> This artefact contains the identification and description of the entities of the domain and the relationships between them, in a UML class diagram.

### 1. Class diagram

> The UML diagram in Figure 1 presents the main entities, the relationships between them, attributes and their domains, and the multiplicity of relationships for WeMeet.

![](https://i.imgur.com/ZZqJgpr.png)

*Figure 1: WeMeet's class diagram*

### 2. Additional Business Rules
 
* BR01.  Authenticated users can add messages to public event forums. 
* BR02. Only attendees of a private event can add messages to its private event forum.
* BR03. Users cannot vote their own message.
* BR04. A user can only receive a single invite from a specific event.
* BR05. A message must have text or a file.
* BR06. Messages only have one level of answers - replies. 
* BR07. Only event organizers can create and edit polls. 
* BR08. Only admins can manage reports.

---


## A5: Relational Schema, validation and schema refinement

> This artefact contains the Relational Schema obtained from the Conceptual Data Model presented in the previous artefact. It also contains the Schema's validation.

### 1. Relational Schema


| Relation reference | Relation Compact Notation |
| --- | --- |
 R01   | user(<ins>id</ins>, username __UK NN__, password __NN__, email __UK NN__, picture, isBlocked, isAdmin __DF__ False) |
 R02   | event(<ins>idEvent</ins>, title __NN__, description, visibility __NN__, picture, local __NN__, publish_date __NN__, start_date __NN__, final_date __NN__)|
R03    | attendee(<ins>id</ins> -> user __NN__, <ins>idEvent</ins> -> event __NN__)
R04    | tag(<ins>idTag</ins>, tagName __UK NN__)
R05    | event_tag(<ins>idTag</ins> -> tag __NN__, <ins>idEvent</ins> -> event __NN__)
R06    | event_organizer(<ins>id</ins> -> user __NN__, <ins>idEvent</ins> -> Event __NN__)
R07    | report(<ins>idReport</ins>, idEvent -> event __NN__, idManager -> user, idReporter -> user __NN__, date __NN__, motive __NN__, state __NN__ __DF__ 'Pending' __CK__ state __IN__ ReportState)
R08    | invite(<ins>idEvent</ins> -> event __NN__, <ins>idInvitee</ins> -> user __NN__, idOrganizer -> user __NN__, accepted)
R09    | poll(<ins>idPoll</ins>, title __NN__, description,  date __NN__, isOpen __NN DF__ True, idEvent -> event __NN__, id -> user __NN__)
R10    | option(<ins>idOption</ins>, text __NN__, idPoll -> poll __NN__)
R11    | choose_option(<ins>id</ins> -> user __NN__, <ins>idOption</ins> -> option __NN__)
R12    | message(<ins>idMessage</ins>, text, date __NN__, likeCount __NN DF__ 0, idEvent -> event __NN__, id -> user, <ins>parent</ins> -> message)
R13    | message_file(<ins>idFile</ins>, file, idMessage -> message __NN__)
R14    | vote(<ins>id</ins> -> user __NN__, <ins>idMessage</ins> -> message __NN__)
R15    | notification(<ins>idNotif</ins>, text __NN__, date __NN__, read __NN DF__ False, id -> user __NN__, type __NN CK__ type __IN__ NotificationTypes, idReport -> report __CK__ (type == 'Report'), (idEvent, idInvitee) -> invite __CK__ (type == 'Invite'), idMessage -> message __CK__ (type == 'Message'))


Legend:
* UK = UNIQUE KEY
* NN = NOT NULL
* DF = DEFAULT
* CK = CHECK


### 2. Domains

| Domain Name | Domain Specification           |
| ----------- | ------------------------------ |
| Report State | ENUM ('Pending', 'Rejected', 'Banned') |
| Notification Types | ENUM ('Invite', 'Message', 'Report')|

### 3. Schema validation

| **TABLE R01**                | user |
| ---------------------------- |-------------------
| **Keys**                     | {id}, {username}, {email}                              |
| **Functional Dependencies:** |                                                        |
| FD0101                       | id → {username, password, email, picture, isBlocked, isAdmin} |
| FD0102                       | username → {id, password, email, picture, isBlocked, isAdmin} |
| FD0103                       | email → {id, username, password, picture, isBlocked, isAdmin}  |
| **NORMAL FORM**              | BCNF               |


| **TABLE R02**                | event          |
| --------------               | -------------- |
| **Keys**                     | {idEvent}      |
| **Functional Dependencies:** |                |
| FD0201                       | idEvent → {title, description, visibility, picture, local, publish_date, start_date, final_date} |
| **NORMAL FORM**              |        BCNF        |

| **TABLE R03**                   | attendee             |
| --------------                  | --------------     |
| **Keys**                        | {id, idEvent}    |
| **Functional Dependencies:**    | none               |
| **NORMAL FORM**                 |       BCNF         |

| **TABLE R04**                   | tag             |
| --------------                  | --------------     |
| **Keys**                        | {idTag}, {tagName}   |
| **Functional Dependencies:**    |            |
| FD0401                          | idTag → {tagName}  |
| FD0402                          | tagName → {idTag}  |
| **NORMAL FORM**                 |     BCNF      |



| **TABLE R05**                   | event_tag         |
| --------------                  | ---                    |
| **Keys**                        | {idTag, idEvent}  |
| **Functional Dependencies:**    | none                   |
| **NORMAL FORM**                 |    BCNF                |

| **TABLE R06**                   | event_organizer         |
| --------------                  | ---                    |
| **Keys**                        | {id, idEvent}  |
| **Functional Dependencies:**    | none                   |
| **NORMAL FORM**                 |       BCNF             |

| **TABLE R07**                   | report                 |
| --------------                  | --------------         |
| **Keys**                        | {idReport}             |
| **Functional Dependencies:**    |                        |
| FD0701                          | idReport → {idEvent, idManager, idReporter, date, motive, state} |
| **NORMAL FORM**                 |                    BCNF    | 

| **TABLE R08**                   | invite             |
| --------------                  | --------------     |
| **Keys**                        | {idEvent, idInvitee}         |
| **Functional Dependencies:**    |                    |
| FD0801                          | {idEvent, idInvitee} → {idOrganizer, accepted} |
| **NORMAL FORM**                 |     BCNF               |

| **TABLE R09**                   | poll               |
| --------------                  | ---                |
| **Keys**                        | {idPoll}           |
| **Functional Dependencies:**    |                    |
| FD0901                          | idPoll → {title, description, date, isOpen, idEvent, id} |
| **NORMAL FORM**                 | BCNF               |

| **TABLE R10**                   | option             |
| --------------                  | ---                |
| **Keys**                        | {idOption}               |
| **Functional Dependencies:**    |                    |
| FD1001                          | idOption -> {text, idPoll}            |
| **NORMAL FORM**                 | BCNF                |

| **TABLE R11**                   | choose_option      |
| --------------                  | ---                |
| **Keys**                        | {id, idOption}               |
| **Functional Dependencies:**    |  none      |
| **NORMAL FORM**                 | BCNF               |

| **TABLE R12**                   | message            |
| --------------                  | ---                |
| **Keys**                        | {idMessage}               |
| **Functional Dependencies:**    |                    |
| FD1201                          | idMessage -> {text, date, likeCount, idEvent, parent}|
| **NORMAL FORM**                 |  BCNF              |

| **TABLE R13**                   | message_file            |
| --------------                  | ---                |
| **Keys**                        | {idFile}              |
| **Functional Dependencies:**    |                    |
| FD1201                          | idFile -> {file, idMessage}|
| **NORMAL FORM**                 | BCNF               |

| **TABLE R14**   | vote      |
| --------------  | ---                |
| **Keys**        | {id, idMessage}  |
| **Functional Dependencies:** | none      |
| **NORMAL FORM** | BCNF               |

| **TABLE R15**   | notification      |
| --------------  | ---                |
| **Keys**        | {idNotif}  |
| **Functional Dependencies:** |       |
| FD1401          | idNotif -> {text, date, read, id, type, idReport, (idEvent, IdInvitee), idMessage} |
| **NORMAL FORM** | BCNF               |


> There is no need for normalisation, since the relations are in BCNF (Boyce–Codd Normal Form), which makes the schema also BCNF.

---


## A6: Indexes, triggers, transactions and database population

> This artefact contains the database's workload, the physical schema of the database, its indexes, its triggers, and transactions needed to assure the integrity of the data. This artefact also contains the complete database creation script, including indexes and triggers.

### 1. Database Workload
 
#### Tuple Estimation:

| **Relation reference** | **Relation Name** | **Order of magnitude**        | **Estimated growth** |
| ------------------ | -------------             | ------------------------- | -------- |
| R01                | user        | dozens of thousands | dozens per day |
| R02                | event                      | thousands           | dozens per day|
| R03                | attendee                    | dozens of thousands| dozens per day |
| R04                | tag                   | dozens | units per month |
| R05                | event_tag            | thousands | dozens per day |
| R06                | event_organizer            | thousands | dozens per day |
| R07                | report                    | hundreds | dozens per year |
| R08               | invite                       | thousands | dozens per day |
| R09                | poll                        | thousands | dozens per day |
| R10                | option                        | thousands | dozens per day |
| R11                | choose_option                | units|dozens|hundreds|etc | no growth |
| R12                | message                    | thousands | dozens per day |
| R13                | message_file                    | thousands | dozens per day |
| R14                | vote                        | thousands | dozens per day |
| R15                | notification                | hundreds | units per day |



### 2. Proposed Indices

#### 2.1. Performance Indexes
 

| **Index**           | IDX01                                  |
| ---                 | ---                                    |
| **Relation**        |   poll  |
| **Attribute**       | idEvent   |
| **Type**            | Hash            |
| **Cardinality**     | Medium |
| **Clustering**      | No   |
| **Justification**   |  Obtaining all polls of an event is a frequent request. However, expected update frequency is medium, so no clustering is proposed. Cardinality is medium because idEvent can be repeated. |
| `SQL code`          |`CREATE INDEX event_poll ON poll USING hash (idEvent);` |

| **Index**           | IDX02                  |
| ---                 | ---                 |
| **Relation**        | message  |
| **Attribute**       | date   |
| **Type**            | B-tree |
| **Cardinality**     | Medium |
| **Clustering**      |  No    |
| **Justification**   | Table message is frequently accessed for messages of an event. A b-tree index allows faster ordenation by date.   |
| `SQL code`          |`CREATE INDEX date_message ON message USING btree (date);`|


| **Index**           | IDX03                  |
| ---                 | ---                 |
| **Relation**        | tag  |
| **Attribute**       | tagName   |
| **Type**            | B-tree |
| **Cardinality**     | High |
| **Clustering**      | No |
| **Justification**   | Update frequency is low and cardinality is high because tagName is unique. The index type is B-tree because it allows for faster queries based on the alphabetic order.   |
| `SQL code`          |`CREATE INDEX tag_alphabetic ON tag USING btree (tagName);`|


#### 2.2. Full-text Search Indices 

<table>
	<tbody>
        <tr>
            <th> Index </th>
            <th> IDX11 </th>
	    </tr>
        <tr>
            <th> Relation </th>
            <td> event </td>
        </tr>
        <tr>
            <th> Attribute </th>
            <td> title, description</td>
        </tr>
        <tr>
            <th> Type </th>
            <td> GIN </td>
        </tr>
        <tr>
            <th> Clustering </th>
            <td> No </td>
        </tr>
        <tr>
            <th> Justification </th>
            <td> Used for improving the performance of full text search, while searching for events by its title and description. The index type is GIN because this attributes can not be changed.  </td>
        </tr>
            <td colspan="2"><pre>
ALTER TABLE event ADD COLUMN tsvectors TSVECTOR;
<br>
CREATE OR REPLACE FUNCTION event_search_update() RETURNS TRIGGER AS $$
BEGIN
 IF TG_OP = 'INSERT' THEN
        NEW.tsvectors = (
         setweight(to_tsvector('english', NEW.title), 'A') ||
         setweight(to_tsvector('english', NEW.description), 'B')
        );
 END IF;
 IF TG_OP = 'UPDATE' THEN
         IF (NEW.title <> OLD.title OR NEW.description <> OLD.description) THEN
           NEW.tsvectors = (
             setweight(to_tsvector('english', NEW.title), 'A') ||
             setweight(to_tsvector('english', NEW.description), 'B')
           );
         END IF;
 END IF;
 RETURN NEW;
END $$
LANGUAGE plpgsql;
<br>
CREATE TRIGGER event_search_update
 BEFORE INSERT OR UPDATE ON event
 FOR EACH ROW
 EXECUTE PROCEDURE event_search_update();
<br>
 CREATE INDEX search_idx ON event USING GIN (tsvectors);        </tr>
    </tbody>
</table>



### 3. Triggers

<table>
	<tbody>
        <tr>
            <th> Trigger </th>
            <th> TRIGGER01 </th>
	    </tr>
        <tr>
            <th> Description </th>
            <td> An event needs to have at least one organizer </td>
        </tr>
        <tr>
            <td colspan="2"><pre>
CREATE OR REPLACE FUNCTION check_event_organizer() RETURNS TRIGGER AS
$BODY$
BEGIN
    IF OLD.id IN (SELECT id from event_organizer WHERE idevent = OLD.idEvent) and (select count(*) from event_organizer WHERE idevent = OLD.idEvent) = 1 AND (SELECT COUNT(*) FROM attendee WHERE idevent = OLD.idEvent) > 1
    THEN
        RAISE EXCEPTION 'Event with attendees must have at least one event organizer!';
    END IF;
    RETURN OLD;
END;
$BODY$
    LANGUAGE plpgsql;
<br>
DROP TRIGGER IF EXISTS check_event_organizer on attendee CASCADE;
CREATE TRIGGER check_event_organizer
    BEFORE DELETE ON attendee
    FOR EACH ROW
EXECUTE PROCEDURE check_event_organizer();
        </tr>
    </tbody>
</table>



<table>
	<tbody>
        <tr>
            <th> Trigger </th>
            <th> TRIGGER02 </th>
	    </tr>
        <tr>
            <th> Description </th>
            <td> A notification is added when a user is invited to an event </td>
        </tr>
        <tr>
            <td colspan="2"><pre>
CREATE OR REPLACE FUNCTION add_invite_notification() RETURNS TRIGGER AS
$BODY$
BEGIN
INSERT INTO notification(text, date, id, type, idinvitee, idevent)
    VALUES (concat('You have been invited to a new event: ', (select title from event where event.idevent = new.idevent)) , now(),  NEW.idinvitee, 'Invite', NEW.idinvitee, NEW.idevent);
    RETURN NEW;
END;
$BODY$
    LANGUAGE plpgsql;
<br>
DROP TRIGGER IF EXISTS add_invite_notification on invite CASCADE;
CREATE TRIGGER add_invite_notification
    AFTER INSERT
    ON invite
    FOR EACH ROW
EXECUTE PROCEDURE add_invite_notification(); 
</tr>
    </tbody>
</table>

<table>
	<tbody>
        <tr>
            <th> Trigger </th>
            <th> TRIGGER03 </th>
	    </tr>
        <tr>
            <th> Description </th>
            <td> A notification is added when a user adds a message on the event's forum </td>
        </tr>
        <tr>
            <td colspan="2"><pre>
CREATE OR REPLACE FUNCTION add_message_notification() RETURNS TRIGGER AS
$BODY$
BEGIN
    INSERT INTO notification(text, date, id, type, idmessage)
    VALUES (concat('New notification: ', NEW.text), NEW.date, 
            (SELECT event_organizer.id FROM event_Organizer WHERE event_Organizer.idEvent = NEW.idEvent), 'Message', NEW.idmessage);
    RETURN NEW;
END;
$BODY$
    LANGUAGE plpgsql;
<br>
DROP TRIGGER IF EXISTS add_message_notification on message CASCADE;
CREATE TRIGGER add_message_notification
AFTER INSERT ON message
FOR EACH ROW
EXECUTE PROCEDURE add_message_notification();
        </tr>
    </tbody>
</table>

<table>
	<tbody>
        <tr>
            <th> Trigger </th>
            <th> TRIGGER04 </th>
	    </tr>
        <tr>
            <th> Description </th>
            <td> A notification to an admin is added when a report is made by an user. </td>
        </tr>
        <tr>
            <td colspan="2"><pre>
CREATE OR REPLACE FUNCTION add_report_admin_notification() RETURNS TRIGGER AS
$BODY$
BEGIN
    INSERT INTO notification(text, date, id, type, idreport)
    VALUES (concat('New report notification: ',NEW.motive), NEW.date, 
            (SELECT users.id from users WHERE isadmin = TRUE ORDER BY random() LIMIT 1), 'Report', NEW.idreport);
    RETURN NEW;
END;
$BODY$
    LANGUAGE plpgsql;
<br>    
DROP TRIGGER IF EXISTS add_report_admin_notification on report CASCADE;
CREATE TRIGGER add_report_admin_notification
AFTER INSERT ON report
FOR EACH ROW
EXECUTE PROCEDURE add_report_admin_notification();
        </tr>
    </tbody>
</table>
<table>
	<tbody>
        <tr>
            <th> Trigger </th>
            <th> TRIGGER05 </th>
	    </tr>
        <tr>
            <th> Description </th>
            <td> Edit a message </td>
        </tr>
        <tr>
            <td colspan="2"><pre>
CREATE OR REPLACE FUNCTION edit_message() RETURNS trigger AS
$BODY$
    BEGIN
    UPDATE message SET text = NEW.text WHERE id = OLD.id;
RETURN NEW;
END;
$BODY$
    LANGUAGE plpgsql;
<br>
DROP TRIGGER IF EXISTS edit_message on message CASCADE;
CREATE TRIGGER edit_message
    AFTER UPDATE ON message
    FOR EACH ROW
EXECUTE PROCEDURE edit_message();
        </tr>
    </tbody>
</table>

<table>
	<tbody>
        <tr>
            <th> Trigger </th>
            <th> TRIGGER06 </th>
	    </tr>
        <tr>
            <th> Description </th>
            <td> When a user creates an event, becomes an event organizer and an attendee of that event </td>
        </tr>
        <tr>
            <td colspan="2"><pre>
CREATE OR REPLACE FUNCTION create_event_organizer() RETURNS TRIGGER AS
$BODY$
    BEGIN
    INSERT INTO attendee (id, idEvent) SELECT NEW.id, NEW.idEvent;
    RETURN NEW;
END;
$BODY$
    LANGUAGE plpgsql;
<br>
DROP TRIGGER IF EXISTS create_event_organizer on event_organizer CASCADE;
CREATE TRIGGER create_event_organizer
    AFTER INSERT ON event_organizer
    FOR EACH ROW
EXECUTE PROCEDURE create_event_organizer();
        </tr>
    </tbody>
</table>

<table>
	<tbody>
        <tr>
            <th> Trigger </th>
            <th> TRIGGER07 </th>
	    </tr>
        <tr>
            <th> Description </th>
            <td> When a user accpets an invite, becomes an attendee </td>
        </tr>
        <tr>
            <td colspan="2"><pre>
CREATE OR REPLACE FUNCTION accept_invite() RETURNS TRIGGER AS
$BODY$
BEGIN
    IF OLD.accepted != TRUE AND NEW.accepted = TRUE
    THEN
        INSERT INTO attendee (id, idevent) SELECT NEW.idInvitee, NEW.idevent;
    END IF;
    RETURN NEW;
END;
$BODY$
    LANGUAGE plpgsql;
<br>
DROP TRIGGER IF EXISTS accept_invite on invite CASCADE;
CREATE TRIGGER accept_invite
    AFTER UPDATE
    ON invite
    FOR EACH ROW
EXECUTE PROCEDURE accept_invite(); 
        </tr>
    </tbody>
</table>

<table>
	<tbody>
        <tr>
            <th> Trigger </th>
            <th> TRIGGER08 </th>
	    </tr>
        <tr>
            <th> Description </th>
            <td> Creates a notification to an event organizer when his/hers event is banned. </td>
        </tr>
        <tr>
            <td colspan="2"><pre>
CREATE OR REPLACE FUNCTION add_banned_notification() RETURNS TRIGGER AS
$BODY$
BEGIN
	IF NEW.state = 'Banned' THEN
    	INSERT INTO notification(text, date, id, type, idreport)
        VALUES ('Your event was banned!', NEW.date, 
                (SELECT event_organizer.id from event_organizer WHERE event_organizer.idevent = New.idevent LIMIT 1),
                'Report', NEW.idreport);
    END IF;
    RETURN NEW;
END;
$BODY$
    LANGUAGE plpgsql;
<br>
DROP TRIGGER IF EXISTS add_banned_notification on report CASCADE;
CREATE TRIGGER add_banned_notification
AFTER UPDATE ON report
FOR EACH ROW
EXECUTE PROCEDURE add_banned_notification();
        </tr>
    </tbody>
</table>

### 4. Transactions
 
> Transactions needed to assure the integrity of the data. 
<table>
	<tbody>
        <tr>
            <th> T01 </th>
            <th> TRAN01 </th>
	    </tr>
        <tr>
            <th> Description </th>
            <th> Get tags of current events </th>
	    </tr>
        <tr>
            <th> Justification </th>
            <td> In the middle of the transaction, the insertion of new rows in the event table can occur, which implies that the information retrieved in both selects is different, consequently resulting in a Phantom Read. It's READ ONLY because it only uses Selects. </td>
        </tr>
        <tr>
            <th> Isolation Level </th>
            <td> SERIALIZABLE READ ONLY </td>
        </tr>
        <tr>
        <td colspan="2"><pre>
        BEGIN TRANSACTION;
        SET TRANSACTION ISOLATION LEVEL SERIALIZABLE READ ONLY;
<br>
        -- Get number of current active events 
        SELECT COUNT(*)
        FROM event
        WHERE now() < final_date AND now() > publish_date;
<br>
        -- Get all tags of an event 
        SELECT e.title, t.tagname 
        FROM event e
        INNER JOIN event_Tag et ON et.idEvent = e.idEvent
        INNER JOIN tag t ON t.idtag = et.idTag
        WHERE now() < final_date AND now() > publish_date;
<br>
        END TRANSACTION;</td>
        </tr>
    </tbody>
</table>


<table>
	<tbody>
        <tr>
            <th> T02 </th>
            <th> TRAN02 </th>
	    </tr>
        <tr>
            <th> Description </th>
            <th> Insertion of tags in events </th>
	    </tr>
        <tr>
            <th> Justification </th>
            <td> In order to maintain consistency, it's necessary to use a transaction to ensure that all the code executes without errors. If an error occurs, a ROLLBACK is issued (e.g. when the insertion of an event fails). The isolation level is Repeatable Read, because, otherwise, an update of event_id_seq could happen, due to an insert in the table event committed by a concurrent transaction, and as a result, inconsistent data would be stored. </td>
        </tr>
        <tr>
            <th> Isolation Level </th>
            <td> REPEATABLE READ </td>
        </tr>
        <tr>            
        <td colspan="2"><pre>
BEGIN TRANSACTION;
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ
<br>        
-- Insert event
INSERT INTO event (title, description, visibility, picture, local, publish_date, start_date, final_date)
VALUES ($title, $description, $visibility, $picture, $local, $publish_date, $start_date, $final_date);
<br>
-- Insert event_tag
INSERT INTO event_tag (idTag, idEvent)
VALUES ($idTag, currval('event_id_seq'));
<br>
END TRANSACTION;
        </tr>
    </tbody>
</table>
        
<table>
	<tbody>
        <tr>
            <th> T03 </th>
            <th> TRAN03 </th>
	    </tr>
        <tr>
            <th> Description </th>
            <th> Insertion of files in messages </th>
	    </tr>
        <tr>
            <th> Justification </th>
            <td> In order to maintain consistency, it's necessary to use a transaction to ensure that all the code executes without errors. If an error occurs, a ROLLBACK is issued (e.g. when the insertion of a message fails). The isolation level is Repeatable Read, because, otherwise, an update of message_id_seq could happen, due to an insert in the table message committed by a concurrent transaction, and as a result, inconsistent data would be stored. </td>
        </tr>
        <tr>
            <th> Isolation Level </th>
            <td> REPEATABLE READ </td>
        </tr>
        <tr>            
        <td colspan="2"><pre>
BEGIN TRANSACTION;
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ
<br>        
-- Insert message
INSERT INTO message (text, date, likeCount, idEvent, id, parent)
VALUES ($text, $date, $likeCount, $idEvent, $id, $parent);
<br>
-- Insert message_file
INSERT INTO message_file (file, idMessage)
VALUES ($file, currval('message_id_seq'));
<br>
END TRANSACTION;
        </tr>
    </tbody>
</table>


## Annex A. SQL Code

[create.sql](https://git.fe.up.pt/lbaw/lbaw2223/lbaw22102/-/blob/main/create.sql)
[populate.sql](https://git.fe.up.pt/lbaw/lbaw2223/lbaw22102/-/blob/main/populate.sql)

### A.1. Database schema
               
```
DROP TABLE IF EXISTS attendee CASCADE;
DROP TABLE IF EXISTS choose_Option CASCADE;
DROP TABLE IF EXISTS EVENT CASCADE;
DROP TABLE IF EXISTS event_Organizer CASCADE;
DROP TABLE IF EXISTS event_Tag CASCADE;
DROP TABLE IF EXISTS invite CASCADE;
DROP TABLE IF EXISTS message CASCADE;
DROP TABLE IF EXISTS message_File CASCADE;
DROP TABLE IF EXISTS notification CASCADE;
DROP TABLE IF EXISTS option CASCADE;
DROP TABLE IF EXISTS poll CASCADE;
DROP TABLE IF EXISTS report CASCADE;
DROP TABLE IF EXISTS tag CASCADE;
DROP TABLE IF EXISTS "user" CASCADE;
DROP TABLE IF EXISTS vote CASCADE;

DROP TYPE IF EXISTS reportState;
DROP type if EXISTS notificationTypes;

CREATE TYPE reportState as ENUM ('Pending','Rejected','Banned');
CREATE TYPE notificationTypes as ENUM ('Invite','Message','Report');


-- Table: user

CREATE TABLE "user" (
    id        SERIAL PRIMARY KEY,
    username  TEXT UNIQUE NOT NULL,
    password  TEXT NOT NULL,
    email     TEXT UNIQUE NOT NULL,
    picture   TEXT,
    isBlocked TEXT,
    isAdmin   BOOLEAN DEFAULT (False) 
);

-- Table: event

CREATE TABLE event (
    idEvent     SERIAL PRIMARY KEY,
    title       TEXT NOT NULL,
    description TEXT,
    visibility  BOOLEAN NOT NULL,
    picture     TEXT,
    local       TEXT NOT NULL,
  	publish_date DATE NOT NULL,
    start_date DATE NOT NULL,
    final_date DATE NOT NULL
);


-- Table: poll

CREATE TABLE poll (
    idPoll      SERIAL PRIMARY KEY,
    title       TEXT NOT NULL,
    description TEXT,
    date        DATE NOT NULL,
    isOpen      BOOLEAN NOT NULL DEFAULT (True),
    idEvent     INTEGER NOT NULL REFERENCES event (idEvent),
    id          INTEGER NOT NULL REFERENCES "user" (id)        
);


-- Table: report

CREATE TABLE report (
    idReport 	SERIAL PRIMARY KEY,
    idEvent  	INTEGER NOT NULL REFERENCES event (idEvent),
    idManager   	INTEGER REFERENCES "user" (id),
    idReporter  	INTEGER NOT NULL REFERENCES "user" (id),
    date     	DATE NOT NULL,
    motive   	TEXT NOT NULL,
  	STATE    	reportState NOT NULL DEFAULT ('Pending')
);

-- Table: option

CREATE TABLE option (
    idOption SERIAL PRIMARY KEY,
    text     TEXT NOT NULL,
    idPoll   INTEGER NOT NULL REFERENCES poll
);


-- Table: tag

CREATE TABLE tag (
    idTag   SERIAL PRIMARY KEY,
    tagName TEXT UNIQUE NOT NULL
                    
);

-- Table: attendee
CREATE TABLE attendee (
    id      INTEGER NOT NULL REFERENCES "user" (id),
    idEvent INTEGER NOT NULL REFERENCES event (idEvent),
    PRIMARY KEY (
        id,
        idEvent
    )
);


-- Table: choose_Option

CREATE TABLE choose_Option (
    id       INTEGER NOT NULL REFERENCES "user" (id),
    idOption INTEGER NOT NULL REFERENCES option,
    PRIMARY KEY (
        id,
        idOption
    )
);



-- Table: event_Organizer

CREATE TABLE event_Organizer (
    id      INTEGER NOT NULL REFERENCES "user" (id),
    idEvent INTEGER NOT NULL REFERENCES event (idEvent),
    PRIMARY KEY (
        id,
        idEvent
    )
);


-- Table: event_Tag

CREATE TABLE event_Tag (
    idTag   INTEGER NOT NULL REFERENCES tag (idTag),
    idEvent INTEGER NOT NULL REFERENCES event (idEvent),
    PRIMARY KEY (
        idTag,
        idEvent
    )
);


-- Table: invite

CREATE TABLE invite (
    idEvent     INTEGER NOT NULL REFERENCES event (idEvent),
    idInvitee   INTEGER NOT NULL REFERENCES "user" (id),
    idOrganizer INTEGER NOT NULL REFERENCES "user" (id),
    accepted    BOOLEAN,
    PRIMARY KEY (
        idEvent,
        idInvitee
    )
);


-- Table: message

CREATE TABLE message (
    idMessage SERIAL PRIMARY KEY,
    text      TEXT,
    date      DATE NOT NULL,
    likeCount INTEGER NOT NULL DEFAULT (0),
    idEvent   INTEGER NOT NULL REFERENCES event (idEvent),
    id	   INTEGER NOT NULL REFERENCES "user" (id),
    parent    INTEGER REFERENCES message (idMessage)
);


-- Table: message_File

CREATE TABLE message_File (
    idFile    SERIAL PRIMARY KEY,
    file      TEXT,
    idMessage INTEGER NOT NULL REFERENCES message (idMessage) 
);


-- Table: notification

CREATE TABLE notification (
    idNotif   SERIAL PRIMARY KEY,
    text      TEXT NOT NULL,
    date      DATE NOT NULL,
    read      BOOLEAN NOT NULL DEFAULT (False),
    id        INTEGER NOT NULL REFERENCES "user" (id),
    Type	   notificationTypes,
    idReport  INTEGER REFERENCES report (idReport) CHECK ((idReport = NULL) or (idReport != NULL and type = 'Report')),
    idEvent   INTEGER CHECK ((idEvent = NULL) or (idEvent != NULL and type = 'Invite')),
    idInvitee INTEGER CHECK ((idInvitee = NULL) or (idInvitee != NULL and type = 'Invite')),
    idMessage INTEGER REFERENCES message (idMessage) CHECK ((idMessage = NULL) or (idMessage != NULL and type = 'Message')),
    FOREIGN KEY (
        idEvent,
        idInvitee
    )
    REFERENCES invite
);


-- Table: vote

CREATE TABLE vote (
    id        INTEGER NOT NULL REFERENCES "user" (id),
    idMessage INTEGER NOT NULL REFERENCES message (idMessage),
    PRIMARY KEY (
        id,
        idMessage
    )
);


-----------------------------------------
-- INDEXES
-----------------------------------------

CREATE INDEX event_poll ON poll USING hash (idEvent);
CREATE INDEX date_message ON message USING btree (date);
CREATE INDEX tag_alphabetic ON tag USING btree (tagName);

-- FTS INDEXES

ALTER TABLE event ADD COLUMN tsvectors TSVECTOR;

CREATE OR REPLACE FUNCTION event_search_update() RETURNS TRIGGER AS $$
BEGIN
 IF TG_OP = 'INSERT' THEN
        NEW.tsvectors = (
         setweight(to_tsvector('english', NEW.title), 'A') ||
         setweight(to_tsvector('english', NEW.description), 'B')
        );
 END IF;
 IF TG_OP = 'UPDATE' THEN
         IF (NEW.title <> OLD.title OR NEW.description <> OLD.description) THEN
           NEW.tsvectors = (
             setweight(to_tsvector('english', NEW.title), 'A') ||
             setweight(to_tsvector('english', NEW.description), 'B')
           );
         END IF;
 END IF;
 RETURN NEW;
END $$
LANGUAGE plpgsql;

DROP TRIGGER IF EXISTS event_search_update on event CASCADE;
CREATE TRIGGER event_search_update
 BEFORE INSERT OR UPDATE ON event
 FOR EACH ROW
 EXECUTE PROCEDURE event_search_update();

 CREATE INDEX search_idx ON event USING GIN (tsvectors); 


-----------------------------------------
-- TRIGGERS and UDFs
-----------------------------------------

--- TRIGGER01
CREATE OR REPLACE FUNCTION check_event_organizer() RETURNS TRIGGER AS
$BODY$
BEGIN
    IF OLD.id IN (SELECT id from event_organizer WHERE idevent = OLD.idEvent) and (select count(*) from event_organizer WHERE idevent = OLD.idEvent) = 1 AND (SELECT COUNT(*) FROM attendee WHERE idevent = OLD.idEvent) > 1
    THEN
        RAISE EXCEPTION 'Event with attendees must have at least one event organizer!';
    END IF;
    RETURN OLD;
END;
$BODY$
    LANGUAGE plpgsql;

DROP TRIGGER IF EXISTS check_event_organizer on attendee CASCADE;
CREATE TRIGGER check_event_organizer
    BEFORE DELETE ON attendee
    FOR EACH ROW
EXECUTE PROCEDURE check_event_organizer();


--- TRIGGER02 
CREATE OR REPLACE FUNCTION add_invite_notification() RETURNS TRIGGER AS
$BODY$
BEGIN
INSERT INTO notification(text, date, id, type, idinvitee, idevent)
    VALUES (concat('You have been invited to a new event: ', (select title from event where event.idevent = new.idevent)) , now(),  NEW.idinvitee, 'Invite', NEW.idinvitee, NEW.idevent);
    RETURN NEW;
END;
$BODY$
    LANGUAGE plpgsql;

DROP TRIGGER IF EXISTS add_invite_notification on invite CASCADE;
CREATE TRIGGER add_invite_notification
    AFTER INSERT
    ON invite
    FOR EACH ROW
EXECUTE PROCEDURE add_invite_notification(); 

--- TRIGGER03 
CREATE OR REPLACE FUNCTION add_message_notification() RETURNS TRIGGER AS
$BODY$
BEGIN
    INSERT INTO notification(text, date, id, type, idmessage)
    VALUES (concat('New notification: ', NEW.text), NEW.date, 
            (SELECT event_organizer.id FROM event_Organizer WHERE event_Organizer.idEvent = NEW.idEvent), 'Message', NEW.idmessage);
    RETURN NEW;
END;
$BODY$
    LANGUAGE plpgsql;

DROP TRIGGER IF EXISTS add_message_notification on message CASCADE;
CREATE TRIGGER add_message_notification
AFTER INSERT ON message
FOR EACH ROW
EXECUTE PROCEDURE add_message_notification();

--- TRIGGER04 
CREATE OR REPLACE FUNCTION add_report_admin_notification() RETURNS TRIGGER AS
$BODY$
BEGIN
    INSERT INTO notification(text, date, id, type, idreport)
    VALUES (concat('New report notification: ',NEW.motive), NEW.date, 
            (SELECT "user".id from "user" WHERE isadmin = TRUE ORDER BY random() LIMIT 1), 'Report', NEW.idreport);
    RETURN NEW;
END;
$BODY$
    LANGUAGE plpgsql;
    
DROP TRIGGER IF EXISTS add_report_admin_notification on report CASCADE;
CREATE TRIGGER add_report_admin_notification
AFTER INSERT ON report
FOR EACH ROW
EXECUTE PROCEDURE add_report_admin_notification();

--- TRIGGER05 
CREATE OR REPLACE FUNCTION edit_message() RETURNS trigger AS
$BODY$
    BEGIN
    UPDATE message SET text = NEW.text WHERE id = OLD.id;
RETURN NEW;
END;
$BODY$
    LANGUAGE plpgsql;

DROP TRIGGER IF EXISTS edit_message on message CASCADE;
CREATE TRIGGER edit_message
    AFTER UPDATE ON message
    FOR EACH ROW
EXECUTE PROCEDURE edit_message();

--- TRIGGER06 
CREATE OR REPLACE FUNCTION create_event_organizer() RETURNS TRIGGER AS
$BODY$
    BEGIN
    INSERT INTO attendee (id, idEvent) SELECT NEW.id, NEW.idEvent;
    RETURN NEW;
END;
$BODY$
    LANGUAGE plpgsql;
    
DROP TRIGGER IF EXISTS create_event_organizer on event_organizer CASCADE;
CREATE TRIGGER create_event_organizer
    AFTER INSERT ON event_organizer
    FOR EACH ROW
EXECUTE PROCEDURE create_event_organizer();

--- TRIGGER07 
CREATE OR REPLACE FUNCTION accept_invite() RETURNS TRIGGER AS
$BODY$
BEGIN
    IF OLD.accepted != TRUE AND NEW.accepted = TRUE
    THEN
        INSERT INTO attendee (id, idevent) SELECT NEW.idInvitee, NEW.idevent;
    END IF;
    RETURN NEW;
END;
$BODY$
    LANGUAGE plpgsql;

DROP TRIGGER IF EXISTS accept_invite on invite CASCADE;
CREATE TRIGGER accept_invite
    AFTER UPDATE
    ON invite
    FOR EACH ROW
EXECUTE PROCEDURE accept_invite(); 


--- TRIGGER08 
CREATE OR REPLACE FUNCTION add_banned_notification() RETURNS TRIGGER AS
$BODY$
BEGIN
	IF NEW.state = 'Banned' THEN
    	INSERT INTO notification(text, date, id, type, idreport)
        VALUES ('Your event was banned!', NEW.date, 
                (SELECT event_organizer.id from event_organizer WHERE event_organizer.idevent = New.idevent LIMIT 1),
                'Report', NEW.idreport);
    END IF;
    RETURN NEW;
END;
$BODY$
    LANGUAGE plpgsql;

DROP TRIGGER IF EXISTS add_banned_notification on report CASCADE;
CREATE TRIGGER add_banned_notification
AFTER UPDATE ON report
FOR EACH ROW
EXECUTE PROCEDURE add_banned_notification();


```
        
### A.2. Database population

```
INSERT INTO "user" (username, password, email, picture, isBlocked, isAdmin) VALUES ('admin', 'aFdHe#f5VWsa', 'admin1_wemeet@gmail.com', '', NULL, TRUE);
INSERT INTO "user" (username, password, email, picture, isBlocked, isAdmin) VALUES ('mariamota22', 'mota4003', 'mariamota2002@gmail.com', 'https://drive.google.com/file/d/1rh0RDPgLJvjbfxuv6TKvp_ekTEASTPnI/view?usp=sharing', NULL, FALSE);
INSERT INTO "user" (username, password, email, picture, isBlocked, isAdmin) VALUES ('joaosilva1', 'joAS85r', 'jojo21@gmail.com', 'https://drive.google.com/file/d/10sSg9mua_y9J4KcgmnCZlL_Cj0LQTeWY/view?usp=sharing', NULL, FALSE);
INSERT INTO "user" (username, password, email, picture, isBlocked, isAdmin) VALUES ('manuelaesteves33', 'martim124', 'manuelassteves33@gmail.com', 'https://drive.google.com/file/d/11LHjxNDDVlsa9iJRbWYdCVyuWtYCalX2/view?usp=share_link', NULL, FALSE);
INSERT INTO "user" (username, password, email, picture, isBlocked, isAdmin) VALUES ('raulferreira21', 'ramf3t', 'raul_ferreira_21@gmail.com', 'https://drive.google.com/file/d/1D8GL8FJY11M4pesov2puGVCZt8Cp5gmX/view?usp=sharing', NULL, FALSE);
INSERT INTO "user" (username, password, email, picture, isBlocked, isAdmin) VALUES ('nunomaciel77', '9yT53fgs', 'nunomaciel77@gmail.com', 'https://drive.google.com/file/d/1_BQ4r_2R7k0b-rdnbPcl4P_boMDKz6Dn/view?usp=sharing', NULL, FALSE);
INSERT INTO "user" (username, password, email, picture, isBlocked, isAdmin) VALUES ('maraneves45', 'mar5ipT', 'maraneves32@gmail.com', 'https://drive.google.com/file/d/12_IzCtKTRRHTveIVSp03SgkrDCsroiVj/view?usp=sharing', NULL, FALSE);
INSERT INTO "user" (username, password, email, picture, isBlocked, isAdmin) VALUES ('mcarlotacarneiro20', 'mccar22', 'mcarlotaccar20@gmail.com', 'https://drive.google.com/file/d/1M8D9xsFxmkUaCptyTGrz2D7O069tKFVR/view?usp=sharing', NULL, FALSE);
INSERT INTO "user" (username, password, email, picture, isBlocked, isAdmin) VALUES ('andreoliveira56', 'andrrTop8', 'dreoliveira56@gmail.com', 'https://drive.google.com/file/d/15hPo_tlOat5wn1oJl4EkDMokwUhh-5HR/view?usp=sharing', NULL, FALSE);
INSERT INTO "user" (username, password, email, picture, isBlocked, isAdmin) VALUES ('aefeup', 'af5Tup!1.GPd', 'aefeup@gmail.com', 'https://drive.google.com/file/d/1RCBXeNsklPGVDFJzwlZih-5ylSoR0gqO/view?usp=share_link', NULL, FALSE);

(...)
```

---


## Revision history

Changes made to the first submission:
1. 09/10/2022 - Added content related to A4
2. 14/10/2022 - Added content related to A5
3. 20/10/2022 - Added content related to A6 

***
GROUP22102, 31/10/2022

* Bruna Marques, up202007191@edu.fe.up.pt
* Francisca Guimarães, up202004229@edu.fe.up.pt (editor)
* Mariana Teixeira, up201905705@edu.fe.up.pt
* Martim Henriques, up202004421@edu.fe.up.pt