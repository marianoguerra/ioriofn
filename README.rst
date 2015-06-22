ioriofn
=====

An efene application to test ioriodb

Build
-----

::

    rebar3 compile

Use
---

Start a shell::

    rebar3 shell

and then::

    application:ensure_all_started(ioriofn).
    random:seed().
    Rnd = ioriofn_rnd:new().

    SenderCount = 20.
    StreamsCount = SenderCount div 3.

    Username = <<"mariano">>.
    Password = <<"secret">>.
    Host = "localhost".
    Port = 8080.
    Bucket = list_to_binary(io_lib:format("_user_~s", [Username])).
    Stream = <<"cc1">>.
    Streams = ioriofn_tester:prefixed_streams("cc", StreamsCount).

    Opts = #{username => Username, password => Password,
             host => Host, port => Port, bucket => Bucket}.

    {Senders, Rnd1} = ioriofn_tester:new_senders(Rnd, Opts, Streams, SenderCount).

To Stop the madness::

    lists:map(fun ioriofn_sender:stop/1, Senders).

TODO
----

* define license and create LICENSE file

License
-------

TODO
