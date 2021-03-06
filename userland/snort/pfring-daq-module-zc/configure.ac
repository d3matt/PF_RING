#												-*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ(2.62)
AC_INIT([pfring-zc-daq-module], [0.1], [deri@ntop.org])
AM_INIT_AUTOMAKE([daq], [0.1])
AC_CONFIG_MACRO_DIR([m4])
AC_CONFIG_SRCDIR([daq_pfring_zc.c])
AC_CONFIG_HEADERS([config.h])

# Checks for programs.
AC_PROG_CC
AC_PROG_LIBTOOL

# Enable visibility if we can
AC_ENABLE_VISIBILITY()
# Special compiler flags for ICC, etc.
AC_SF_COMPILER_SETUP()

# Checks for the DAQ API headers and the SFBPF headers/library.
AC_CHECK_DAQ_API
AC_CHECK_SFBPF

# Checks for PF_RING
AC_ARG_WITH( libpfring_includes,
	     [  --with-libpfring-includes=DIR      libpfring include directory],
	     [with_pfring_includes="$withval"], [with_pfring_includes="no"])

AC_ARG_WITH( pfring_kernel_includes,
	     [  --with-pfring-kernel-includes=DIR  pfring kernel include directory],
	     [with_pfring_kernel_includes="$withval"], [with_pfring_kernel_includes="no"])

AC_ARG_WITH( libpfring_libraries,
	     [  --with-libpfring-libraries=DIR     libpfring library directory],
	     [with_pfring_libraries="$withval"], [with_pfring_libraries="no"])

if test "x$with_pfring_includes" != "xno"; then
	CPPFLAGS="${CPPFLAGS} -I${with_pfring_includes}"
else 
	if test -d "${HOME}/PF_RING/"; then
		CPPFLAGS="${CPPFLAGS} -I${HOME}/PF_RING/userland/lib"
	fi
fi

if test "x$with_pfring_kernel_includes" != "xno"; then
	CPPFLAGS="${CPPFLAGS} -I${with_pfring_kernel_includes}"
else 
	if test -d "${HOME}/PF_RING/"; then
		CPPFLAGS="${CPPFLAGS} -I${HOME}/PF_RING/kernel"
	fi
fi

if test "x$with_pfring_libraries" != "xno"; then
	LDFLAGS="${LDFLAGS} -L${with_pfring_libraries}"
else 
	if test -d "${HOME}/PF_RING/"; then
		LDFLAGS="${LDFLAGS} -L${HOME}/PF_RING/userland/lib"
	fi
fi

# PF_RING headers/library checks
AC_CHECK_HEADER([pfring.h],  [], [AC_MSG_ERROR([Could not find pfring.h! ])])

if test -d "${HOME}/PF_RING/"; then
	LDFLAGS="${LDFLAGS} ${HOME}/PF_RING/userland/lib/libpfring.a ${HOME}/PF_RING/userland/libpcap/libpcap.a -lpthread `${HOME}/PF_RING/userland/lib/pfring_config --libs`"
else
	AC_CHECK_LIB( [pcap],
              [pfring_open],
	      [LDFLAGS="${LDFLAGS} -lpcap"],
	      [LDFLAGS="${LDFLAGS} -lpfring"],
	      [] )
fi



# Checks for nBroker
AC_ARG_WITH(libnbroker_includes,
	    [--with-libnbroker-includes=DIR libnbroker include directory],
	    [with_nbroker_includes="$withval"], [with_nbroker_includes="no"])

AC_ARG_WITH(libnbroker_libraries,
	    [--with-libnbroker-libraries=DIR libnbroker library directory],
	    [with_nbroker_libraries="$withval"], [with_nbroker_libraries="no"])

if test "x$with_nbroker_includes" != "xno"; then
	CPPFLAGS="${CPPFLAGS} -I${with_nbroker_includes}"
fi

if test "x$with_nbroker_libraries" != "xno"; then
	LDFLAGS="${LDFLAGS} -L${with_nbroker_libraries}"
fi

# nBroker headers/library checks
AC_CHECK_HEADER([nbroker_api.h],  [], [])

AC_CHECK_LIB([nbroker],
             [nbroker_init],
	     [LDFLAGS="${LDFLAGS} -lnbroker"; CPPFLAGS="${CPPFLAGS} -DHAVE_NBROKER"],
	     [],
	     [])

# Checks for header files.
AC_CHECK_HEADERS([netinet/in.h stdint.h stdlib.h string.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_TYPE_SIZE_T
AC_TYPE_UINT32_T
AC_TYPE_UINT8_T

# Checks for library functions.
#AC_FUNC_MALLOC
AC_CHECK_FUNCS([memset strchr strdup strerror strtol])

# Substitutions

# Output
AC_CONFIG_FILES([Makefile])
AC_OUTPUT
