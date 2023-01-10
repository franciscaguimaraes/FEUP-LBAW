# PA: Product and Presentation

> To facilitate events’ management and choices for people by making them organized, simple, and intuitive.

## A9: Product

WeMeet is a web-based information system for managing various types of events, aiming to become a useful product targeted at individual users that want to attend and/or administer events.

Throughout the years, there has been a continuous lack of in-person connection between people. Therefore, we believe this is a tool that would intuitively encourage more social interactions as it can be used by anyone, from families to groups of friends to work meetings. 
With WeMeet, our clients will be able to create and attend events in a very simple, reliable, and organized way. Our website will have an intuitive design to provide a responsive and enjoyable browsing experience on either desktop or smartphone devices. Upon entering our website, our clients will be presented with a simple home page containing a brief overview of our main purpose.

Our clients will be categorized into groups with different permissions. 
These groups include non-authenticated users, users who don’t have an account, who will only be able to view information about public events and utilize a search bar that allows them to search for events of their interest using tags. All events, public or private, will present a forum where guests can interact with each other by uploading files or adding comments. Non-authenticated users may only view public forums.

The authenticated users, users who have created an account, will, in addition to the privileges mentioned above, have in their user profile their personal attendance calendar with the history of events they have gone to or are going to. They can accept or refuse invitations, and interact and share their experiences in public forums as they wish.

The authenticated users can be attendees, and, in case they create an event, or are granted permission to edit said event, event organizers.
The attendees, users who have accepted an invitation to an event, can be attendees of a public or a private event. If they attend a private event, they will have access to a private forum that can only be seen by the event organizer and attendees. The event organizers, users who created an event, will have complete access and editing privileges to the event’s information, including adding more advanced features such as the creation of polls. The event organizer can also grant editing privileges to the attendees whenever they want.

The Administrators are users that will have the same permissions as the above-mentioned groups, with the addition of having event/user management and supervision responsibilities. Their goal is to keep the platform clean and appropriate, providi

### 1. Installation

> Link to the release with the final version of the source code in the group's Git repository.  

```
composer install

docker login git.fe.up.pt:5050

./upload_image.sh

docker run -it -p 8000:80 --name=lbaw22102 -e DB_DATABASE="lbaw22102" -e DB_SCHEMA="lbaw22102" -e DB_USERNAME="lbaw22102" -e DB_PASSWORD="XKPDfBZS" git.fe.up.pt:5050/lbaw/lbaw2223/lbaw22102
```

### 2. Usage

> URL to the final product: http://lbaw22102.lbaw.fe.up.pt  

#### 2.1. Administration Credentials

> Administration URL: URL  

| Email | Password |
| -------- | -------- |
| admin1_wemeet@gmail.com    | password |

#### 2.2. User Credentials

| Type          | Email  | Password |
| ------------- | --------- | -------- |
| basic user | mariamota2002@gmail.com    | password |
| event organizer   | manuelassteves33@gmail.com    | password |

### 3. Application Help

> There are help buttons implemented on the footer of our pages. For example see the button "User Help" on the bottom right corner of footer.

### 4. Input Validation

User input is validated in both client-side and server-side. For client-side, when creating an event, the "title" and "visability" are both required inputs. For server-side, also when creating an event, besides checking if both the inputs above-mentioned are not null, it also checks their type, using the "validate" method of Laravel.

### 5. Check Accessibility and Usability

> Results of accessibility and usability tests.

 Accessibility: [Checklist_de_Acessibilidade_-_SAPO_UX.pdf](uploads/6d0b9ee2973b4f1aeaf07f4d527172c8/Checklist_de_Acessibilidade_-_SAPO_UX.pdf)

 Usability: [Checklist_de_Usabilidade_-_SAPO_UX.pdf](uploads/01c198554b0daf4efbf69f2c9029a6ed/Checklist_de_Usabilidade_-_SAPO_UX.pdf) 

### 6. HTML & CSS Validation

> Results of the validation of the HTML and CSS code using the following tools. 

[css.pdf](uploads/7b2e06f728fd6808ea38f2bb8ec3afa5/css.pdf)

 [mycalendar.pdf](uploads/436e5459bd8245a46faa130f6ff841d3/mycalendar.pdf)

 [myevents.pdf](uploads/06fe5fffffcbe993708df41c7ea33ae4/myevents.pdf)

[profile.pdf](uploads/572e173bfcfb55e790365283c3f9e2aa/profile.pdf)

[userhelp.pdf](uploads/0ca6942e8bdab769028134f91dcbaede/userhelp.pdf)

 [aboutus.pdf](uploads/be0fbf6a94c9cae3255c7014d989616a/aboutus.pdf)

 [contacts.pdf](uploads/180336eb7983b1a5d0eea57a7250e09e/contacts.pdf)

 [createEvent.pdf](uploads/5a8d87358cff733d37f7cdce577857b1/createEvent.pdf)

 [editprofile.pdf](uploads/3d9efac1fe63d4eb5581c52276441378/editprofile.pdf)

 [eventPage.pdf](uploads/45222bdda6568b05b09f091da1362c87/eventPage.pdf)

 [feed.pdf](uploads/fe1a1f6295c503458422aa60737f94a5/feed.pdf)

 [landing_page.pdf](uploads/7eddbb9fec4dcc916c72ed7ccb92667c/landing_page.pdf)

### 7. Revisions to the Project

We implemented the contact us page, but instead of being a static page, it allows the user to send an email via mailtrap.
We also added a new boolean attribute to the invite class to know if the event is canceled.

### 8. Implementation Details

#### 8.1. Libraries Used

