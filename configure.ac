AC_INIT([gnome-panel], [2.32.2],
        [http://bugzilla.gnome.org/enter_bug.cgi?product=gnome-panel])
AM_CONFIG_HEADER(config.h)
AM_INIT_AUTOMAKE([1.9 no-dist-gzip dist-bzip2 tar-ustar -Wno-portability])
m4_ifdef([AM_SILENT_RULES], [AM_SILENT_RULES([yes])])

AC_CONFIG_MACRO_DIR([m4])
GNOME_COMMON_INIT

# Before making a release, the LT_VERSION string should be modified.
# The string is of the form C:R:A.
# - If interfaces have been changed or added, but binary compatibility has
#   been preserved, change to C+1:0:A+1
# - If binary compatibility has been broken (eg removed or changed interfaces)
#   change to C+1:0:0
# - If the interface is the same as the previous version, change to C:R+1:A

LIB_PANEL_APPLET_LT_VERSION=0:6:0
AC_SUBST(LIB_PANEL_APPLET_LT_VERSION)

AM_MAINTAINER_MODE
GNOME_MAINTAINER_MODE_DEFINES

IT_PROG_INTLTOOL([0.40.0])

AC_ISC_POSIX
AC_PROG_CC
AC_STDC_HEADERS
AC_LIBTOOL_WIN32_DLL
AC_PROG_LIBTOOL
#AC_PROG_SED available in 2.59b
AM_PROG_CC_C_O

AM_PATH_PYTHON
AC_PATH_PROG(GLIB_GENMARSHAL, glib-genmarshal)
AC_PATH_PROG(GCONFTOOL, gconftool-2)

GNOME_COMPILE_WARNINGS(maximum)

# Check for introspection
GOBJECT_INTROSPECTION_CHECK([0.6.7])

m4_pattern_allow([AM_V_GEN])dnl Make autoconf not complain about the rule below
PANEL_INTLTOOL_PANEL_APPLET_RULE='%.panel-applet: %.panel-applet.in $(INTLTOOL_MERGE) $(wildcard $(top_srcdir)/po/*.po) ; $(AM_V_GEN) LC_ALL=C $(INTLTOOL_MERGE) -d -u -c $(top_builddir)/po/.intltool-merge-cache $(top_srcdir)/po $< [$]@'
AC_SUBST([PANEL_INTLTOOL_PANEL_APPLET_RULE])

AC_ARG_ENABLE(deprecation_flags,
              [AC_HELP_STRING([--enable-deprecation-flags],
                              [use *_DISABLE_DEPRECATED flags @<:@default=no@:>@])],,
              [enable_deprecation_flags=no])

if test "x$enable_deprecation_flags" = "xyes"; then
   DISABLE_DEPRECATED_CFLAGS=$DISABLE_DEPRECATED
   AC_SUBST(DISABLE_DEPRECATED_CFLAGS)
fi

AC_CHECK_HEADERS(crt_externs.h)
AC_CHECK_FUNCS(_NSGetEnviron)

LIBGNOME_DESKTOP_REQUIRED=2.24.0
GDK_PIXBUF_REQUIRED=2.7.1
PANGO_REQUIRED=1.15.4
GLIB_REQUIRED=2.25.12
GTK_REQUIRED=2.19.7
LIBWNCK_REQUIRED=2.19.5
GCONF_REQUIRED=2.6.1
LIBGNOME_MENU_REQUIRED=2.27.92
LIBECAL_REQUIRED=1.6.0
LIBEDATASERVER_REQUIRED=1.2.0
LIBEDATASERVERUI_REQUIRED=1.2.0
CAIRO_REQUIRED=1.0.0
GWEATHER_REQUIRED=2.27.90
DBUS_GLIB_REQUIRED=0.80

dnl pkg-config dependency checks

PKG_CHECK_MODULES(EGG_SMCLIENT, ice sm gtk+-2.0)

PKG_CHECK_MODULES(PANEL, gdk-pixbuf-2.0 >= $GDK_PIXBUF_REQUIRED pango >= $PANGO_REQUIRED gtk+-2.0 >= $GTK_REQUIRED glib-2.0 >= $GLIB_REQUIRED gio-2.0 >= $GLIB_REQUIRED gio-unix-2.0 >= $GLIB_REQUIRED gnome-desktop-2.0 >= $LIBGNOME_DESKTOP_REQUIRED gconf-2.0 >= $GCONF_REQUIRED libgnome-menu >= $LIBGNOME_MENU_REQUIRED libcanberra-gtk dbus-glib-1 >= $DBUS_GLIB_REQUIRED)
AC_SUBST(PANEL_CFLAGS)
AC_SUBST(PANEL_LIBS)

PKG_CHECK_MODULES(LIBPANEL_APPLET, gtk+-2.0 >= $GTK_REQUIRED gconf-2.0 >= $GCONF_REQUIRED gio-2.0 >= $GLIB_REQUIRED gio-unix-2.0 >= $GLIB_REQUIRED)
AC_SUBST(LIBPANEL_APPLET_CFLAGS)
AC_SUBST(LIBPANEL_APPLET_LIBS)

PKG_CHECK_MODULES(FISH, gtk+-2.0 >= $GTK_REQUIRED cairo >= $CAIRO_REQUIRED)
AC_SUBST(FISH_CFLAGS)
AC_SUBST(FISH_LIBS)

PKG_CHECK_MODULES(NOTIFICATION_AREA, gtk+-2.0 >= $GTK_REQUIRED)
AC_SUBST(NOTIFICATION_AREA_CFLAGS)
AC_SUBST(NOTIFICATION_AREA_LIBS)

PKG_CHECK_MODULES(WNCKLET, gtk+-2.0 >= $GTK_REQUIRED libwnck-1.0 >= $LIBWNCK_REQUIRED)
AC_SUBST(WNCKLET_CFLAGS)
AC_SUBST(WNCKLET_LIBS)

AC_CHECK_HEADERS(langinfo.h)
AC_CHECK_FUNCS(nl_langinfo)

AC_ARG_ENABLE(eds,
	AC_HELP_STRING([--enable-eds],
		[Enable evolution-data-server dependencies @<:@default=auto@:>@]
	),,enable_eds=auto)
LIBECAL_REQUIREMENT=
HAVE_EDS=no
if test "x$enable_eds" = "xyes"; then
  LIBECAL_REQUIREMENT="libecal-1.2 >= $LIBECAL_REQUIRED libedataserver-1.2 >= $LIBEDATASERVER_REQUIRED libedataserverui-1.2 >= $LIBEDATASERVERUI_REQUIRED gnome-desktop-2.0 >= $LIBGNOME_DESKTOP_REQUIRED"
  CLOCK_EDS_ICONDIR=`$PKG_CONFIG --variable=datadir evolution-data-server-1.2`/pixmaps/evolution-data-server
else
  if test "x$enable_eds" != "xno"; then
    AC_MSG_CHECKING(for evolution-data-server)
    if $PKG_CONFIG --exists libecal-1.2 libedataserverui-1.2 evolution-data-server-1.2; then
       AC_MSG_RESULT(yes)
       LIBECAL_REQUIREMENT="libecal-1.2 >= $LIBECAL_REQUIRED libedataserver-1.2 >= $LIBEDATASERVER_REQUIRED libedataserverui-1.2 >= $LIBEDATASERVERUI_REQUIRED gnome-desktop-2.0 >= $LIBGNOME_DESKTOP_REQUIRED"
       CLOCK_EDS_ICONDIR=`$PKG_CONFIG --variable=datadir evolution-data-server-1.2`/pixmaps/evolution-data-server
       HAVE_EDS=yes
    else
       AC_MSG_RESULT(no)
    fi
  fi
fi
if test -n "$LIBECAL_REQUIREMENT"; then
  AC_DEFINE([HAVE_LIBECAL], 1,
            [Defined when evolution-data-server libecal-1.2 and libedataserverui-1.2 are detected])
fi
AM_CONDITIONAL(HAVE_LIBECAL, test -n "$LIBECAL_REQUIREMENT")
AC_SUBST(CLOCK_EDS_ICONDIR)

PKG_CHECK_MODULES(TZ, gio-2.0 >= $GLIB_REQUIRED)
AC_SUBST(TZ_CFLAGS)
AC_SUBST(TZ_LIBS)

PKG_CHECK_MODULES(CLOCK, pango >= $PANGO_REQUIRED gtk+-2.0 >= $GTK_REQUIRED glib-2.0 >= $GLIB_REQUIRED gio-2.0 >= $GLIB_REQUIRED $LIBECAL_REQUIREMENT librsvg-2.0 dbus-glib-1 gweather >= $GWEATHER_REQUIRED gnome-desktop-2.0 >= $LIBGNOME_DESKTOP_REQUIRED)
AC_SUBST(CLOCK_CFLAGS)
AC_SUBST(CLOCK_LIBS)

DBUS_REQUIRED=1.1.2
NETWORK_MANAGER_REQUIRED=0.6
 
AC_ARG_ENABLE(network_manager, AS_HELP_STRING([--enable-network-manager],[Enable NetworkManager support (auto)]),enable_network_manager=$enableval,enable_network_manager=auto)
if test "x$enable_network_manager" = "xno" ; then
  HAVE_NETWORK_MANAGER=no
else
  PKG_CHECK_MODULES(NETWORK_MANAGER, NetworkManager >= $NETWORK_MANAGER_REQUIRED, HAVE_NETWORK_MANAGER=yes, HAVE_NETWORK_MANAGER=no)
fi
if test "x$HAVE_NETWORK_MANAGER" = "xyes" ; then
    AC_DEFINE(HAVE_NETWORK_MANAGER, 1, [Defined if NetworkManager support is enabled])
fi

# Make it possible to compile the applets in-process
PANEL_INPROCESS_NONE=
PANEL_INPROCESS_ALL=
PANEL_INPROCESS_APPLETS=
AC_ARG_WITH(in-process-applets,
	AC_HELP_STRING([--with-in-process-applets=APPLETS],
		[comma-separated list of applets to compile in-process (possible values: none, clock, fish, notification-area, wncklet, all) @<:@default=none@:>@]),
	[for i in `echo $withval | tr , ' '`; do
		if test $i = "none"; then
			PANEL_INPROCESS_NONE=1
		else if test $i = "all"; then
			PANEL_INPROCESS_ALL=1
		else if test $i = "clock"; then
			PANEL_INPROCESS_APPLETS="$PANEL_INPROCESS_APPLETS $i"
		else if test $i = "fish"; then
			PANEL_INPROCESS_APPLETS="$PANEL_INPROCESS_APPLETS $i"
		else if test $i = "notification-area"; then
			PANEL_INPROCESS_APPLETS="$PANEL_INPROCESS_APPLETS $i"
		else if test $i = "wncklet"; then
			PANEL_INPROCESS_APPLETS="$PANEL_INPROCESS_APPLETS $i"
		else
			echo "applet $i not recognized, ignoring..."
		fi; fi; fi; fi; fi; fi
	done],
	[])

if test -n "$PANEL_INPROCESS_ALL"; then
	PANEL_INPROCESS_APPLETS="clock fish notification-area wncklet"
fi
if test -n "$PANEL_INPROCESS_NONE"; then
	PANEL_INPROCESS_APPLETS=""
fi
for i in $PANEL_INPROCESS_APPLETS; do
	if test $i = "clock"; then
		CLOCK_COMPILE_INPROCESS=1
		AC_DEFINE([CLOCK_INPROCESS], 1,
		[Defined when compiling the clock applet in-process])
	else if test $i = "fish"; then
		FISH_COMPILE_INPROCESS=1
		AC_DEFINE([FISH_INPROCESS], 1,
		[Defined when compiling the fish applet in-process])
	else if test $i = "notification-area"; then
		NOTIFICATION_AREA_COMPILE_INPROCESS=1
		AC_DEFINE([NOTIFICATION_AREA_INPROCESS], 1,
		[Defined when compiling the notification-area applet in-process])
	else if test $i = "wncklet"; then
		WNCKLET_COMPILE_INPROCESS=1
		AC_DEFINE([WNCKLET_INPROCESS], 1,
		[Defined when compiling the wncklet applet in-process])
	fi; fi; fi; fi
done
if test "x$PANEL_INPROCESS_APPLETS" = "x"; then
	PANEL_INPROCESS_APPLETS="(none)"
fi

AM_CONDITIONAL(CLOCK_INPROCESS, test -n "$CLOCK_COMPILE_INPROCESS")
AM_CONDITIONAL(FISH_INPROCESS, test -n "$FISH_COMPILE_INPROCESS")
AM_CONDITIONAL(NOTIFICATION_AREA_INPROCESS, test -n "$NOTIFICATION_AREA_COMPILE_INPROCESS")
AM_CONDITIONAL(WNCKLET_INPROCESS, test -n "$WNCKLET_COMPILE_INPROCESS")

# For the run dialog
gl_CHECK_TYPE_STRUCT_DIRENT_D_TYPE

dnl X development libraries check

PKG_CHECK_MODULES(X, x11 xau, :, [
  # pkg-config modules not found (only present since X11R7 aka Xorg); use
  # old-style detection
  AC_PATH_XTRA()
  # X not found
  if test x$no_x = xyes ; then
    AC_MSG_ERROR([X development libraries not found])
  fi

  gp_save_cflags="$CFLAGS"
  gp_save_libs="$LIBS"
  CFLAGS="$X_CFLAGS"
  LIBS="$X_PRE_LIBS $X_LIBS $X_EXTRA_LIBS"
  AC_CHECK_LIB(X11, XFree,, AC_MSG_ERROR([libX11 not found]))
  AC_CHECK_LIB(Xau, XauFileName,, AC_MSG_ERROR([libXau not found]))
  CFLAGS="$gp_save_cflags"
  LIBS="$gp_save_libs"

  X_LIBS="$X_PRE_LIBS $X_LIBS -lX11 -lXau $X_EXTRA_LIBS"
])

AC_SUBST(X_LIBS)

dnl X RANDR extension

PKG_CHECK_MODULES(XRANDR, xrandr >= 1.2.0, have_randr=yes, have_randr=no)
if test "x$have_randr" = "xyes"; then
  AC_DEFINE(HAVE_RANDR, 1, [Have the Xrandr extension library])
fi

dnl Modules dir
AC_SUBST([modulesdir],"\$(libdir)/gnome-panel/modules")

dnl Applets dir
AC_SUBST([appletsdir],"\$(datadir)/gnome-panel/applets")

dnl
dnl Temporary bonobo checks here
dnl

LIB_PANEL_APPLET_BONOBO_LT_VERSION=2:73:2
AC_SUBST(LIB_PANEL_APPLET_BONOBO_LT_VERSION)

LIBBONOBOUI_REQUIRED=2.1.1
ORBIT_REQUIRED=2.4.0

AC_ARG_ENABLE(bonobo,
	      AS_HELP_STRING([--enable-bonobo],[Enable Bonobo compatibility modules (auto)]),
	      enable_bonobo=$enableval,
	      enable_bonobo=auto)

BONOBO_CFLAGS=
BONOBO_LIBS=
LIBPANEL_APPLET_BONOBO_CFLAGS=
LIBPANEL_APPLET_BONOBO_LIBS=
ORBIT_IDL=
BONOBO_IDLDIR=
BONOBO_ACT_IDLDIR=

if test "x$enable_bonobo" = "xno" ; then
  HAVE_BONOBO=no
else
  HAVE_BONOBO=no
  PKG_CHECK_MODULES(BONOBO, ORBit-2.0 >= $ORBIT_REQUIRED libbonoboui-2.0 >= $LIBBONOBOUI_REQUIRED, HAVE_BONOBO=yes, HAVE_BONOBO=no)

  if test "x$enable_bonobo" = "xyes" -a "x$HAVE_BONOBO" = "xno" ; then
    AC_MSG_ERROR(Bonobo compatibility modules explicity enabled but not available)
  fi

  if test "x$HAVE_BONOBO" = "xyes"; then
    PKG_CHECK_MODULES(PANEL_MODULE_BONOBO, ORBit-2.0 >= $ORBIT_REQUIRED libbonoboui-2.0 >= $LIBBONOBOUI_REQUIRED)
    PKG_CHECK_MODULES(LIBPANEL_APPLET_BONOBO, ORBit-2.0 >= $ORBIT_REQUIRED gtk+-2.0 >= $GTK_REQUIRED libbonoboui-2.0 >= $LIBBONOBOUI_REQUIRED gconf-2.0 >= $GCONF_REQUIRED)

    dnl IDL directories
    ORBIT_IDL="`$PKG_CONFIG --variable=orbit_idl ORBit-2.0`"
    BONOBO_IDLDIR="`$PKG_CONFIG --variable=idldir libbonobo-2.0`"
    BONOBO_ACT_IDLDIR="`$PKG_CONFIG --variable=idldir bonobo-activation-2.0`"

    AC_PATH_PROG(GIO_QUERYMODULES, gio-querymodules, no)
  fi
fi

AM_CONDITIONAL(HAVE_BONOBO, test "x$HAVE_BONOBO" = "xyes")
AC_SUBST(BONOBO_CFLAGS)
AC_SUBST(BONOBO_LIBS)
AC_SUBST(LIBPANEL_APPLET_BONOBO_CFLAGS)
AC_SUBST(LIBPANEL_APPLET_BONOBO_LIBS)
AC_SUBST(ORBIT_IDL)
AC_SUBST(BONOBO_IDLDIR)
AC_SUBST(BONOBO_ACT_IDLDIR)

dnl  Language Support

GETTEXT_PACKAGE=gnome-panel-2.0
AC_DEFINE_UNQUOTED(GETTEXT_PACKAGE, "$GETTEXT_PACKAGE", [Define the gettext package to be used])
AC_SUBST(GETTEXT_PACKAGE)

AM_GLIB_GNU_GETTEXT

dnl gtk-doc stuff

GTK_DOC_CHECK([1.0])

dnl gnome-doc-utils stuff

GNOME_DOC_INIT

AM_GCONF_SOURCE_2

dnl platform test
case $host in
*-*-solaris*)
	ostype=solaris
	AC_DEFINE(HAVE_SOLARIS, 1, [Define to 1])
	;;
esac

dnl Don't use AC_PROG_AWK since we need the full pathname.
AC_PATH_PROGS(AWK, mawk gawk nawk awk, )
AC_PATH_PROGS(PERL, perl5 perl)

dnl define a MAINT-like variable REBUILD which is set if Perl
dnl and awk are found, so autogenerated sources can be rebuilt
AC_ARG_ENABLE(rebuilds, [  --disable-rebuilds      disable all source autogeneration rules],,enable_rebuilds=yes)
REBUILD=\#
if test "x$enable_rebuilds" = "xyes" && \
     test -n "$PERL" && \
     $PERL -e 'exit !($] >= 5.002)' > /dev/null 2>&1 && \
     test -n "$AWK" ; then
  REBUILD=
fi
AC_SUBST(REBUILD)

AC_OUTPUT([
Makefile
icons/Makefile
icons/16x16/Makefile
icons/22x22/Makefile
icons/24x24/Makefile
icons/32x32/Makefile
icons/48x48/Makefile
icons/scalable/Makefile
gnome-panel/Makefile
gnome-panel/libegg/Makefile
gnome-panel/libpanel-applet-private/Makefile
gnome-panel/libpanel-util/Makefile
gnome-panel/gnome-panel.desktop.in
libpanel-applet/libpanelapplet-3.0.pc
libpanel-applet/libpanelapplet-3.0-uninstalled.pc
libpanel-applet/Makefile
po/Makefile.in
applets/Makefile
applets/clock/Makefile
applets/clock/pixmaps/Makefile
applets/fish/Makefile
applets/notification_area/Makefile
applets/wncklet/Makefile
doc/Makefile
doc/reference/Makefile
doc/reference/panel-applet/Makefile
help/Makefile
help/clock/Makefile
help/fish/Makefile
man/Makefile
bonobo/Makefile
bonobo/idl/Makefile
bonobo/libpanel-applet/libpanelapplet-2.0.pc
bonobo/libpanel-applet/libpanelapplet-2.0-uninstalled.pc
bonobo/libpanel-applet/Makefile
bonobo/panel-module/Makefile
])

if echo foo | xgettext --from-code=UTF-8 -LC -o - - 2>/dev/null ; then
:;
else
echo "Warning: Your version of gettext does not support --from-code."
echo "         This will cause translation of some strings in non US-English"
echo "         locales to fail. For full support please upgrade to GNU"
echo "         gettext 0.12 or later."
echo
fi

dnl ---------------------------------------------------------------------------
dnl - Show summary
dnl ---------------------------------------------------------------------------

echo "
              gnome-panel $VERSION
              `echo gnome-panel $VERSION | sed "s/./=/g"`

        prefix:                        ${prefix}
        exec_prefix:                   ${exec_prefix}
        libdir:                        ${libdir}
        bindir:                        ${bindir}
        sbindir:                       ${sbindir}
        sysconfdir:                    ${sysconfdir}
        localstatedir:                 ${localstatedir}
        datadir:                       ${datadir}
        source code location:          ${srcdir}
        compiler:                      ${CC}
        cflags:                        ${CFLAGS}
        Maintainer mode:               ${USE_MAINTAINER_MODE}
        Use *_DISABLE_DEPRECATED:      ${enable_deprecation_flags}

        Bonobo applets support:        ${HAVE_BONOBO}
        Applets to build in-process:   ${PANEL_INPROCESS_APPLETS}
        Evolution-Data-Server support: ${HAVE_EDS}
        NetworkManager support:        ${HAVE_NETWORK_MANAGER}
        XRandr support:                ${have_randr}
        Build introspection support:   ${found_introspection}
        Build gtk-doc documentation:   ${enable_gtk_doc}

"
