# Uncomment this if SQL is installed.
# ADD_DEFINITIONS(-DHAVE_SQL_STORAGE)

INCLUDE_DIRECTORIES (
    ${ODBC_INCLUDE_DIRS}
)

ADD_LIBRARY (persist-sql
    odbcxx
    ODBCAtomStorage
    SQLPersistSCM
)

ADD_DEPENDENCIES(persist-sql opencog_atom_types)

TARGET_LINK_LIBRARIES(persist-sql
    sql-support
    atomspace
    ${ODBC_LIBRARIES}
)

IF (HAVE_GUILE)
    TARGET_LINK_LIBRARIES(persist-sql smob)
ENDIF (HAVE_GUILE)


ADD_EXECUTABLE(sniff
    sniff.cc
)

TARGET_LINK_LIBRARIES(sniff
    sql-support
    persist-sql
    atomspace
    execution
    clearbox
    smob
    atomcore
    atomutils
    ${ODBC_LIBRARIES}
)

INSTALL (TARGETS persist-sql
    DESTINATION "lib${LIB_DIR_SUFFIX}/opencog"
)

INSTALL (FILES
    ODBCAtomStorage.h
    odbcxx.h
    SQLPersistSCM.h
    DESTINATION "include/opencog/persist/sql/odbc"
)
