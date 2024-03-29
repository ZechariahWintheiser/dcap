m4_define([dcap_major_version], [2])
m4_define([dcap_minor_version], [47])
m4_define([dcap_micro_version], [14])
m4_define([dcap_patch_version], [])

m4_define([libdcap_version],
      [dcap_major_version.dcap_minor_version.dcap_micro_version])

AC_INIT([dcap], [libdcap_version], [support@dcache.org])
AC_CONFIG_AUX_DIR(config)

AC_DEFINE_UNQUOTED(DCAP_VER_PROTOCOL, dcap_major_version, [DCAP protocol version])
AC_DEFINE_UNQUOTED(DCAP_VER_MAJOR, dcap_minor_version, [LibDCAP version major])
AC_DEFINE_UNQUOTED(DCAP_VER_MINOR, dcap_micro_version, [LibDCAP version minor])
AC_DEFINE_UNQUOTED(DCAP_VER_PATCH, "dcap_patch_version", [LibDCAP patch level])

AM_INIT_AUTOMAKE

LIBRARY_VERSION="dcap_major_version:dcap_minor_version:1"
AC_SUBST(LIBRARY_VERSION)

AC_CANONICAL_HOST

AC_PROG_CC
m4_ifdef([AM_PROG_AR], [AM_PROG_AR])

LT_INIT([shared,disable-static])

AM_CONFIG_HEADER(src/debug_level.h)

#
# Wall
#
AC_CACHE_CHECK([for maximum warnings compiler flag],
  ac_cv_cwflag,
[case "${CC-cc}" in
  *gcc*) ac_cv_cwflag="-Wall -pedantic -pipe";;
  *)
  ;;
esac])
AC_SUBST([CWFLAG], [$ac_cv_cwflag])


AC_SEARCH_LIBS([gethostbyname], [nsl])
AC_SEARCH_LIBS([bind], [socket])

AC_CHECK_FUNCS([alarm])
AC_CHECK_FUNCS([atexit])
AC_CHECK_FUNCS([bzero])
AC_CHECK_FUNCS([dup2])
AC_CHECK_FUNCS([ftruncate])
AC_CHECK_FUNCS([gethostbyaddr])
AC_CHECK_FUNCS([gethostbyname])
AC_CHECK_FUNCS([gethostname])
AC_CHECK_FUNCS([memmove])
AC_CHECK_FUNCS([memset])
AC_CHECK_FUNCS([mkdir])
AC_CHECK_FUNCS([realpath])
AC_CHECK_FUNCS([rmdir])
AC_CHECK_FUNCS([select])
AC_CHECK_FUNCS([socket])
AC_CHECK_FUNCS([strcasecmp])
AC_CHECK_FUNCS([strchr])
AC_CHECK_FUNCS([strdup])
AC_CHECK_FUNCS([strerror])
AC_CHECK_FUNCS([strndup])
AC_CHECK_FUNCS([strrchr])
AC_CHECK_FUNCS([strstr])
AC_CHECK_FUNCS([htonll])
AC_CHECK_FUNCS([ntohll])
AC_CHECK_FUNCS([acl])
AC_CHECK_FUNCS([facl])
AC_CHECK_FUNCS([_xstat])
AC_CHECK_FUNCS([_lxstat])
AC_CHECK_FUNCS([_fxstat])
AC_CHECK_FUNCS([lseek64])
AC_CHECK_FUNCS([pread64])
AC_CHECK_FUNCS([pwrite64])
AC_CHECK_FUNCS([ftello64])
AC_CHECK_FUNCS([fseeko64])
AC_CHECK_HEADERS([arpa/inet.h])
AC_CHECK_HEADERS([fcntl.h])
AC_CHECK_HEADERS([limits.h])
AC_CHECK_HEADERS([netdb.h])
AC_CHECK_HEADERS([netinet/in.h])
AC_CHECK_HEADERS([sys/param.h])
AC_CHECK_HEADERS([sys/socket.h])
AC_CHECK_HEADERS([sys/time.h])
AC_CHECK_HEADERS([sys/vfs.h])
AC_CHECK_HEADERS([termios.h])
AC_CHECK_HEADERS([stropts.h])
AC_CHECK_MEMBERS([struct stat.st_blksize])
AC_CHECK_MEMBERS([struct stat.st_rdev])
AC_CHECK_MEMBER([struct dirent.dirent])
AC_CHECK_MEMBER([struct dirent.dirent64])
AC_CHECK_MEMBERS([FILE._fileno])
AC_CHECK_MEMBERS([FILE._magic])
AC_CHECK_MEMBERS([FILE._file])
AC_CHECK_MEMBERS([FILE._flag])
AC_C_CONST
AC_FUNC_CHOWN
AC_FUNC_CLOSEDIR_VOID
AC_FUNC_FORK
AC_FUNC_FSEEKO
AC_FUNC_LSTAT
AC_FUNC_LSTAT_FOLLOWS_SLASHED_SYMLINK
AC_FUNC_MALLOC
AC_FUNC_MEMCMP
AC_FUNC_REALLOC
AC_FUNC_SELECT_ARGTYPES
AC_FUNC_STAT
AC_FUNC_VPRINTF
AC_HEADER_DIRENT
AC_HEADER_TIME
AC_PROG_RANLIB
AC_STRUCT_ST_BLOCKS
AC_TYPE_MODE_T
AC_TYPE_OFF_T
AC_TYPE_SIGNAL
AC_TYPE_SIZE_T
AC_TYPE_UID_T
AC_C_BIGENDIAN
AC_CHECK_TYPES([long long])


