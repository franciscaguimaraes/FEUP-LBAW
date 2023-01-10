# EAP: Architecture Specification and Prototype

> To facilitate events’ management and choices for people by making them organized, simple, and intuitive.

## A7: Web Resources Specification

> This artifact documents the architecture of the web application to be developed, including the CRUD (create, read, update, delete) operations for each resource. This specification adheres to the OpenAPI standard using YAML.


### 1. Overview

> This section defines an overview of the web application where the modules are identified and briefly described. 


| ||
| -------- | -------- |
| M01: Authentication and Individual Profile     | Web resources associated with user authentication and individual profile management, including the following system features: login, logout, register, password recovery, view and edit profile.   |
| M02: Events |  Web resources associated with events. Includes the following system features:  create, edit, join, invite and delete events.|
| M03: Messages and Forum | Web resources associated with events forum and messages. Includes the following system features:  add, edit, vote and delete comments, create and answer polls and upload files. |
| M04: User Administration | Web resources associated with users administration, including the following system features: block/unblock users, manage event reports and delete events and user accounts. |
| M05 : Static Pages | Web resources associated with static pages, including the following system features: about us, contacts, user help.|


### 2. Permissions

> This section defines the permissions used in the modules to establish the conditions of access to resources.

|  | | |
| -------- | -------- | -------- |
| PUB    | Public   | Users without privileges |
| USR    | User    | Authenticated Users    |
| OWN    | Owner    | User who is the owner of the information (comment, event or account) |
| ADM    | Administrator | System Administrators|

### 3. OpenAPI Specification

> OpenAPI specification in YAML format to describe the web application's web resources.



