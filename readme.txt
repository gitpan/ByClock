NAME
    Schedule::ByClock - Give back the control to the caller at given
    times.

SYNOPSIS
       use Schedule::ByClock;

       # Constructor
       $th = Schedule::ByClock->new([time [,time [,...]]]);

       # Methods
       @times = $th->set_control_list([time [,time [,...]]]);
       @times = $th->get_control_list();

       $second = $th->get_control_on_second([second [,second [,...]]]);
       $minute = $th->get_control_on_minute([minute [,minute [,...]]]);

       $version = $th->get_version();

DEPENDENCIES
    Schedule::ByClock use the Time::localtime module.

DESCRIPTION
    This module implements an 'intelligent' (?) layer over sleep().
    Call the module when you want to sleep to a given second in the
    minute, or to a given minute in the hour, without having to
    calculate how long to wait.

    Use with multiple 'time' values to sleep until the
    chronologically first 'time' in the list.

    Note that all times used in Schedule::ByClock are calculated
    from the local time in the computer where Schedule::ByClock is
    executed.

USAGE
    Assume that you want to do something repeatedly every minute,
    when the seconds show for instance 34.

    Assume that you want to do something (maybe just once) the next
    time the seconds in the computer shows 23. Assume that 'now' is
    18. You would need to use sleep and to calculate how many
    seconds there are from 'now' till 23. Easy, 23 - 18 = 5.
    sleep(5); Then, assume that 'now' is 28. I.e. 23 - 28 = 55.
    (Huh?)

    Assume that you want to do something repeatedly, when the
    seconds show either 12 or 45 or 55, whichever comes first
    compared to 'now'. Assume that 'now' is 56. You would have to
    find out if it's 12, 45 or 55 that comes 'after' 56. Then you
    would have to calculate 12 - 56 = 16. (Right?)

    Assume that you don't want to wait for seconds, but for minutes
    instead. Sleep until the minutes in the hour are either 23, 55
    or 59.

    You should have got the picture by now. (Or I have failed. :-)

EXAMPLES
    Constructor
        All examples below use this constructor.

        $th = Schedule::ByClock->new(12,8,55); # Constructor with
        three 'time' values.

    Example 1
        At 09:09:24, you call:

        $rc = $th->get_control_on_second(); # This will return at
        09:09:55.

    Example 2
        $rc = $th->set_control_list(23);

        At 09:09:24, you call:

        $rc = $th->get_control_on_second(); # This will return at
        09:10:23.

    Example 3
        $rc = $th->set_control_list(); # Note! Empty list.

        At 09:09:24, you call:

        $rc = $th->get_control_on_second(); # This will return
        immediately (with return value undef).

        At 09:09:25, you call:

        $rc = $th->get_control_on_second(12); # This will return at
        09:10:12.

        At 09:09:25, you call:

        $rc = $th->get_control_on_minute(12); # This will return at
        09:12:00. <= Note the minutes.

    Example 4
        At 09:09:55, you call:

        $rc = $th->get_control_on_second(); # This will return at
        09:10:55, one minute later.

CONSTRUCTOR
    $th = Schedule::ByClock->new([time [,time [...]]])
        Constructs a new ByClock object with an optional list of
        'times' for pre-programmed returns.

        The 'time' values can be in arbitrary order.

        Any 'time' that is not within the range 0 - 59 will be
        ignored and a warning (carp) will be written to the
        terminal.

METHODS
    @times = $th->set_control_list([time [,time [,...]]]);
        Store a list of 'times' in the ByClock object to be used in
        future calls to get_control_on_second() and/or
        get_control_on_minute(), overriding the old list (if any).

        The 'time' values can be in arbitrary order.

        Any 'time' that is not within the range 0 - 59 will be
        ignored and a warning (carp) will be written to the
        terminal. If no 'times' are given (no parameters), then the
        internally stored list of 'times' will be cleared. Returns
        the newly stored list of 'times'.

    @times = $th->get_control_list();
        Returns a list of 'times' currently stored in the ByClock
        object.

    $second = $th->get_control_on_second();
        Sleep and return control to the caller at the
        chronologically first second in the pre-programmed list of
        'times'. Returns the second that corresponds to the return.
        If the internal list of seconds is empty the call will
        immediately return undef.

    $minute = $th->get_control_on_minute();
        Sleep and return control to the caller at the
        chronologically first minute in the pre-programmed list of
        'times'. The call will return in the first second ('00') of
        the requested minute. Returns the minute that corresponds to
        the return. If the internal list of times is empty the call
        will immediately return undef.

    $second = $th->get_control_on_second([second [,second [,...]]]);
        Sleep and return control to the caller at the
        chronologically first second in the provided list of
        'seconds'. This call will ignore the internally stored list
        of times (if any). Returns the second that corresponds to
        the return.

    $minute = $th->get_control_on_minute([minute [,minute [,...]]]);
        Sleep and return control to the caller at the
        chronologically first minute in the provided list of
        'minutes'. This call will ignore the internally stored list
        of times (if any). Returns the minute that corresponds to
        the return.

    $version = $th->get_version();
        Returns the current version of Schedule::ByClock.

    Tip:
        It is slightly more efficient to initially load the list of
        'time' values, either in the constructor or in a call to
        $th->set_control_list(), since this will force
        Schedule::ByClock to validate the 'time' values only once.

        Whenever a call to $th->get_control_on_second() or $th-
        >get_control_on_minute() is done with a parameter list, all
        values in the list will have to be validated.

AUTHOR
    Gustav Schaffter <gschaffter@cyberjunkie.com>

    http://www.schaffter.com

COPYRIGHT
    Copyright (c) 1999, 2000 and 2001, Gustav Schaffter. All rights
    reserved. This program is free software; you can redistribute it
    and/or modify it under the same terms as Perl itself.

