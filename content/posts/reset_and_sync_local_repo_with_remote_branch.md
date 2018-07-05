+++
title =  "Reset and sync local repo with remote branch"
date = 2018-07-04T14:00:32+02:00
featured_image = ""
description = ""
tags = []
categories = ['git']
+++

# *Attention: This command will destroy any local changes in your current branch!!!*


Initial tree

    * ac35af91 ...
    * 712d825e ...
    * 21168829 ...
    *   eeb840ea (origin/master, origin/HEAD) Merged in feature-1 (pull request #N)
    |\  
    | * f4b3f37b (origin/feature-1, feature-1) ...
    |/  
    | * e12dc3e2 (HEAD -> master) ...
    |/  
    * 056b1367 ...
    * e21f30c9 ...


Execute

    git fetch origin
    git reset --hard origin/master
    git clean -f -d

    =>
    ...
    HEAD is now at eeb840ea Merged in feature-1 (pull request #N)


Final tree

    * ac35af91 ...
    * 712d825e ...
    * 21168829 ...
    *   eeb840ea (HEAD -> master, origin/master, origin/HEAD) Merged in feature-1 (pull request #N)
    |\  
    | * f4b3f37b (origin/feature-1, feature-1) ...
    |/  
    * 056b1367 ...
    * e21f30c9 ...