dnl Store the compile flags prior to testing the presence.

_cppflags="${CPPFLAGS}"
_ldflags="${LDFLAGS}"
_ldlibpath="${LD_LIBRARY_PATH}"


dnl === zlib stuff ===

AC_CHECK_LIB(z, adler32, [ZLIB="-lz"],[
AC_MSG_ERROR([zlib was not found])
])
AC_SUBST(ZLIB)

dnl === Manage variables ===

CPPFLAGS=${_cppflags}
LDFLAGS=${_ldflags}
dnl Should not be needed but becasue globus libraries
dnl linking is somewhat problematic this is needed.
export LD_LIBRARY_PATH=${_ldlibpath}



dnl === openssl stuff ===

nossl=0
AC_CHECK_HEADERS([openssl/ssl.h], [], [nossl=1])
AC_CHECK_HEADERS([openssl/err.h], [], [nossl=1])
if test "${nossl}" != "0" ; then
AC_MSG_RESULT([Disabling ssl plugin as header files not found.])
else
AC_CHECK_LIB(ssl,SSL_write,[
SSL_LIBS="-lssl"
],[
nossl=1
])
AC_CHECK_LIB(crypto,ERR_print_errors_fp,[
SSL_LIBS="${SSL_LIBS} -lcrypto"
],[
nossl=1
])
fi
AC_SUBST(SSL_LIBS)

CPPFLAGS=${_cppflags}
LDFLAGS=${_ldflags}
export LD_LIBRARY_PATH=${_ldlibpath}

