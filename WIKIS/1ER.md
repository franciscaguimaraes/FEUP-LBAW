# ER: Requirements Specification Component

> To facilitate events’ management and choices for people by making them organized, simple, and intuitive.

## A1: WeMeet

WeMeet is a web-based information system for managing various types of events, aiming to become a useful product targeted at individual users that want to attend and/or administer events.

Throughout the years, there has been a continuous lack of in-person connection between people. Therefore, we believe this is a tool that would intuitively encourage more social interactions as it can be used by anyone, from families to groups of friends to work meetings. 
With WeMeet, our clients will be able to create and attend events in a very simple, reliable, and organized way. Our website will have an intuitive design to provide a responsive and enjoyable browsing experience on either desktop or smartphone devices. Upon entering our website, our clients will be presented with a simple home page containing a brief overview of our main purpose.

Our clients will be categorized into groups with different permissions. 
These groups include non-authenticated users, users who don’t have an account, who will only be able to view information about public events and utilize a search bar that allows them to search for events of their interest using tags. All events, public or private, will present a forum where guests can interact with each other by uploading files or adding comments. Non-authenticated users may only view public forums.

The authenticated users, users who have created an account, will, in addition to the privileges mentioned above, have in their user profile their personal attendance calendar with the history of events they have gone to or are going to. They can accept or refuse invitations, and interact and share their experiences in public forums as they wish.

The authenticated users can be attendees, and, in case they create an event, or are granted permission to edit said event, event organizers.
The attendees, users who have accepted an invitation to an event, can be attendees of a public or a private event. If they attend a private event, they will have access to a private forum that can only be seen by the event organizer and attendees. The event organizers, users who created an event, will have complete access and editing privileges to the event’s information, including adding more advanced features such as the creation of polls. The event organizer can also grant editing privileges to the attendees whenever they want.

The Administrators are users that will have the same permissions as the above-mentioned groups, with the addition of having event/user management and supervision responsibilities. Their goal is to keep the platform clean and appropriate, providing every user an enjoyable and pleasant experience.

---


## A2: Actors and User Stories

> This artifact contains the description of our system's actors and their respective user stories.


### 1. Actors

