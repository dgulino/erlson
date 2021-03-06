Erlson - Erlang Simple Object Notation
======================================

Erlson is a dynamic name-value dictionary data type for Erlang.

Erlson dictionaries come with a convenient syntax and can be directly converted
to and from JSON.

Examples:

```erlang
    % creating an empty dictionary
    X = #{},

    % associating field 'foo' with 1 and 'bar' with "abc"
    D = #{foo = 1, bar = "abc"},

    % accessing dictionary element
    1 = D.foo,

    % adding nested dictionaries to dictionary D
    D1 = D#{baz = #{fum = #{i = 0}}},

    % accessing elements of the nested dictionary
    0 = D1.baz.fum.i,

    % modifying elements of the nested dictionary
    D2 = D1#{baz.fum.i = 100, baz.fum.j = "new nested value"}.

    ...

    % converting Erlson dictionary to JSON iolist()
    erlson:to_json(D2).

    % creating Erlson dictionary from JSON iolist()
    D = erlson:from_json(Json).
```

General properties
------------------

* Erlson dictionaries contain zero or more Name->Value associations
(fields), where each Name is `atom()` or `binary()` and Value can be of `any()`
type.

* Name->Value associations are unique. If a new association is created for the
existing Name, the old value will be replaced by the new value.

* Erlson dictionaries can be nested.


Runtime properties
------------------

* Only fields with `atom()` names can be accessed using the Erlson
dictionary syntax.

* Unlike Erlang records, Erlson dictionaries can be dynamically constructed
without any static type declarations.

* At runtime, Erlson dictionaries are represented as a list of {Name, Value}
tuples ordered by Name. This way, each Erlson dictionary is a valid `proplist`
and `orddict` in terms of the correspondent stdlib modules.

* Erlson dictionaries (dictionary syntax) can't be used as patterns and in
guard expressions.

* Erlson dicts can be used in both compiled modules and Erlang interactive
shell.


Properties related to JSON
--------------------------

* Each valid JSON object can be converted to correspondent Erlson
dictionary.

* An Erlson dictionary can be converted to JSON if it follows JSON data
model.

* JSON->Erlson->JSON conversion produces an equivalent JSON object
(fields will be reordered).


Usage instructions
------------------

For compiled modules that use Erlson syntax, the Erlson library header must be
included:

    -include_lib("erlson/include/erlson.hrl").


When rebar is used as a build tool, it should be configured to use
"erlson_rebar_plugin". In order to do that, add the following line to the
project's "rebar.config" file:

    {rebar_plugins, [erlson_rebar_plugin]}.


In order to use Erlson syntax from Erlang shell, run the following command (e.g.
include it in `.erlang` file):
    
    erlson:init().


Dependencies
------------

Erlson relies on `mochijson2` library for JSON encoding and decoding. It comes
as a part of [Mochiweb](https://github.com/mochi/mochiweb). Erlson doesn't
automatically include it, but if you wish to do it with a rebar-enabled project,
add it as dependency in your `rebar.config`. For example:

    {deps,
        [
            % we need Mochiweb for mochijson2
            {mochiweb, "", {git, "https://github.com/mochi/mochiweb.git", {branch, "master"}}}
        ]}.
