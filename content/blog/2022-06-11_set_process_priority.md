+++
title = "set process priority"

[taxonomies]
tags = ["linux", "bash", "io", "nice"]
+++

To check niceness: ```ps -eo pid,ppid,ni,comm```


or `NI` column in `htop`.


To set cpu priority: ```nice -n -10 …```


To set io priority: ```ionice ...```.

It is composable with `nice` as child processes inherit parent attributes.

Sources:
* <https://en.wikipedia.org/wiki/Nice_(Unix)>
* <https://linux.die.net/man/1/nice>
* <https://linux.die.net/man/1/ionice>
* <https://www.tecmint.com/set-linux-process-priority-using-nice-and-renice-commands/>
* <https://en.wikipedia.org/wiki/Child_process>
* <https://stackoverflow.com/questions/45970377/combining-ionice-and-nice-in-linux-and-transitive-priorities>