link: [a7_openapi.yaml](https://git.fe.up.pt/lbaw/lbaw2223/lbaw22102/-/blob/main/a7_openapi.yaml)


```yaml
openapi: 3.0.0

info:
 version: '1.0'
 title: 'LBAW WeMeet Web API'
 description: 'Web Resources Specification (A7) for WeMeet'

servers:
- url: https://lbaw22102.lbaw.fe.up.pt/
  description: Production server

externalDocs:
 description: Find more info here.
 url: https://git.fe.up.pt/lbaw/lbaw2223/lbaw22102/-/wikis/Architecture-Specification-and-Prototype

tags:
- name: 'M01: Authentication and Individual Profile'
  description: 'Web resources associated with user authentication and individual profile management, includes the following system features: login/logout, registration, credential recovery, view and edit personal profile information.'
- name: 'M02: Events'
  description: 'Web resources associated with events. Includes the following system features:  create, edit, join, invite and delete events.'
- name: 'M03: Messages and Forum'
  description: 'Web resources associated with events forum and messages. Includes the following system features:  add, edit, vote and delete comments, create and answer polls and upload files.'
- name: 'M04: User Administration'
  description: 'Web resources associated with users administration, including the following system features: block/unblock users, manage event reports and delete events and user accounts.'
- name: 'M05: Static pages'
  description: 'Web resources associated with static pages, including the following system features: about us, contacts, user help.'

paths:
 /login:
   get:
     operationId: R101
     summary: 'R101: Login Form'
     description: 'Provide login form. Access: PUB'
     tags:
       - 'M01: Authentication and Individual Profile'
     responses:
       '200':
         description: 'Ok. Show [UI06]'
   post:
     operationId: R102
     summary: 'R102: Login Action'
     description: 'Processes the login form submission. Access: PUB'
     tags:
       - 'M01: Authentication and Individual Profile'

     requestBody:
       required: true
       content:
         application/x-www-form-urlencoded:
           schema:
             type: object
             properties:
               password:    
                 type: string
               email:          
                 type: string
             required:
                  - password

     responses:
       '302':
         description: 'Redirect after processing the login credentials.'
         headers:
           Location:
             schema:
               type: string
             examples:
               302Success:
                 description: 'Successful authentication. Redirect to events page.'
                 value: '/events'
               302Error:
                 description: 'Failed authentication. Redirect to login form.'
                 value: '/login'

 /logout:
   post:
     operationId: R103
     summary: 'R103: Logout Action'
     description: 'Logout the current authenticated user. Access: USR, ADM'
     tags:
       - 'M01: Authentication and Individual Profile'
     responses:
       '302':
         description: 'Redirect after processing logout.'
         headers:
           Location:
             schema:
               type: string
             examples:
               302Success:
                 description: 'Successful logout. Redirect to login form.'
                 value: '/login'

 /register:
   get:
     operationId: R104
     summary: 'R104: Register Form'
     description: 'Provide new user registration form. Access: PUB'
     tags:
       - 'M01: Authentication and Individual Profile'
     responses:
       '200':
         description: 'Ok. Show [UI07]'

   post:
     operationId: R105
     summary: 'R105: Register Action'
     description: 'Processes the new user registration form submission. Access: PUB'
     tags:
       - 'M01: Authentication and Individual Profile'

     requestBody:
       required: true
       content:
         application/x-www-form-urlencoded:
           schema:
             type: object
             properties:
               username:
                 type: string
               email:
                 type: string
               password:
                 type: string
             required:
                  - username
                  - email
                  - password

     responses:
       '302':
         description: 'Redirect after processing the new user information.'
         headers:
           Location:
             schema:
               type: string
             examples:
               302Success:
                 description: 'Successful authentication. Redirect to user profile.'
                 value: '/events'
               302Failure:
                 description: 'Failed authentication. Redirect to login form.'
                 value: '/login'

 /profile:
    get:
      operationId: R106
      summary: 'R106: View User Profile'
      description: 'Show the individual user profile. Access: USR'
      tags:
        - 'M01: Authentication and Individual Profile'

      responses:
        '200':
          description: 'Ok. Show [UI10]'
        '404':
          description: 'Not Found.'


 /profile/editProfile:
    post:
      operationId: R107
      summary: 'R107: Edit Profile'

      description: 'Edit profile information. Access: OWN'
      tags:
        - 'M01: Authentication and Individual Profile'

      requestBody:
        required: true
        content:
          application/x-www-form-urlencoded:
            schema:
              type: object
              properties:
                password:
                  type: string
                picture:
                  type: string
                  format: byte

      responses:
       '302':
         description: 'Redirect after processing update of information.'
         headers:
           Location:
             schema:
               type: string
             examples:
               302Success:
                 description: 'Successful update. Redirect to edit user profile.'
                 value: '/profile/editProfile'
               302Failure:
                 description: 'Update Failed. Redirect to edit profile form.'
                 value: '/profile/editProfile'

 /events:
  get:
      operationId: R201
      summary: "R201: Show Events"
      description: "Shows all public events and private events if user is attending them. Access: PUB, USR"
      tags:
        - 'M02: Events'

      responses:
            '200':
              description: Ok. Show [UI14]
            '400':
              description: Bad Request

 /events/{id}:
  get:
      operationId: R202
      summary: "R202: Show Event Card"
      description: "Shows Event's information. Access: PUB, USR"
      tags:
        - 'M02: Events'

      parameters:
        - in: path
          name: id
          schema:
            type: integer
          required: true

      responses:
            '200':
              description: Ok. Show [UI15]
            '400':
              description: Bad Request

 /eventsCreate:
  get:
      operationId: R203
      summary: "R203: Event Form"
      description: "Provide event form. Access: USR"
      tags:
        - 'M02: Events'

      responses:
       '200':
         description: 'Ok'

  post:
      operationId: R204
      summary: "R204: Event Action"
      description: "Processes the event form submission. Access: USR"
      tags:
        - 'M02: Events'

      requestBody:
        required: true
        content:
          application/x-www-form-urlencoded:
            schema:
              type: object
              properties:
                title:    
                  type: string
                description:          
                  type: string
                visibility:
                  type: integer
                picture:
                  type: string
                  format: byte
                local:
                  type: string
                start_date:
                  type: string
                final_date:
                  type: string
              required:
                    - title 
                    - visibility
                    - local
                    - start_date 
                    - final_date 

      responses:
        '302':
          description: 'Redirect after processing the event form.'
          headers:
            Location:
              schema:
                type: string
              examples:
                302Success:
                  description: 'Successful creation. Redirect to events page.'
                  value: '/events'
                302Error:
                  description: 'Creation failed. Redirect to event form.'
                  value: '/eventsCreate'

 /myevents:
  get:
      operationId: R205
      summary: "R205: View events created by user"
      description: "Show events created by user. Access: OWN"
      tags:
        - 'M02: Events'

      responses:
       '200':
         description: 'Ok. Show [UI12]'


 /calendar:
  get:
      operationId: R206
      summary: "R206: View attended/to attend events"
      description: "Show events attened/to attend by user. Access: USR"
      tags:
        - 'M02: Events'

      responses:
        '200':
         description: 'Ok. Show [UI12]'

 /:   
    get:
      operationId: R501
      summary: 'R501: View Home Page'
      description: "View Home page. Access: PUB"
      tags:
        - 'M05: Static pages'      
        
      responses:
        '200':
          description: 'OK. Show [UI01]'

 /deleteUser/{id}: 
    get:
      operationId: R401
      summary: "R401: Delete User"
      description: "Delete User . Access: ADM"
      tags:
        - 'M04: User Administration'

      parameters:
        - in: path
          name: id
          schema:
            type: integer
          required: true

      responses:
        '200':
         description: 'Ok. Show [UI02]'
   
 /manageUsers:
    get:
      operationId: R402
      summary: "R402: View User Management Page"
      description: "View user management page . Access: ADM"
      tags:
        - 'M04: User Administration'

      responses:
        '200':
         description: 'Ok. Show [UI02]'

 /removeFromEvent/{id_attendee}/{id_event}: 
    get:
      operationId: R403
      summary: "R403: Manage Attendees"
      description: "Manages Attendees . Access: OWN"
      tags:
        - 'M04: User Administration'

      parameters:
        - in: path
          name: id_attendee
          schema:
            type: integer
          required: true
        - in: path
          name: id_events
          schema:
            type: integer
          required: true

      responses:
        '200':
         description: 'Ok. Show [UI03]'

 /joinEvent/{id}: 
    get:
      operationId: R207
      summary: "R207: Join Event"
      description: "Join Event . Access: USR"
      tags:
        - 'M02: Events'

      parameters:
        - in: path
          name: id
          schema:
            type: integer
          required: true

      responses:
        '200':
         description: 'Ok'

 /abstainEvent/{id}:
  get:
      operationId: R208
      summary: "R208: Abstain Event"
      description: "Abstain Event . Access: USR"
      tags:
        - 'M02: Events'

      parameters:
        - in: path
          name: id
          schema:
            type: integer
          required: true

      responses:
        '200':
         description: 'Ok'


 /event:
    get:
      operationId: R209
      summary: "R209: Search Events"
      description: "Searches for events and returns the results as JSON. Access: PUB"
      tags:
        - 'M02: Events'

      parameters:
        - in: query
          name: query
          description: String to use for full-text search
          schema:
            type: string
          required: false
        - in: query
          name: closed
          description: Boolean with the closed flag value
          schema:
            type: boolean
          required: false

      responses:
        '200':
          description: Success
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Event'
        '400':
          description: Bad Request

components:
  schemas:
    Event:
        type: object
        properties:
          id:
            type: integer
          title:
            type: string
          description:
            type: string
          visibility:
            type: boolean
          picture:
            type: string
            format: byte
          publish_date:
            type: string 
          start_date:
            type: string
          final_sate:
            type: string


```

---


## A8: Vertical prototype

> The Vertical Prototype includes the implementation of the features marked as necessary. This artifact aims to validate the architecture presented, also serving to gain familiarity with the technologies used in the project.

### 1. Implemented Features

#### 1.1. Implemented User Stories

> The following table describes the implemented user stories.

| User Story reference | Name                   | Priority                   | Description                   |
| -------------------- | ---------------------- | -------------------------- | ----------------------------- |
US101 | Browse Public Events | High | As a User, I want to browse public events so that I can find one that I’m interested in. 
US102 | View Public Event | High | As a User, I want to view a public event so that I can see the event’s information (date, time, and location). 
US301 | Log out | High | As an Authenticated User, I want to log out of my session so that I can share the same device with other users. 
US302 | Edit Profile | High | As an Authenticated User, I want to edit my profile so that I can change my personal information. 
US303 | View Profile | High | As an Authenticated User, I want to view my profile so that I can view my information. 
US304 | Create Event | High | As an Authenticated User, I want to create an event so that I can invite authenticated users and manage the event. 
US305 | Share Public Event with Users| High | As an Authenticated User, I want to share a public event, so that any person will be able to see the information about the event.
US306 | Join Public Event | High | As an Authenticated User, I want to be able to join a public event so that I can let the event organizer know that I am going to their event.
US307 | Manage My Events | High | As an Authenticated User, I want to manage my events so that I can organize them.
US308 | Manage Events Attended / to Attend | High | As an Authenticated User, I want to manage the events I attended/to attend so that I can organize my personal schedule accordingly.
US501 | Edit Event Details | High | As an Event Organizer, I want to edit an event's information, such as name, date, and location, so that I can keep it up to date. 
US502 | Manage Event Participants | High | As an Event Organizer I want to be able to manage my event participants so that I can know exactly each person's permissions.
US601 | Administer User Accounts | High | As an Administrator, I want to be able to administer user accounts so that I can have control over their actions and keep the system clean and appropriate.

> 94% of our high user stories were implemented. The missing user story is:

| User Story reference | Name                   | Priority                   | Description                   |
| -------------------- | ---------------------- | -------------------------- | ----------------------------- |
US103 | Search | High | As a User, I want to be able to search the entire platform so that I can find an event/user I'm looking for by using full text search, exact match, or filters. 


#### 1.2. Implemented Web Resources

The web resources that were implemented in the prototype are described in the next section.

> Module M01: Authentication and Individual Profile  

| Web Resource Reference | URL                            |
| ---------------------- | ------------------------------ |
| R101: Login Form | /login |
| R102: Login Action | POST /login |
| R103: Logout Action | POST /logout |
| R104: Register Form | /register |
| R105: Register Action | POST /register |
| R106: View User Profile | /profile |
| R107: Edit Profile | POST /profile/editProfile | 

Module M02: Event
| Web Resource Reference | URL                            |
| ---------------------- | ------------------------------ |
| R201: Show Events | /events |
| R202: Show Event Card | /events/{id} |
| R203: Event Form | /eventsCreate |
| R204: Event Action | POST /eventsCreate |
| R205: View created events by user | /myevents |
| R206: View attended/to attend events by user | /calendar 
| R207: Join Event | /joinEvent/{id} |
| R208: Abstain Event | /abstainEvent/{id} |
| R209: Search Event | /event |


Module M04: User Administration 
| Web Resource Reference | URL                            |
| ---------------------- | ------------------------------ |
| R401: Delete User | /deleteUser/{id}
| R402: View User Management Page | /manageUsers
| R401: Manage Attendees | /removeFromEvent/{id_attendee}/{id_event}


Module M05: Static Pages 
| Web Resource Reference | URL                            |
| ---------------------- | ------------------------------ |
| R501: View Home Page | / |


### 2. Prototype

The prototype is available at [https://lbaw22102.lbaw.fe.up.pt/](https://lbaw22102.lbaw.fe.up.pt/)

Credentials:

| Type of User | Email | Password |
| ---------------------- | ----------- |-----------|
| Admin | admin1_wemeet@gmail.com | password |
| Regular | manuelassteves33@gmail.com | password |

The code is available at 
[https://git.fe.up.pt/lbaw/lbaw2223/lbaw22102](https://git.fe.up.pt/lbaw/lbaw2223/lbaw22102)

---


## Revision history

Changes made to the first submission:
1. Added content related to A7 and A8 (20/11/2022)

***
GROUP22102, 25/11/2022

* Bruna Marques, up202007191@edu.fe.up.pt
* Francisca Guimarães, up202004229@edu.fe.up.pt
* Mariana Teixeira, up201905705@edu.fe.up.pt 
* Martim Henriques, up202004421@edu.fe.up.pt (editor)