#
# Copyright (C) 2015 Red Hat
#
# This file is part of tlog.
#
# Tlog is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 2 of the License, or
# (at your option) any later version.
#
# Tlog is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with tlog; if not, write to the Free Software
# Foundation, Inc., 51 Franklin St, Fifth Floor, Boston, MA  02110-1301  USA
#
# Process this file with autoconf to produce a configure script.

AC_PREREQ(2.61)
AC_INIT([tlog], [4])
AM_INIT_AUTOMAKE([1.9 -Wall foreign])
AM_MAINTAINER_MODE
AC_CONFIG_HEADER([config.h])
AC_CONFIG_MACRO_DIR([m4/autotools])
AC_GNU_SOURCE
ABS_SRCDIR=`cd ${srcdir}; pwd`
CPPFLAGS="-I${ABS_SRCDIR} -I${ABS_SRCDIR}/include -DNDEBUG $CPPFLAGS"

# Check for programs.
AC_PROG_CC
AC_PROG_CC_STDC
AC_PROG_INSTALL
AC_PROG_SED
AC_PROG_MKDIR_P
m4_ifdef([AM_PROG_AR], [AM_PROG_AR])
AC_PATH_PROG([M4], [m4])
AS_IF([test x$M4 == x], [AC_MSG_ERROR([m4 not found])])
LT_INIT

# Check for pthreads
# Needs to be before adding -Werror, otherwise fails
AX_PTHREAD

# Check for features
AC_ARG_ENABLE(
    debug,
    AS_HELP_STRING([--enable-debug], [enable debugging features]),
    [], [enable_debug="no"])

# Output features to preprocessor and compiler
if test "$enable_debug" = "yes"; then
    CPPFLAGS="$CPPFLAGS -UNDEBUG"
    CFLAGS="$CFLAGS -Wall -Wextra -Werror -g -O0"
fi

# Check for libraries
JSON_LIBS=
PKG_CHECK_MODULES(JSON, json-c,
                  [AC_DEFINE([HAVE_JSON],[1],[Have JSON library])])
AM_CONDITIONAL(HAVE_JSON, test -n "$JSON_LIBS")

LIBCURL_CHECK_CONFIG([yes], [7.15.4], ,
                     AC_MSG_ERROR([libcurl not found]))

# Output
AC_CONFIG_FILES([Makefile
                 m4/Makefile
                 m4/tlog/Makefile
                 include/Makefile
                 include/tlog/Makefile
                 lib/Makefile
                 src/Makefile
                 doc/Makefile
                 man/Makefile])
AC_OUTPUT
