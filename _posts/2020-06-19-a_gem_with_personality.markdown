---
layout: post
title:      "A Gem with personality ٩( 'ω' )و"
date:       2020-06-19 23:54:34 -0400
permalink:  a_gem_with_personality
---


Creating this moody Healthy Weather gem has been a journey. I basically worked on each method at least three times... and had to gave up one function that I wanted to have. So I can't wait for this blog to document my own growth.


# What is Healthy Weather gem
This gem basically tells you the current weather of the location you type in. If the weather is nice, it will show you a list of national parks in the state of the location. You can learn more information about the park of your choice or choose a different city to look at.
The weather can be reported for around the world. But for the curretn version the park info is only available for the locations within the U.S.
This Gem has strong personality. How do I know that (not just bc I'm the mom duh)? It talks with facial expressions and it makes typos without being sorry. Huge attitude, right?


Now let's talk about my journey:
# 1. Error keeps coming back bc...?
I used two differet API and a JSON file I downloaded locally. For documentation purpose, I will specify the two ways I used to apply API key below:

1st way:
```
require 'rest-client'
require 'json'

    response = RestClient::Request.execute(
        method:   :get,
        url:       url,
        headers:   {'X-Api-Key': 'BweVtjeqnX4rjRIEaUVgld1wx2X7V1fx7JEBreeb'}
    )
```


2nd way is provided by the Rapid API website:
```
require 'uri'
require 'net/http'
require 'openssl'
require 'json'

        http = Net::HTTP.new(url.host, url.port)
        http.use_ssl = true
        http.verify_mode = OpenSSL::SSL::VERIFY_NONE
        
        request = Net::HTTP::Get.new(url)
        request["x-rapidapi-host"] = 'host_website'
        request["x-rapidapi-key"] = 'your_subscription_key'
        
        response = http.request(request)
```

After doing the setup, when I test with the CLI, there are more errors coming out about how I formatted the API data, which is perfectly normal. After I fixed the errors, however, it came back when I was working on other errors. I was very upset for a moment. Thought maybe there are some relationship between the variables that I messed up. But it turns out:

```
[1] pry(#<City>)> response_j
=> {“message”=>
  “You have exceeded the MONTHLY quota for Zip Boundary Endpoints on your current plan, BASIC. Upgrade your plan at…}”
```

The only response I had is: What's more guilty than being poor...?


# 2. API too slow!
The national park API I used works very slow. For state that has 30 something parks, it will take almost half a minute to load all the data. So my initial plan was to load all the parks in the country (400+ parks) at the beginning of the execution and save it in a variable. This way, no matter how many cities the user looks for, I can respond fast.
But... it turns about the app has a limitation of 50 items per fetch. So even if I look for all the parks, it will only return 50.

Ok, it's annoying, but not too bad. So I search for all the parks in a single state once and save that as a variable. When the user wants to check another park from the same state, I save the time by referring to the variable I saved.


# 3. Don't break me!!
Every time I test a location with random input, there will be an error. I finished the structure in about a day and half and spent two days keep testing different errors and modify my code. My work includes:
 - Preventing missing information in the API database to break my gem. For example, some parks have missing information in entrance fee field, or operating hours field, or park alerts. So if I used the structure of normally filled park to fetch the data, it will show errors such as no method for Nil Class.
 - Sanitizing input when users put city names such as "nEW yOrk" and display it in the correct format when I puts the weather conditoin.
 - and many more... 
 But I still cannot be 100% confident. If you encountered any errors, please let me know! I'm more than happy to fix it for you :)


# 4. Set up error messages...
Setting up a flow diagram is very helpful to find:
 - How many error messages should be given
 - What are the optimal places to put them
 - When the user typed undesired input, how to let them enter a new one every time.
For unable to upload my hand drawn diagram to the site. I found a example from the web:
![](https://i.pinimg.com/originals/8a/de/ec/8adeec11292e6cf4c626d3c34b9919d4.png)


All in all, it is still far from being perfect. But I learnt so much and it is a happy Healthy Weather gem. I hope you enjoy using it~
