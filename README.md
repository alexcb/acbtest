## Semi-lightweight `test` and `grep` wrappers

Wrappers for `test` and `grep` which will output the args when they fail.

The wrappers use bash (sorry `sh` is not supported due to quote escapping for `{$var@Q}`.

### `acbtest`

Wraps the `test` command, and outputs the command to stderr if it fails, e.g.

    $ ./acbtest "abc" = "acb"
    test 'abc' '=' 'acb' failed

### `acbdiff`

Wraps the `diff` command, and outputs the command to stderr if it fails, e.g.

    $ ./acbdiff <(echo hello) <(echo Hello)
    1c1
    < hello
    ---
    > Hello
    diff '/dev/fd/63' '/dev/fd/62' failed

### `acbgrep`

Wraps the `grep` command, and outputs the command to stderr if it fails; additionally if reading from a pipe,
it will also display the contents of the pipe.

    $ ./acbgrep o some-file 
    hello
    world

    $ cat some-file | ./acbgrep o
    hello
    world

    $ ./acbgrep bye some-file 
    grep 'bye' 'some-file' failed

    # special case where stdin is printed on failure
    $ cat some-file | ./acbgrep bye
    hello
    world
    grep 'bye' failed

    # disable echoing stdin with
    $ cat some-file | ACBGREP_NO_ECHO=1 ./acbgrep bye
    grep 'bye' failed

### License

acbtest (and acbgrep) is free and unencumbered software released into the public domain; see [Unlicense](Unlicense) for details.
