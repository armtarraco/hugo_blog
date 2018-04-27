+++
date = "2018-04-27T09:13:19+02:00"
title = "Rails debug to STDERR"

+++
Write to STDERR ...

<!--more-->

    def my_method(*args)
    $stderr.puts "Hello from #my_method!"
    
    # ...
    end
    
    

… and temporarily silence STDOUT, like the SOLO button on a mixing board:

    bin/rails server 1>/dev/null