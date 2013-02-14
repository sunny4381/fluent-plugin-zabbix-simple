= fluent-plugin-zabbix-simple

== Component

=== ZabbixSimpleOutput

fluent-plugin-zabbix-simple converts fluent JSON key into zabbix key, and sends zabbix key and its value to Zabbix Server.

* You can define multiple key map, no size limitation.

* You can use regex and you can replace the fluent JSON key using regex.

== Install

execute command `gem install`:

    $ sudo gem install fluent-plugin-zabbix-simple

td-agent has its own Ruby ecosystem.
If you have installed td-agent, You would use `gem` command included with td-agent:

    $ sudo /usr/lib64/fluent/ruby/bin/gem isntall fluent-plugin-zabbix-simple

== Configuration

=== Zabbix Server Configuration

In advance, You shoud define zabbix items like this:

    Key: httpd.status[2xx]
    Type: Zabbix trapper
    Type of information: Numeric(unsigned)

    Key: httpd.status[3xx]
    Type: Zabbix trapper
    Type of information: Numeric(unsigned)

Tips:

* You must set `Type` to `Zabbix trapper`.

* You must choose a appropriate `Type of information`.

=== Plugin Configuration and Test

To test plugin, create file:

    <source>
      type forward
    </source>
    <match httpd.access.status_count>
      type zabbix_simple
      zabbix_server 192.168.0.1
      map_key1 httpd.access_(...)_count httpd.status[\1]
    </match>

Save above file to `fluentd.conf`, execute `fluentd`:

    $ fluentd -c ./fluent.conf -vv

If you have installed td-agent, execute a command like this:

    $ /usr/sbin/td-agent -c ./fluent.conf -vv

Open another termina, send a test message to fluentd server.

    $ echo '{"httpd.access_2xx_count":321}' | fluent-cat httpd.access.status_count

after a few seconds, confirm that the 321 has been recorded in the Zabbix Server.

== Too Many Keys

By default, key_map is scanned up to 20.
you must specify `key_size` if you want to use key_map more than 20.

    <match httpd.access.status_count>
      type zabbix_simple
      zabbix_server 192.168.0.1
      key_size 25
      map_key1  pattern1  replace1
      map_key2  pattern2  replace2
      map_key3  pattern3  replace3
      map_key4  pattern4  replace4
      map_key5  pattern5  replace5
      map_key6  pattern6  replace6
      map_key7  pattern7  replace7
      map_key8  pattern8  replace8
      map_key9  pattern9  replace9
      map_key10 pattern10 replace10
      map_key11 pattern11 replace11
      map_key12 pattern12 replace12
      map_key13 pattern13 replace13
      map_key14 pattern14 replace14
      map_key15 pattern15 replace15
      map_key16 pattern16 replace16
      map_key17 pattern17 replace17
      map_key18 pattern18 replace18
      map_key19 pattern19 replace19
      map_key20 pattern20 replace20
      map_key21 pattern21 replace21
      map_key22 pattern22 replace22
      map_key23 pattern23 replace23
      map_key24 pattern24 replace24
      map_key25 pattern25 replace25
    </match>

== Specification of `map_key`

* you can use `map_key0`.

* lookup from `map_key0`, use first matched map_key, ignore the remaining map_keys.

* you must not padding 0 like `map_key01`.

== TODO

- patches welcome!

== Copyright

Copyright:: Copyright (c) 2013 NAKANO Hideo
License::   Apache License, Version 2.0