[Bootstrap](https://getbootstrap.com/) and [Laravel](https://laravel.com/) were the common libraries used for the project.

#### 8.2 User Stories

| Identifier | Name | Module | Priority  | Team Member | State  |
| --- | --- | --- | --- | --- | ---|
US101 | Browse Public Events | M02: Events | High | **Bruna Marques** | 100% |
US102 | View Public Event | M02: Events | High |  **Bruna Marques** | 100% |
US103 | Search | M02: Events |  High | **Mariana Teixeira**| 60%
US201 | Login | M01: Authentication and Individual Profile | High | **Francisca Guimarães**| 100% |
US202 | Registration | M01: Authentication and Individual Profile | High | **Bruna Marques**| 100% |
US301 | Log out | M01: Authentication and Individual Profile | High | **Francisca Guimarães**| 100% |
US302 | Edit Profile | M01: Authentication and Individual Profile | High | **Francisca Guimarães**| 100% |
US303 | View Profile | M01: Authentication and Individual Profile | High | **Francisca Guimarães**| 100% |
US304 | Create Event | M02: Events | High | **Mariana Teixeira**| 100% |
US305 | Share Public Event with Users| M02: Events | High | **Francisca Guimarães**| 100% |
US306 | Join Public Event | M02: Events | High | **Mariana Teixeira**| 100% |
US307 | Manage My Events | M02: Events | High | **Bruna Marques**| 100% |
US308 | Manage Events Attended / to Attend | M02: Events | High |**Bruna Marques**| 100% |
US501 | Edit Event Details | M02: Events | High | **Mariana Teixeira**| 100% | 
US502 | Manage Event Participants | M02: Events | High | **Martim Henriques**| 100% |
US601 | Administer User Accounts | M04: User Administration | High | **Martim Henriques**| 100%
US105 | About US | M05: Static pages | Medium | **Francisca Guimarães**| 100% |
US106 | Contacts | M05: Static pages | Medium | **Mariana Teixeira**| 100%
US107 | User Help | M05: Static pages | Medium | **Francisca Guimarães**| 100% | 
US203 | Recover Password | M01: Authentication and Individual Profile | Medium | **Francisca Guimarães**| 100% |
US309 | Accept/Refuse Invitation | M02: Events | Medium | **Mariana Teixeira**| 100%
US310 | Delete Account | M01: Authentication and Individual Profile | Medium | **Francisca Guimarães**| 100% |
US311 | Support Profile Picture | M01: Authentication and Individual Profile | Medium | **Francisca Guimarães**| 100% |
US312 | View Personal Notifications | M01: Authentication and Individual Profile | Medium | **Mariana Teixeira**| 100%
US313 | Notification of an Invitation to Event | M02: Events | Medium | **Mariana Teixeira**| 100%
US401 | View Event’s Messages | M03: Messages and Forum |  Medium | **Bruna Marques**| 100% |
US402 | Add Comments | M03: Messages and Forum |  Medium | **Bruna Marques**| 100% |
US405 | Vote in Comments | M03: Messages and Forum | Medium | **Bruna Marques**| 100% |
US406 | View Attendees List | M02: Events | Medium | **Bruna Marques**| 100% |
US407 | Leave Event | M02: Events | Medium | **Mariana Teixeira**| 100% |
US408 | Edit Comment | M03: Messages and Forum |  Medium | **Bruna Marques**| 100% |
US409 | Delete Comment | M03: Messages and Forum |  Medium | **Bruna Marques**| 100% |
US503 | Invite User to my Private Event | M02: Events | Medium | **Mariana Teixeira**| 100% 
US505 | Cancel Event | M02: Events | Medium | **Francisca Guimarães**| 90% | 
US506 | Manage Event Visibility | M02: Events | Medium | **Mariana Teixeira**| 100% | -%
US603 | Browse Events | Medium | M02: Events | **Martim Henriques**| 100%
US604 | View Event Details | Medium | M04: User Administration | **Martim Henriques**| 100%
US605 | Manage Event Reports | M04: User Administration | Medium | **Mariana Teixeira**| 100%
US606 | Delete Event | M04: User Administration | Medium | **Martim Henriques**| 100%
US607 | Delete User Account | M04: User Administration | Medium | **Martim Henriques**| 100%
US315 | Report Event | M02: Events | Low | **Bruna Marques** | 100% |

---


## A10: Presentation
 

### 1. Product presentation

All users access the Home page to see the feed of public events and search for them.
In the my events section, after selecting an event in which the authenticated user is an event organizer, he can remove an attendee or turn an attendee into an event organizer and edit the event. Besides that, he can add, edit, like or delete a comment and leave the event, as well as an attendee. In a private event, the event organizer can invite a user. Then, the invited user receives a notification and can accept/refuse the invitation.
If an attendee reports an event, the admin can ignore the complaint or block the event in the reports page.

The link to the final product is the following: http://lbaw22102.lbaw.fe.up.pt

### 2. Video presentation

![](https://i.imgur.com/yiJcX3t.png)

The video presentation can be found here: [Video](https://drive.google.com/file/d/1ruStd6t0iiaC-VoekVfuu9NtzNiV8YXC/view?usp=sharing)

---


## Revision history

Changes made to the first submission:
1. 03/01/2023 - Added conted related to A9 and A10
2. 03/01/2023 - Updated A7

***
GROUP22102, 23/09/2022

* Bruna Marques, up202007191@edu.fe.up.pt
* Francisca Guimarães, up202004229@edu.fe.up.pt
* Mariana Teixeira, up201905705@edu.fe.up.pt (editor)
* Martim Henriques, up202004421@edu.fe.up.pt