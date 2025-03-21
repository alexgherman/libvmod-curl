AC_PREREQ(2.64)
AC_COPYRIGHT([Copyright (c) 2011-2016 Varnish Software])
AC_INIT([libvmod-curl], [1.0.4])
AC_CONFIG_MACRO_DIR([m4])
AC_CONFIG_SRCDIR(src/vmod_curl.vcc)
AM_CONFIG_HEADER(config.h)

AC_CANONICAL_SYSTEM
AC_LANG(C)

AM_INIT_AUTOMAKE([foreign color-tests parallel-tests])

AC_GNU_SOURCE
AC_PROG_CC
AC_PROG_CC_STDC
if test "x$ac_cv_prog_cc_c99" = xno; then
	AC_MSG_ERROR([Could not find a C99 compatible compiler])
fi
AC_PROG_CPP

AC_PROG_INSTALL
AC_PROG_LIBTOOL
AC_PROG_MAKE_SET

# Check for rst utilities
AC_CHECK_PROGS(RST2MAN, [rst2man rst2man.py], "no")
if test "x$RST2MAN" = "xno"; then
	AC_MSG_WARN([rst2man not found - not building man pages])
fi
AM_CONDITIONAL(HAVE_RST2MAN, [test "x$RST2MAN" != "xno"])

# Check for pkg-config
PKG_PROG_PKG_CONFIG
PKG_CHECK_MODULES([CURL], [libcurl])

# Checks for header files.
AC_HEADER_STDC
AC_CHECK_HEADERS([sys/stdlib.h])

# Check for python
AC_CHECK_PROGS(PYTHON, [python3 python3.1 python3.2 python2.7 python2.6 python2.5 python2 python], "no")
if test "x$PYTHON" = "xno"; then
	AC_MSG_ERROR([Python is needed to build this vmod, please install python.])
fi

# VARNISH_PREREQ([7.3.0])
VARNISH_VMODS([curl])

AX_CURLOPT_CHECK([CURLOPT_TIMEOUT_MS])
AX_CURLOPT_CHECK([CURLOPT_CONNECTTIMEOUT_MS])
AX_CURLOPT_CHECK([CURLOPT_UNIX_SOCKET_PATH])

VMOD_TESTS="$(cd $srcdir/src && echo tests/*.vtc)"
AC_SUBST(VMOD_TESTS)

AC_CONFIG_FILES([
	Makefile
	src/Makefile
])
AC_OUTPUT

AS_ECHO("
	==== $PACKAGE_STRING ====

	varnish:	$VARNISH_VERSION
	prefix:		$prefix
	vmoddir:	$vmoddir
	vcldir:		$vcldir
	pkgvcldir:	$pkgvcldir

	compiler:	$CC
	cflags:		$CFLAGS
	ldflags:	$LDFLAGS
")