![](https://i.imgur.com/5qwb9nt.png)

*Fig.1: Actors UML*

| Actors | Description   |
| --- | --- |
| User | Generic user that has access to the website. |
| Visitor | Unauthenticated user that has access to public information, such as the main page and public events. |
| Attendee    | Authenticated user that accepted an invitation to an event and can, therefore, see privileged information about it and interact with other attendees. |
| Event Organizer | Authenticated user that created an event and has all the event management privileges. |
| Administrator   | Authenticated user that is responsible for the management of users and supervision of the web app. |
| Gmail API | External OAuth API, Gmail API, that can be used to register, authenticate into the system and send event invitations. |


*Table 1: Actor's Description*

### 2. User Stories

#### 2.1. **User**

| Identifier | Name | Priority | Description |
| --- | --- | --- | --- |
US101 | Browse Public Events | High | As a User, I want to browse public events so that I can find one that I’m interested in. 
US102 | View Public Event | High | As a User, I want to view a public event so that I can see the event’s information (date, time, and location). 
US103 | Search | High | As a User, I want to be able to search the entire platform so that I can find an event/user I'm looking for by using full text search, exact match, or filters. 
US104 | Explore Events by Tag | Medium | As a User, I want to be able to explore events by tag so that I can find an event according to my interests. 
US105 | About US | Medium | As a User, I want to access the about page so that I can see a complete description of the website and its creators. 
US106 | Contacts | Medium | As a User, I want to access contacts so that I can get in touch with the platform creators.
US107 | User Help | Medium | As a User, I want to be able to get help from someone specialized so that I can use the website properly.

*Table 2: User's User Stories*


#### 2.2. **Visitor**

| Identifier | Name | Priority | Description |
| --- | --- | --- | --- |
US201 | Login | High | As a Visitor, I want to authenticate into the system so that I can access privileged information.
US202| Registration | High | As a Visitor, I want to register myself into the system so that I can authenticate myself into the system. 
US203 | Recover Password | Medium | As an Authenticated User, I want to recover my password so that I am not locked out of my account.
US204 | Sign-up using external API | Low | As a Visitor, I want to create a new account linked to my e-mail account so that I can access privileged information. 
US205 | Sign-in using external API | Low | As a Visitor, I want to sign-in through my e-mail account so that I can authenticate myself into the system.

*Table 3: Visitor's User Stories*

#### 2.3. **Authenticated User**

| Identifier | Name | Priority | Description |
| --- | --- | --- | --- |
US301 | Log out | High | As an Authenticated User, I want to log out of my session so that I can share the same device with other users. 
US302 | Edit Profile | High | As an Authenticated User, I want to edit my profile so that I can change my personal information. 
US303 | View Profile | High | As an Authenticated User, I want to view my profile so that I can view my future and past events. 
US304 | Create Event | High | As an Authenticated User, I want to create an event so that I can invite authenticated users and manage the event. 
US305 | Share Public Event with Users| High | As an Authenticated User, I want to share a public event with other authenticated users, so that the other user see the information about the event.
US306 | Join Public Event | High | As an Authenticated User, I want to be able to join a public event so that I can let the event organizer know that I am going to their event.
US307 | Manage My Events | High | As an Authenticated User, I want to manage my events so that I can organize them.
US308 | Manage Events Attended / to Attend | High | As an Authenticated User, I want to manage the events I attended/to attend so that I can organize my personal schedule accordingly.
US309 | Accept/Refuse Invitation | Medium | As an Authenticated User, I want to be able to accept or refuse an invitation to a private event so that I can see more details, participate on the forum and let the event organizer know that I am going to their event.
US310 | Delete Account | Medium | As an Authenticated User, I want to delete my account so that I no longer share my information with the platform.
US311 | Support Profile Picture | Medium | As an Authenticated User, I want to upload a profile picture so that I can be more easily recognized by other users.
US312 | View Personal Notifications | Medium | As an Authenticated User, I want to be able to view my personal notifications so that it will be easier for me to manage my attendance.
US313 | Notification of an Invitation to Event | Medium | As an Authenticated User, I want to receive a notification of an invitation to an event so that I can see the invitation as soon as possible.
US314 | Recommended Events | Medium | As an Authenticated User, I want to be able to, optionally, answer a few questions during the registration so that events of my interest can be recommended to me.
US315 | Report Event | Low | As an Authenticated User, I want to report an event so that an inappropriate event is removed from the platform.
US316 | Manage Invitations Received | Low | As an Authenticated User, I want to manage the invitations I received so that I can decide if I want to accept or refuse it.
US317 | Follow Organizers of my Interest | Low | As a Authenticated User, I want to follow the event organizers of my interest so that I can be keep up with their content.

*Table 4: Authenticated User's User Stories*

#### 2.4. **Attendee**

| Identifier | Name | Priority | Description |
| --- | --- | --- | --- |
US401 | View Event’s Messages | Medium | As an Attendee, I want to view the event’s forum messages so that I can see what other attendees are discussing.
US402 | Add Comments | Medium | As an Attendee, I want to add a comment on the event’s forum so that I can express my opinion or ask a question.
US403 | Answer Polls | Medium | As an Attendee, I want to answer polls so that I can express my opinion on a certain subject.
US404 | Upload Files | Medium | As an Attendee, I want to be able to upload files in the event’s forum so that I can share them with all the guests that are also going.
US405 | Vote in Comments | Medium | As an Attendee, I want to vote in comments so that I can agree or disagree with another attendee's post
US406 | View Attendees List | Medium | As an Attendee, I want to view the attendees list of an event so that I can see who is going
US407 | Leave Event | Medium | As an Attendee, I want to be able to leave an event so that I don’t see more about the event and people know that I am not going.
US408 | Edit Comment | Medium | As an Attendee, I want to edit my comment so that I can update or correct it
US409 | Delete Comment | Medium |  As an Attendee, I want to delete my comment so that the comment is no longer visible to other attendees.

 
*Table 5: Attendee's User Stories*

#### 2.5. **Event Organizer**

| Identifier | Name | Priority | Description |
| --- | --- | --- | --- |
US501 | Edit Event Details | High | As an Event Organizer, I want to edit an event's information, such as name, date, and location, so that I can keep it up to date. 
US502 | Manage Event Participants | High | As an Event Organizer I want to be able to manage my event participants so that I can know exactly each person's permissions.
US503| Invite User to my Private Event | Medium | As an Event Organizer, I want to invite users to my private event by their username so that they can access the event’s information and decide to come. 
US504| Create Polls | Medium | As an Event Organizer, I want to create polls so that I can know the opinion of the attendees about a certain matter.
US505 | Cancel Event | Medium | As an Event Organizer, I want to cancel an event so that I can inform the attendees that it will no longer be carried out.
US506 | Manage Event Visibility | Medium | As an Event Organizer, I want to manage the event visibility so that I can decide if the event is public (for anyone) or private (only for certain people).
US507| Send Invitations using external API | Low | As an Event Organizer, I want to send an invitation using mine and another user’s e-mail account so that I have another option to invite users.

*Table 6: Event Organizer's User Stories*

#### 2.6 **Administrator**

| Identifier | Name | Priority | Description |
| --- | --- | --- | --- |
US601 | Administer User Accounts | High | As an Administrator, I want to be able to administer user accounts so that I can have control over their actions and keep the system clean and appropriate.
US602 | Block and Unblock User Accounts | Medium | As an Administrator, I want to block or unblock a user from the system so that I can control their access to restricted contents of the site.
US603 | Browse Events | Medium | As an Administrator, I want to browse events so that I can verify the platform is behaving as expected.
US604 | View Event Details | Medium | As an Administrator, I want to view event details, like location or description, so that I can confirm the event is appropriate. 
US605 | Manage Event Reports | Medium | As an Administrator, I want to manage event reports so that inactive, false, or inappropriate events cannot be seen. 
US606 | Delete Event | Medium | As an Administrator, I want to delete an event so it will disappear completely from the platform. 
US607 | Delete User Account | Medium | As an Administrator, I want to be able to delete a user account so it will disappear completely from the platform.

*Table 7: Administrator's User Stories*

### 3. Supplementary Requirements

#### 3.1. **Business Rules**

| Identifier | Name | Description |
| --- | --- | --- |
BR01 | Deleted User | When a user deletes their account, their activity (events history, comments, etc.) is kept but will be associated with an anonymous user. 
BR02 | Event’s Visibility | Events can be public or private. Private events are not shown in search results and can only be viewed by their own organizer and attendees. 
BR03 | Administrator User | Administrator accounts are independent of the user accounts, i.e., they cannot create or participate in events. 
BR04 | Interaction with own comments | A user cannot vote or comment in their own comment.
BR05 | Event History | The "quitting an event" date must be after the "accept invitation" date and before the "conclusion of event" date. (accept invitation <= quitting event <= conclusion of event).

*Table 8: WeMeet Business Rules' Table*

#### 3.2. **Technical Requirements**

| Identifier | Name | Description |
| --- | --- | --- |
TR01 | Availability | In each 24 hour period, the system must be available 99 percent of the time.
**TR02** | **Accessibility**| **The system must ensure that everyone can access the pages, regardless of whether they have any handicap or not, or the Web browser they use. This platform can be used by anyone, so it must be prepared to have considerable accessibility to everyone.**
**TR03** | **Usability** | **The system should be intuitive and straightforward to use. Our goal is to encourage in-person meetings, and to do so, our website must be easy and simple to use to help our users engage in a new and better way of life.**
TR04 | Performance | The system should have response times shorter than 2 s to ensure the user's attention. 
TR05 | Web Application | The system should be implemented as a web application with dynamic pages (JavaScript, HTML5, CSS3 and PHP).
TR06 | Portability | The server-side system should work across multiple platforms (Mac OS, Linux, etc.).
TR07 | Database | The database management system used should be the PostgreSQL system, with a version of 11 or higher.
TR08 | Security | The system must protect information from unauthorized access using an authentication and verification system.
TR09 | Robustness | The system must be prepared to handle and continue operating when runtime errors occur.
TR10 | Scalability | The system must be prepared to deal with the growth in the number of users and their actions.
**TR11** | **Ethics** | **The system must respect the ethical principles in software development (for example, personal user details should not be shared without the owner knowing). Society interacts with software and we believe it is important for it to be guided by good and moral principles to make sure it improves quality of life for everyone.**

*Table 8: WeMeet Technical Requirements' Table*




#### 3.3. **Restrictions**
|Identifier | Name | Description
| --- | --- | --- |
C01 | Deadline | The system should be ready to use by the end of the semester 
C02 | Event Administration | There cannot be events who don't have an organizer, i.e, if the event organizer deletes their account, the event should be carried out with another administrator. 

*Table 9: WeMeet Restrictions' Table*

---


## A3: Information Architecture

> This artifact contains a sitemap and some wireframes to display a better representation of WeMeet and what the general design of the main pages will be.


### 1. Sitemap

> WeMeet's system is organized into 4 main areas: the pages with administration features (Administration), the pages with authentication features (Authentication), the static pages that provide information about our system to our clients (Static Page), the individual user pages (User Page), and the pages used to explore and access events (Event Page).

![](https://i.imgur.com/IrQzwwS.png)

*Fig.2: WeMeet Sitemap*

### 2. Wireframes

> Wireframes for the Home Page (UI01), Event Feed (UI14), and Event Forum Page (UI16) are displayed in the images below.


#### UI01: Home Page

![](https://i.imgur.com/l2tcB73.png)


*Fig.3: Home Page Wireframe*


#### UI13: Event Feed

![](https://i.imgur.com/3CFRK1i.png)


*Fig.4: Event Feed Wireframe*


#### UI14: Event Forum Page

![](https://i.imgur.com/luTuaB2.png)


*Fig.5: Event Forum Page Wireframe*



---


## Revision history

Changes made to the first submission:

1. 24/09/2022 - ER Created
2. 27/09/2022 - Added content related to A1
3. 28/09/2022 - Added content related to A2
4. 03/09/2022 - Added content related to A3
5. 17/10/2022 - Updated wireframes - A3
6. 20/10/2022 - Updated A1
7. 21/10/2022 - Updated and added new user stories (recover password; API) - A2
***

GROUP22102, 21/10/2022

* Bruna Marques, up202007191@edu.fe.up.pt (editor)
* Francisca Guimarães, up202004229@edu.fe.up.pt 
* Mariana Teixeira, up201905705@edu.fe.up.pt
* Martim Henriques, up202004421@edu.fe.up.pt