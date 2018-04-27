+++
date = "2018-04-27T11:38:03+02:00"
draft = true
title = "Compare files in different branches with Git"

+++
Find changes with Git comparing files between branches.

<!--more-->

    git diff <old-branch> <new-branch> -- file-url

Example:

    diff --git a/app/views/api/v2/messages/_show.json.jbuilder b/app/views/api/v2/messages/_show.json.jbuilder
    index b9f8bd74..231f83c0 100644
    --- a/app/views/api/v2/messages/_show.json.jbuilder
    +++ b/app/views/api/v2/messages/_show.json.jbuilder
    @@ -15,7 +15,8 @@ json.user_id      message.user_id
     json.user do
       json.id         message.user.id
       json.full_name  message.user.full_name
    -  json.image      message.user.image.url
    +  json.image      message.user.image
    +  json.image_url  message.user.get_image
     end
    