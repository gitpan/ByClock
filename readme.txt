This is the readme file for Schedule::ByClock

I had the need myself. I needed to run a perl script
24 hours a day. Every minute, when the seconds showed
00, my routine wakes up and does something. When the
seconds show 45, my routine wakes up and does something
else.

The problem with the sleep() call in perl (and may
other languages as well) is that sleep() returns *after*
a given number of seconds. I needed it to return *at* a
given time...

I wrote the code for it and everything was nice.

Then I needed to do the same thing in another routine.

Soo, Schedule::ByClock was born. As you may read in the
POD, it's freeware. I'd still apreciate to get a sign
of life if you use it and apreciate it. If you do not
apreciate it, then I would sicerely want to know why.
I may enhance ByClock, but that will not happen by
itself. I will do that only on request...

Me:
gschaffter@cyberjunkie.com
http://www.schaffter.com
