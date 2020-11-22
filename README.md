# NAME

Alien::libavro_c - libavro_c, with alien

# VERSION

Version 0.02

# SYNOPSIS

    use Alien::libavro_c;

    Alien::libavro_c->libs;
    Alien::libavro_c->libs_static;
    Alien::libavro_c->cflags;

    # Or a more realistic example; in your makefile:
    use Config;
    my $zk_libs        = Alien::libavro_c->libs;
    my $zk_libs_static = Alien::libavro_c->libs_static;

    my $lddflags = $Config{lddlflags} // '';
    $lddlflags  .= ' ';

    my $libext = $Config{lib_ext};
    if ( $libs_static =~ /libavro_c\.\Q$libext\E/ ) {
        # We can statically link against libavro_c.
        # To link statically, we need to pass arguments to `ld`, not to the C
        # compiler, and we need to drop the dynamic version from the arguments:
        $_ =~ s/-lavro_c\b// for $zk_libs, $zk_libs_static;
        $lddlflags .= ' ' . $zk_libs_static;
    }

    WriteMakefile(
        INC       => Alien::libavro_c->cflags,
        LIBS      => [ $zk_libs ],
        LDDLFLAGS => [ $lddlflags ],
        ...
    );

# DESCRIPTION

`Alien::libavro_c` is an `Alien` interface to `libavro_c`.

Turns out that `libavro_c` is pretty hard to get hold of!  It's source is
shipped as part of ZooKeeper, so in some systems you need to install ZooKeeper
\-- and all the Java stack it needs -- just to get the C shared library.

In other systems, you can get it from package managers just fine, but it doesn't
have a `pkg-config` meta file, and so finding it ends up requiring writing and
running C.

And in some systems (Alpine, Arch-Linux) there's just no way to get the library.

This module tries very hard to get a working `libavro_c`:  It checks pkg-config,
it checks by compiling code, and if there's nothing in the system that we can use,
it builds version 3.5.6 from source.

# NOTES

The built-from-source version comes with some caveats!

First, we use version 3.5.6 because that's the last official release that
can be built from source without needing Java; see [https://issues.apache.org/jira/browse/ZOOKEEPER-3530](https://issues.apache.org/jira/browse/ZOOKEEPER-3530)
for details.

Second, we patch a bug fixed upstream in the 3.6.x releases that lead
to segfault on connection errors.

Third, we patch its `CMakeLists.txt` with some missing make targets;
see [https://issues.apache.org/jira/browse/ZOOKEEPER-4012](https://issues.apache.org/jira/browse/ZOOKEEPER-4012).

Fourth, we patch its `CMakeLists.txt` to change how it generates the
statically-linked `libavro_c.a`; see [https://issues.apache.org/jira/browse/ZOOKEEPER-4014](https://issues.apache.org/jira/browse/ZOOKEEPER-4014)

Fifth, we patch its build process to generate a `pkg-config` meta
file; see [https://issues.apache.org/jira/browse/ZOOKEEPER-4013](https://issues.apache.org/jira/browse/ZOOKEEPER-4013)

Hopefully as the above get addressed, there will be less and less
cases where this module ends up building the library itself!

# LICENSE AND COPYRIGHT

This software is Copyright (c) 2020 by B Fraser.

This is free software, licensed under:

    The Artistic License 2.0 (GPL Compatible)
