ioriofn
=======

An efene application to test ioriodb

Build
-----

::

    rebar3 compile

Use
---

Start the erlang shell (no efene shell yet)::

    rebar3 shell

and then::

    application:ensure_all_started(ioriofn).
    random:seed().
    Rnd = ioriofn_rnd:new().
    ioriofn_metric:create_metrics().

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
    {Readers, Rnd2} = ioriofn_tester:new_readers(Rnd1, Opts, Streams, SenderCount).

To start metrics sender::

    MetricsOpts = Opts#{stream => <<"ioriofn_metrics">>,
        timeouts => [60000], messages => [fun ioriofn_metric:get_all_str_keys/0]}.
    {ok, MetricSender} = ioriofn_sender:start_link(MetricsOpts).

To get metrics::

    ioriofn_metric:get_all().

To Stop the madness::

    lists:map(fun ioriofn_sender:stop/1, Senders).

Basic usage::

    application:ensure_all_started(ioriofn).
    Host = "localhost".
    Port = 8080.
    Username = <<"mariano">>.
    Password = <<"secret">>.

    {ok, C1} = ioriofn:new(Host, Port).
    {ok, C2} = ioriofn:login(C1, Username, Password).

    ioriofn:ping(C2).
    ioriofn:list_buckets(C2).
    ioriofn:list_streams(C2, <<"_user_mariano">>).
    ioriofn:list_streams(C2, <<"public">>).

TODO
----

* define license and create LICENSE file

License
-------

TODO