dnl === Thinks that should be in macros already ===
AC_CACHE_CHECK([for off64_t],
    ac_cv_have_off64_t,
    AC_TRY_COMPILE([
        #define _LARGEFILE64_SOURCE
        #include <sys/types.h>],
        [off64_t testoffset],
        ac_cv_have_off64_t=yes,
        ac_cv_have_off64_t=no))
if test x"$ac_cv_have_off64_t" = "xyes" ; then
    AC_DEFINE(HAVE_OFF64_T, 1, [Whether off64_t type is available])
fi


dnl === Globus stuff ===
noglobus=0
EPEL_LIBS_NOT=0


AC_ARG_WITH(globus-lib,
[ --with-globus-lib[[=DIR]] use libglobus libdir],[
if test "X${withval}" != "X" ; then
GLOBUS_LIB_DIR=$withval
LDFLAGS="${LDFLAGS} -L$withval"
if test "X${LD_LIBRARY_PATH}" = "X" ; then
export LD_LIBRARY_PATH="${withval}"
else
export LD_LIBRARY_PATH="${LD_LIBRARY_PATH}:${withval}"
fi
fi
])
AC_ARG_WITH(globus-include,
[ --with-globus-include[[=DIR]] use libglobus includedir],[
if test "X${withval}" != "X" ; then
GLOBUS_INCLUDE_DIR=$withval
CPPFLAGS="${CPPFLAGS} -I$withval"
fi
])

AC_CHECK_HEADERS( [globus_config.h],[], [noglobus=1])
AC_CHECK_HEADERS([gssapi.h],[],[noglobus=1])

if test "${noglobus}" != "0" ; then
AC_MSG_RESULT([Disabling Globus plugins as header files not found.])
else

dnl Set up the flags for found libraries


EPEL_LIBS_NOT=0
GCC64PTHR_LIBS_NOT=0
GCC64DBGPTHR_LIBS_NOT=0
GCC32PTHR_LIBS_NOT=0
GCC32DBGPTHR_LIBS_NOT=0

AC_CHECK_LIB(globus_gssapi_gsi,gss_wrap, [
GLOBUS_EPEL_LIBS="-lglobus_gssapi_gsi ${GLOBUS_EPEL_LIBS}"
],[
EPEL_LIBS_NOT=1
])


dnl == 64pthreads library checking version ==

AC_CHECK_LIB(globus_gssapi_gsi_gcc64pthr,gss_wrap, [
GLOBUS_GCC64PTHR_LIBS="-lglobus_gssapi_gsi_gcc64pthr ${GLOBUS_GCC64PTHR_LIBS}"
],[
GCC64PTHR_LIBS_NOT=1
])


dnl == 64dbgpthr library checking version ==

AC_CHECK_LIB(globus_gssapi_gsi_gcc64dbgpthr,gss_wrap, [
GLOBUS_GCC64DBGPTHR_LIBS="-lglobus_gssapi_gsi_gcc64dbgpthr ${GLOBUS_GCC64DBGPTHR_LIBS}"
],[
GCC64DBGPTHR_LIBS_NOT=1
])


dnl == 32pthreads library checking version ==

AC_CHECK_LIB(globus_gssapi_gsi_gcc32pthr,gss_wrap, [
GLOBUS_GCC32PTHR_LIBS="-lglobus_gssapi_gsi_gcc32pthr ${GLOBUS_GCC32PTHR_LIBS}"
],[
GCC32PTHR_LIBS_NOT=1
])


dnl == 32dbgpthr library checking version ==

AC_CHECK_LIB(globus_gssapi_gsi_gcc32dbgpthr,gss_wrap, [
GLOBUS_GCC32DBGPTHR_LIBS="-lglobus_gssapi_gsi_gcc32dbgpthr ${GLOBUS_GCC32DBGPTHR_LIBS}"
],[
GCC32DBGPTHR_LIBS_NOT=1
])


dnl set the version of globus fully detected and the library linking.
want_globus=no
if test "0" = "${GCC32DBGPTHR_LIBS_NOT}" ; then
GLOBUS_LIBS=${GLOBUS_GCC32DBGPTHR_LIBS}
PKGDEPVDT=1
want_globus=yes
fi

if test "0" = "${GCC32PTHR_LIBS_NOT}" ; then
GLOBUS_LIBS=${GLOBUS_GCC32PTHR_LIBS}
PKGDEPVDT=1
want_globus=yes
fi

if test "0" = "${GCC64DBGPTHR_LIBS_NOT}" ; then
GLOBUS_LIBS=${GLOBUS_GCC64DBGPTHR_LIBS}
PKGDEPVDT=1
want_globus=yes
fi

if test "0" = "${GCC64PTHR_LIBS_NOT}" ; then
GLOBUS_LIBS=${GLOBUS_GCC64PTHR_LIBS}
PKGDEPVDT=1
want_globus=yes
fi

if test "0" = "${EPEL_LIBS_NOT}" ; then
GLOBUS_LIBS=${GLOBUS_EPEL_LIBS}
PKGDEPEPEL=1
want_globus=yes
fi



dnl == End checking as globus header files exist ==
fi

dnl Substitute the globus library linking variable

AC_SUBST(GLOBUS_LIBS)

dnl restore the compile flags to what they where before checking for globus.

CPPFLAGS=${_cppflags}
LDFLAGS=${_ldflags}
export LD_LIBRARY_PATH=${_ldlibpath}



dnl == MIT Kerberos ==

AC_ARG_WITH(krb5-gssapi-include,
[ --with-krb5-gssapi-include[[=DIR]] use krb5 gssapi includedir],[
if test "X${withval}" != "X" ; then
KRB5_GSSAPI_INCLUDE_DIR=$withval
fi
],[
KRB5_GSSAPI_INCLUDE_DIR=/usr/include/gssapi
])
AC_SUBST(KRB5_GSSAPI_INCLUDE_DIR)

nokrb=0
CPPFLAGS="${CPPFLAGS} -I${KRB5_GSSAPI_INCLUDE_DIR}"
AC_CHECK_HEADERS([gssapi_krb5.h],[],[nokrb=1])
AC_CHECK_HEADERS([gssapi_generic.h],[],[nokrb=1])


if test "${nokrb}" != "0" ; then
AC_MSG_RESULT([Disabling KRB plugins as header files not found.])
else

AC_CHECK_LIB(gssapi_krb5,gss_import_name, [
KRB_LIBS="-lgssapi_krb5 ${KRB_LIBS}"
],[
nokrb=1
])

fi
AC_SUBST(KRB_LIBS)

nocunit=0
AC_CHECK_HEADERS([CUnit/Basic.h],[],[nocunit=1])
AC_CHECK_LIB(cunit,CU_initialize_registry, [
CUNIT_LIBS="-lcunit"
],[
nocunit=1
])
AC_SUBST(CUNIT_LIBS)

CPPFLAGS=${_cppflags}
LDFLAGS=${_ldflags}
export LD_LIBRARY_PATH=${_ldlibpath}

dnl ====================
dnl Check and set Prefix
dnl ====================

if test "${prefix}" = NONE ; then
    dcap_prefix=/usr/local
else
    dcap_prefix=${prefix}
fi

dnl =============
dnl Check RPM dir
dnl =============

if test X = "X${RPMD}" ; then
    RPMD=`pwd`
fi
AC_MSG_CHECKING([for directory where to create rpms])
AC_ARG_WITH(rpm-dir,[  --with-rpm-dir=<dir> Default is \$RPMD/],
            rpm_dir="$withval", rpm_dir="$RPMD")
if test -d "$rpm_dir" ; then
    AC_MSG_RESULT([found $rpm_dir])
    RPM_DIR=$rpm_dir
else
    AC_MSG_RESULT([no such directory $rpm_dir])
    AC_MSG_RESULT([the directory $rpm_dir will be created for you if possible])
    RPM_DIR=$rpm_dir
fi
AC_SUBST(RPM_DIR)

dnl =============
dnl Check Doc dir
dnl =============
dnl Note: as of autoconf 2.6 this is included by default

if test X = "X${DOCD}" ; then
    DOCD=${dcap_prefix}/share/doc/${PACKAGE}-${VERSION}
fi
AC_MSG_CHECKING([for directory where to install docs])
AC_ARG_WITH(doc-dir,[  --with-doc-dir=<dir> Default is \$DOCD/],
            doc_dir="$withval", doc_dir="$DOCD")
if test -d "$doc_dir" ; then
    AC_MSG_RESULT([found $doc_dir])
    DOC_DIR=$doc_dir
else
    AC_MSG_RESULT([no such directory $doc_dir])
    AC_MSG_RESULT([the directory $doc_dir will be created for you if possible])
    DOC_DIR=$doc_dir
fi

AC_SUBST(DOC_DIR)

AC_SUBST(ac_aux_dir)

dnl Some debugging dcap #defines

AC_DEFINE_UNQUOTED(DC_NO_OUTPUT,0,[dcap debug level for no output])
AC_DEFINE_UNQUOTED(DC_ERROR,1,[dcap debug level for error])
AC_DEFINE_UNQUOTED(DC_INFO,2,[dcap debug level for info])
AC_DEFINE_UNQUOTED(DC_TIME,4,[dcap debug level to include time stamps])
AC_DEFINE_UNQUOTED(DC_TRACE,8,[dcap debug level to include stacks])
AC_DEFINE_UNQUOTED(DC_CALLS,16,[dcap debug level to include calls])
AC_DEFINE_UNQUOTED(DC_IO,32,[dcap debug level to include IO logging])
AC_DEFINE_UNQUOTED(DC_THREADS,64,[dcap debug level to include thread tracing])


dnl Set default preload options

OPEN_SYM="open"
CLOSE_SYM="close"
READ_SYM="read"
WRITE_SYM="write"
LSEEK_SYM="lseek"
LSEEK64_SYM="lseek64"
PREAD_SYM="pread"
PREAD64_SYM="pread64"
PWRITE_SYM="pwrite"
PWRITE64_SYM="pwrite64"
STAT_SYM="stat"
STAT64_SYM="stat64"
FSTAT64_SYM="fstat64"
LSTAT_SYM="lstat"
LSTAT64_SYM="lstat64"
FSTAT_SYM="fstat"
FSYNC_SYM="fsync"
DUP_SYM="dup"
OPENDIR_SYM="opendir"
CLOSEDIR_SYM="closedir"
READDIR_SYM="readdir"
READDIR64_SYM="readdir64"
TELLDIR_SYM="telldir"
SEEKDIR_SYM="seekdir"
UNLINK_SYM="unlink"
RMDIR_SYM="rmdir"
MKDIR_SYM="mkdir"
CHMOD_SYM="chmod"
ACCESS_SYM="access"
RENAME_SYM="rename"
CHOWN_SYM="chown"
WRITEV_SYM="writev"
READV_SYM="readv"
ACL_SYM="acl"
FACL_SYM="facl"
dnl Override defaults By OS
case "${host_os}" in
linux-gnu)
AC_MSG_RESULT([Setting the Linux preload methods])
STAT_SYM="__xstat"
STAT64_SYM="__xstat64"
FSTAT64_SYM="__fxstat64"
LSTAT_SYM="__lxstat"
LSTAT64_SYM="__lxstat64"
FSTAT_SYM="__fxstat"
;;
darwin*)
AC_MSG_RESULT([Setting Darwin preload methods])
;;
sgi*)
AC_MSG_RESULT([Setting SGI preload methods])
OPENDIR_SYM="_opendir"
CLOSEDIR_SYM="_closedir"
READDIR_SYM="_readdir"
READDIR64_SYM="_readdir64"
TELLDIR_SYM="_telldir"
SEEKDIR_SYM="_seekdir"
;;
solaris*)
AC_MSG_RESULT([Setting SUN preload methods])
OPENDIR_SYM="_opendir"
CLOSEDIR_SYM="_closedir"
READDIR_SYM="_readdir"
READDIR64_SYM="_readdir64"
TELLDIR_SYM="_telldir"
SEEKDIR_SYM="_seekdir"
;;
*)
AC_MSG_RESULT([Operating system not coded for explicitly so disabling preload])
CPPFLAGS="${CPPFLAGS} -DDCAP_NO_PREAD"
;;
esac

