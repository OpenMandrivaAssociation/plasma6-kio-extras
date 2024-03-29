# vim:set softtabstop=3 shiftwidth=3 tabstop=3 expandtab:

add_definitions(-DTRANSLATION_DOMAIN=\"kio6_activities\")

remove_definitions(-DQT_NO_CAST_FROM_ASCII)

include_directories(
   ${CMAKE_CURRENT_SOURCE_DIR}
   ${CMAKE_CURRENT_BINARY_DIR}
)

add_subdirectory(fileitemplugin)

# ########## next target ###############
add_library(kio_activities MODULE)
set_target_properties(kio_activities PROPERTIES
   OUTPUT_NAME "activities"
)

target_sources(kio_activities PRIVATE
   KioActivities.cpp
   KioActivitiesApi.cpp
   utils/qsqlquery_iterator.cpp
   common/database/Database.cpp
   common/database/schema/ResourcesDatabaseSchema.cpp
)

target_link_libraries(
   kio_activities
   Qt::Sql
   KF6::KIOCore
   KF6::DBusAddons
   Plasma::Activities
   KF6::I18n
   Qt::Network
)

# static library to use in an autotest and create a plugin module
add_library(kio_activities_a STATIC)

target_sources(kio_activities_a PRIVATE
   KioActivities.cpp
   KioActivitiesApi.cpp
   utils/qsqlquery_iterator.cpp
   common/database/Database.cpp
   common/database/schema/ResourcesDatabaseSchema.cpp
)

target_link_libraries(
   kio_activities_a
   Qt::Sql
   KF6::KIOCore
   KF6::DBusAddons
   Plasma::Activities
   KF6::I18n
   Qt::Network
)

if (BUILD_TESTING)
   add_subdirectory(autotests)
endif()

# install target (typical /usr/lib64/qtX/plugins/kf5/kio)
install(
   TARGETS kio_activities
   DESTINATION ${KDE_INSTALL_PLUGINDIR}/kf6/kio
)
