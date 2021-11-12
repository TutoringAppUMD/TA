Original App Design Project - README Template
===

# App name: TA
### Creators: 
Diana Del Cid, Leonel Santos, Riddhima Rai, Yuzhu Fu

## Table of Contents
1. [Overview](#Overview)
1. [Product Spec](#Product-Spec)
1. [Wireframes](#Wireframes)
2. [Schema](#Schema)

## Overview
### Description
A tutoring app that connects students and teaching assistants according to subjects or skills.

### App Evaluation
- **Category:** Education
- **Mobile:** Interactive, connecting students with TA's
- **Story:** Allows students to seek help from teaching assistant/connect with them, and post questions or problems 
- **Market:** Any student who seeks help can register for an account; any TA's who are willing to answer questions and provide meaningful and correct solutions may also create an account
- **Habit:** Students and TA's may give the app their preferences (specific subject or skills) or goals (what they want learn), then the app will match them based on those specifications.
- **Scope:** Main focus is to match students with the appropriate TA. App can be expanded to support students in posting questions, TA's may answer the question in a comment under the post but direct messages will also be available.

## Product Spec
### 1. User Stories (Required and Optional)

**Required Must-have Stories**

* User can create an account based on whether he/she is a student or TA.
* User can log in
* User can remain logged in across restarts
* User can log out
* User specificies subjects needed if he/she is a student or skills if he/she is a TA. 
* User can select to skip or see next match, or user can send a quick chat.
* User can view main feed (matches only): photo posts (problems) and captions (specific questions) this normally by a student user and comments (solutions) by a TA.
* User can post questions/photo problems
* User can comment under posts with solutions
* User can send direct message a match

**Optional Nice-to-have Stories**

* Profile pages/pictures for each user
* Settings (Accesibility, Notification, General, etc.)

### 2. Screen Archetypes

* Login 
* Register - User signs up or logs into their account
   * Upon Download/Opening of the application, the user is prompted to log in or sign up to gain access to skill or subject preferences to be properly matched (student -> TA). 
* Subject/ Skill Selection Screen - User may select subjects or skills
   * Upon selection matches will be found and user eill be taken ro the match or un,atch selection screen. 
* Match Screen - User may view matches specifications (subjects/skills, and decide whether to unmatch (next/skip) or quick chat
* Quick Chat Screen - Chat for users to communicate upon matching (direct 1-on-1)
   * Upon registering users will be matched to either a student or TA depending on the account created, users can choose to send a quick chat to get to know one another or unmatch. Quick chats end up in direct messages.
* Feedview Screen 
   * Users can view photos of problems, caption (specific questions), and comments (solutions) under posts. Logout button and camera button.
* Gallery Screen
   * Allows user to select a photo of a question or problem (student) for their matches to see, and write a caption (specific question) 
* Comment Screen 
   * After a user clicks comment on a post they will be taken to a seperate screen where they may type in their solution.
*  Direct Message Screen
   * Allows user to directly message current matches this differs from the quick chat screen because it has to be used after a quick chat has been sent to a match to continue messaging.

### 3. Navigation

**Tab Navigation** (Tab to Screen)

* Feed (Home) 
* Direct Messages
* Subject/Skill selection

Optional:
* Settings
* Profile Page

**Flow Navigation** (Screen to Screen)
* Log-in -> Account creation if no log in is available or automatically remains logged in across restarts
* Student or TA verification -> Subject or Skill Selection
* Match or Unmatch -> Text field to send quick chat.
* Main feed -> Can upload photos of questions with caption, may post comments under posts, can logout
* Direct Message -> Can directly message with a specific match 

## Wireframes
![image](https://user-images.githubusercontent.com/89175881/140597724-8e0864fd-b8cf-431e-b682-078faecec2c2.jpeg)

## Schema
### Models
#### Post

   | Property      | Type     | Description |
   | ------------- | -------- | ------------|
   | objectId      | String   | unique id for the user post (default field) |
   | author        | Pointer to User| image author |
   | image         | File     | image that user posts |
   | caption       | String   | image caption by author |
   | comments      | String   | comments that has been posted to an image |
   | chatbox       | Array[Messages]    | A placeholder of messages in chronological order |
   | message       | class    | Wrap the sender, receiver and the text messages(string format) in this class |
   | createdAt     | DateTime | date when post is created (default field) |
   
### Networking
#### List of network requests by screen
   - Register/Login Screen
      - (Create/POST) Create a new user and provide it with some predifined options for the app to match later
   - Home Feed Screen
      - (Read/GET) Query all posts where user is author
         ```swift
         let query = PFQuery(className:"Post")
         query.whereKey("author", equalTo: currentUser)
         query.order(byDescending: "createdAt")
         query.findObjectsInBackground { (posts: [PFObject]?, error: Error?) in
            if let error = error { 
               print(error.localizedDescription)
            } else if let posts = posts {
               print("Successfully retrieved \(posts.count) posts.")
           // TODO: Do something with posts...
            }
         }
         ```
      - (Create/POST) Create a new comment on a post
   - Create Post Screen
      - (Create/POST) Create a new post object
      - (Create/POST) Creat a new caption under an image
   - Profile Screen
      - (Read/GET) Query logged in user object
      - (Update/PUT) Update subjects/ skills as modified 
   - Message Screen
      - (Creat/POST/Read/GET) Load eisting messages, create new messages whenever the user wants to connect someone


