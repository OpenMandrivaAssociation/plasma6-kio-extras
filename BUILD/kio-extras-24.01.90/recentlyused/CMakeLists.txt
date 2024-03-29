add_definitions(-DTRANSLATION_DOMAIN=\"kio6_recentlyused\")

add_library(recentlyused MODULE)
set_target_properties(recentlyused PROPERTIES
    OUTPUT_NAME "recentlyused"
    LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin/kf6/kio"
)
target_sources(recentlyused PRIVATE
    recentlyused.cpp
)
# recentlyused.h has examples in the comment secions which match /*,
# so triggering the preprocessor to warn about nested comments.
# Using a pragma to ignore that diagnostic does not work for preprocessor warnings
# so for now simply that warning is completly disabled
if (CMAKE_CXX_COMPILER_ID STREQUAL "GNU" OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    set(_flag "-Wno-comment")
    check_cxx_compiler_flag(${_flag} _flag_present)
    if(_flag_present)
        target_compile_options(recentlyused PRIVATE "${_flag}")
    endif()
endif()

ecm_qt_declare_logging_category(recentlyused
    HEADER recentlyused-logsettings.h
    IDENTIFIER KIO_RECENTLYUSED_LOG
    CATEGORY_NAME kf.kio.workers.recentlyused
    OLD_CATEGORY_NAMES log_kio_recentlyused kf.kio.slaves.recentlyused
    DESCRIPTION "KIO recentlyused"
    EXPORT KIO_EXTRAS
)

target_link_libraries(recentlyused
    KF6::KIOCore
    KF6::I18n
    Plasma::Activities
    Plasma::ActivitiesStats)

install(TARGETS recentlyused DESTINATION ${KDE_INSTALL_PLUGINDIR}/kf6/kio)

# forgetfileitemaction plugin
kcoreaddons_add_plugin(forgetfileitemaction
    SOURCES forgetfileitemaction.cpp
    INSTALL_NAMESPACE "kf6/kfileitemaction")

target_link_libraries(forgetfileitemaction
    KF6::I18n
    KF6::KIOWidgets)
