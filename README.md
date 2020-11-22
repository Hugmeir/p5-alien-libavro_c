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

# LICENSE AND COPYRIGHT

This software is Copyright (c) 2020 by B Fraser.

This is free software, licensed under:

    The Artistic License 2.0 (GPL Compatible)
# NAME

Alien::libavro\_c - libavro\_c, with alien

# VERSION

Version 0.01

# SYNOPSIS

    use Alien::libavro_c;

    Alien::libavro_c->libs;
    Alien::libavro_c->libs_static;
    Alien::libavro_c->cflags;

# DESCRIPTION

`Alien::libavro_c` is an `Alien` interface to `libavro-c`.

# AUTHOR

B Fraser, `<fraserbn at gmail.com>`

# BUGS

Please report any bugs or feature requests to `bug-alien-libavro_c at rt.cpan.org`, or through
the web interface at [https://rt.cpan.org/NoAuth/ReportBug.html?Queue=Alien-libavro\_c](https://rt.cpan.org/NoAuth/ReportBug.html?Queue=Alien-libavro_c).  I will be notified, and then you'll
automatically be notified of progress on your bug as I make changes.

# LICENSE AND COPYRIGHT

This software is Copyright (c) 2020 by B Fraser.

This is free software, licensed under:

    The Artistic License 2.0 (GPL Compatible)
