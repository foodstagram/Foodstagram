# FOODSTAGRAM

## Table of Contents
1. [Overview](#Overview)
1. [Product Spec](#Product-Spec)
1. [Wireframes](#Wireframes)
2. [Schema](#Schema)

## Overview
### Description
Foodstagram is a convenient app to show off all your food. Keep all your meals on your own feed, and share with friends!

### App Evaluation
- **Category:** Social Networking / Food
- **Mobile:** Mobile version would be easiest to use, due to direct picture uploading, but other platforms can work too.
- **Story:** Post and view feeds of foods
- **Market:** Any individual can use this app, since people of all ages and parts of the world like to photograph their food.
- **Habit:** This app can be habit forming for those who liek to take photos of their food, or to check on food.
- **Scope:** First this project would target users who are already interested in food, such as food blog sand food videos. Then we can broaden it to the general public. 

## Product Spec

### 1. User Stories (Required and Optional)

**Required Must-have Stories**

- [x] User can post new photo to feed
- [x] User can create new account
- [x] User can login
* User can view a feed of photos

**Optional Nice-to-have Stories**

* User can follow/unfollow another user
* User can view photos based on hashtags
* Integration with yelp to tag food photos with restaurants


### 2. Screen Archetypes

* Welcome Page
   * User can create account/ login
* Home Page feed
   * User can view a feed of photos
* Detailed Photo Page
    * View comments on photo, and detailed post description
* User Posts
    * Feed of user's profile posts
* Add Post
    * Screen to select photo and write description
* Settings & Logout
    * Change settings and logout of app

### 3. Navigation

**Tab Navigation** (Tab to Screen)

* My Feed - personal posts
* Home -  feed of posts
* Account - settings

**Flow Navigation** (Screen to Screen)

* Login/ Signup
   * login -> Home
   * signup -> verification -> home
* Home Feed
   * when photo clicked -> detailed photo view
* All Screens after logging in
    * Add post

## Wireframes
<img src="https://i.imgur.com/yGGDaZd.png" width=600>




## Schema 
### Models
#### Post
  | Property      | Type     | Description |
   | ------------- | -------- | ------------|
   | objectId      | String   | unique id for the user post (default field) |
   | user        | Pointer to User| image creator |
   | image         | File     | image that user posts |
   | caption       | String   | image caption by user |
   | createdAt     | DateTime | date when post is created (default field) |
   | updatedAt     | DateTime | date when post is last updated (default field) |

### Networking
#### List of network requests by screen
   - My Posts Feed Screen
      - (Read/GET) Query all posts where user is author
         ```swift
         let query = PFQuery(className:"Post")
         query.whereKey("user", equalTo: currentUser)
         query.order(byDescending: "createdAt")
         query.findObjectsInBackground { (posts: [PFObject]?, error: Error?) in
            if let error = error { 
               print(error.localizedDescription)
            } else if let posts = posts {
               print("Successfully retrieved \(posts.count) posts.")
            }
         }
         ```

   - Home Feed Screen
      - (Read/GET) Query random posts by all users
         ```swift
         let query = PFQuery(className:"Post")
         query.findObjectsInBackground { (posts: [PFObject]?, error: Error?) in
            if let error = error { 
               print(error.localizedDescription)
            } else if let posts = posts {
               print("Successfully retrieved \(posts.count) posts.")
            }
         }
        ```

   - Create Post Screen 
      - (Create/POST) Create a new post object
        ```swift
        override func viewDidAppear(_ animated: Bool) {
        super.viewDidAppear(animated) 
        let query = PFQuery(className:"Posts")
        query.includeKey("user")
        query.limit = 20
        
        query.findObjectsInBackground { ( posts, error ) in
            if posts != nil {
                self.posts = posts!
                self.tableView.reloadData()
        } }
         ```
  
   - Profile Screen
      - (Read/GET) Query logged in user object
        ```swift
        let profile  = PFObject(className: "Profile")
         member = PFUser.current()!
        ```

