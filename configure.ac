dnl Process this file with autoconf to produce a configure script.

AC_PREREQ(2.69)

dnl Name your plug-in here
m4_define([plugin_name], [pspi])

dnl These three define the plug-in version number
m4_define([plugin_major_version], [1])
m4_define([plugin_minor_version], [0])
m4_define([plugin_micro_version], [7])

m4_define([plugin_version],
          [plugin_major_version.plugin_minor_version.plugin_micro_version])

AC_INIT([plugin_name], [plugin_version])

AC_DEFINE(PLUGIN_NAME, PACKAGE_NAME, [Plug-In name])
AC_DEFINE(PLUGIN_VERSION, PACKAGE_VERSION, [Plug-In version])

AC_DEFINE(PLUGIN_MAJOR_VERSION, plugin_major_version, [Plug-In major version])
AC_DEFINE(PLUGIN_MINOR_VERSION, plugin_minor_version, [Plug-In minor version])
AC_DEFINE(PLUGIN_MICRO_VERSION, plugin_micro_version, [Plug-In micro version])

AC_CONFIG_SRCDIR([src/main.c])
AM_CONFIG_HEADER(config.h)

AM_INIT_AUTOMAKE(no-define)

AC_CANONICAL_HOST
AC_MSG_CHECKING([for native Win32])
case "$host" in
  *-*-mingw*)
    os_win32=yes
    ;;
  *)
    os_win32=no
    ;;
esac
AC_MSG_RESULT([$os_win32])
AM_CONDITIONAL(OS_WIN32, test "$os_win32" = "yes")

AC_ARG_WITH(pssdk, [  --with-pssdk=DIRECTORY location of the Photoshop SDK],
	    [PSSDK=$with_pssdk],
	    [AC_MSG_ERROR([You must use the --with-pssdk flag])])

AC_SUBST(PSSDK)

AC_ISC_POSIX
AC_PROG_CC
AM_PROG_CC_STDC
AC_HEADER_STDC
AC_PROG_CXX

ACLOCAL="$ACLOCAL $ACLOCAL_FLAGS"

AM_MAINTAINER_MODE


dnl Use -Wall if we have gcc.
changequote(,)dnl
if test "x$GCC" = "xyes"; then
  case " $CFLAGS " in
  *[\ \	]-Wall[\ \	]*) ;;
  *) CFLAGS="$CFLAGS -Wall" ;;
  esac
fi
changequote([,])dnl


PKG_CHECK_MODULES(GIMP, gimp-2.0 gimpui-2.0 glib-2.0)

AC_SUBST(GIMP_CFLAGS)
AC_SUBST(GIMP_LIBS)

AC_SUBST(GLIB_CFLAG)
AC_SUBST(GLIB_LIBS)

GIMP_LIBDIR=`$PKG_CONFIG --variable=gimplibdir gimp-2.0`
AC_SUBST(GIMP_LIBDIR)


dnl i18n stuff

GETTEXT_PACKAGE=gimp-pspi
AC_SUBST(GETTEXT_PACKAGE)
AC_DEFINE_UNQUOTED(GETTEXT_PACKAGE, "$GETTEXT_PACKAGE",
				    [The gettext translation domain.])

dnl Add the languages which your application supports here.
ALL_LINGUAS="fi ru"

AC_PROG_INTLTOOL
AM_GLIB_GNU_GETTEXT

AC_CHECK_FUNCS(bind_textdomain_codeset)

LOCALEDIR='${datadir}/locale'
DATADIR='${datadir}/plugin_name'

AC_SUBST(LOCALEDIR)
AC_SUBST(DATADIR)


AC_MSG_CHECKING([if GTK+ is version 2.4.20 or newer])
if $PKG_CONFIG --atleast-version=2.4.20 gtk+-2.0; then
  have_gtk_2_4=yes
else
  have_gtk_2_4=no
fi
AC_MSG_RESULT($have_gtk_2_4)

if test "x$have_gtk_2_4" != "xyes"; then
  CPPFLAGS="$CPPFLAGS -DG_DISABLE_DEPRECATED -DGDK_DISABLE_DEPRECATED -DGTK_DISABLE_DEPRECATED"
fi


AC_CONFIG_FILES([
Makefile
src/Makefile
po/Makefile.in
])

AC_OUTPUT