AC_DEFINE_UNQUOTED(OPEN_SYM,"$OPEN_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(CLOSE_SYM,"$CLOSE_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(READ_SYM,"$READ_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(WRITE_SYM,"$WRITE_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(LSEEK_SYM,"$LSEEK_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(LSEEK64_SYM,"$LSEEK64_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(PREAD_SYM,"$PREAD_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(PREAD64_SYM,"$PREAD64_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(PWRITE_SYM,"$PWRITE_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(PWRITE64_SYM,"$PWRITE64_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(STAT_SYM,"$STAT_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(STAT64_SYM,"$STAT64_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(FSTAT64_SYM,"$FSTAT64_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(LSTAT_SYM,"$LSTAT_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(LSTAT64_SYM,"$LSTAT64_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(FSTAT_SYM,"$FSTAT_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(FSYNC_SYM,"$FSYNC_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(DUP_SYM,"$DUP_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(OPENDIR_SYM,"$OPENDIR_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(CLOSEDIR_SYM,"$CLOSEDIR_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(READDIR_SYM,"$READDIR_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(READDIR64_SYM,"$READDIR64_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(TELLDIR_SYM,"$TELLDIR_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(SEEKDIR_SYM,"$SEEKDIR_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(UNLINK_SYM,"$UNLINK_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(RMDIR_SYM,"$RMDIR_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(MKDIR_SYM,"$MKDIR_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(CHMOD_SYM,"$CHMOD_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(ACCESS_SYM,"$ACCESS_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(RENAME_SYM,"$RENAME_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(CHOWN_SYM,"$CHOWN_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(WRITEV_SYM,"$WRITEV_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(READV_SYM,"$READV_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(ACL_SYM,"$ACL_SYM",[native function for native or local file system access.])
AC_DEFINE_UNQUOTED(FACL_SYM,"$FACL_SYM",[native function for native or local file system access.])

dnl We dont test for the dependencies for the telnet plugin
dnl as there appear to be no major ones.

notelnet="0"

dnl Do the conditional compilation flags in rest of autotools.

AM_CONDITIONAL([WANT_LIBGSITUNNEL], [test "${want_globus}" = "yes"])
AM_CONDITIONAL([WANT_LIBGSSTUNNEL], [test "${nokrb}" = "0"])
AM_CONDITIONAL([WANT_LIBTELNETTUNNEL], [test "${notelnet}" = "0"])
AM_CONDITIONAL([WANT_LIBSSLTUNNEL], [test "${nossl}" = "0"])
AM_CONDITIONAL([PKG_DEP_VDT], [test "${PKGDEPVDT}" = "1"])
AM_CONDITIONAL([PKG_DEP_EPEL], [test "${PKGDEPEPEL}" = "1"])



AC_SUBST(GSSAPI_TARGETS)

dnl Passing configure variables to spec file for rpm to build.

AC_SUBST(GLOBUS_INCLUDE_DIR)
AC_SUBST(GLOBUS_LIB_DIR)

TUNNELLIBDIR="$libdir"
AC_ARG_WITH(tunneldir,
[ --with-tunneldir[[=DIR]] where to install dcap plugins],[
if test "X${withval}" != "X" ; then
TUNNELLIBDIR="$withval"
fi
])

save_prefix="$prefix"
save_exec_prefix="$exec_prefix"
if test "X$prefix" = "XNONE" ; then
   prefix="$ac_default_prefix"
fi
if test "X$exec_prefix" = "XNONE" ; then
   exec_prefix="$prefix"
fi
eval tunnellibdir="$TUNNELLIBDIR"
eval tunnellibdir="$tunnellibdir"
prefix="$save_prefix"
exec_prefix="$save_exec_prefix"

AC_DEFINE_UNQUOTED(TUNNELLIBDIR, "$tunnellibdir", [where to install dcap plugins])
AC_SUBST(TUNNELLIBDIR)

AC_OUTPUT(Makefile \
  src/Makefile \
  plugins/Makefile \
  plugins/gssapi/Makefile \
  plugins/ssl/Makefile \
  plugins/telnet/Makefile \
  tests/Makefile \
  dcap.spec )
