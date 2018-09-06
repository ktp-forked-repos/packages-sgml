cmake_minimum_required(VERSION 2.8.12)
project(swipl-sgml)

include("../cmake/PrologPackage.cmake")
include(CheckTypeSize)

AC_CHECK_HEADERS(malloc.h unistd.h sys/time.h fcntl.h floatingpoint.h)
AC_CHECK_FUNCS(snprintf strerror strtoll)

check_type_size("long" SIZEOF_LONG)

configure_file(config.h.cmake config.h)

swipl_plugin(
    sgml2pl
    C_SOURCES parser.c util.c charmap.c catalog.c model.c xmlns.c utf8.c
	      xml_unicode.c quote.c sgml2pl.c xsd.c
    C_LIBS ${LIBEDIT_LIBRARIES}
    PL_LIBS sgml.pl xsdp_types.pl iso_639.pl sgml_write.pl xpath.pl
	    pwp.pl c14n2.pl)

test_libs(sgml sgml_write xsd)
