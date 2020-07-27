---
layout: post
title:      "Project Management Tool for Teams"
date:       2020-07-26 23:16:21 -0400
permalink:  project_management_tool_for_teams
---


In my past work, my team wants to find a tool to mange the previews that our gallery clients send to their clients through our platform. But existing project management tools either do not have a comprehensive client facing portal, or do not have robust associations between the tables and the tickets.

In this project, I picked up the requirements we had and created an ideal version of app that we were looking for! These are the things I learnt from the process:

<not done writing yet>

# Delete is not Destroy
```
class Category < ActiveRecord::Base
  has_many: :posts,
             dependent: :nullify
end
```

# Master has_and_belongs_to_many alternatives

# User belongs_to Teams
Teams are private working space, it cannot be randomly entered by anyone who wants to. To achieve this, I was inspired by the design of Monday.com's system design. But mine is better X)
In this Project Management Tool:
* When user creates a team, the user automatically became the admin user of the team.
* If a user of the team invite a new user by entering the new user's email, when the invited user creates an account, it can be automatically directed to the team's page.
* if a user creats an account without any invitation from the platform, the user will be directly to create his/her own team.

