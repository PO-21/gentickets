cmake_minimum_required(VERSION 2.8.4)

#если ругается на -std=c++11 - версия gcc != 4.8

#имя проекта
project(gentickets)

set(CMAKE_AUTOMOC ON)
set(CMAKE_INCLUDE_CURRENT_DIR ON)
#предполагаемые костыли для qtest
SET(QT_USE_QTMAIN TRUE)
SET(QT_USE_QTTEST TRUE)

#обязательно указать CMAKE_PREFIX_PATH "C:/Qt/5/5.5/mingw492_32" к qt5

#подключение необходимых модулей qt
find_package (Qt5Widgets REQUIRED )
find_package (Qt5Core REQUIRED )
find_package (Qt5Sql REQUIRED )
find_package (Qt5Gui REQUIRED )
find_package (Qt5PrintSupport REQUIRED )
find_package (Qt5Xml REQUIRED)
find_package(Qt5XmlPatterns REQUIRED)

include_directories(${Qt5Widgets_INCLUDE_DIRS})
add_definitions(${Qt5Widgets_DEFINITIONS})

include_directories(${Qt5Sql_INCLUDE_DIRS})
add_definitions(${Qt5Sql_DEFINITIONS})

include_directories(${Qt5PrintSupport_INCLUDE_DIRS})
add_definitions(${Qt5PrintSupport_DEFINITIONS})

include_directories(${Qt5Xml_INCLUDE_DIRS})
add_definitions(${Qt5Xml_DEFINITIONS})

include_directories(${Qt5XmlPatterns_INCLUDE_DIRS})
add_definitions(${Qt5XmlPatterns_DEFINITIONS})

set(QT5_LIBRARIES Qt5::Core Qt5::Gui Qt5::Widgets Qt5::PrintSupport Qt5::Sql Qt5::Xml Qt5::XmlPatterns )

#флаги компилятора
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${Qt5Widgets_EXECUTABLE_COMPILE_FLAGS} -std=c++11 -Wall -U__STRICT_ANSI__")

if (UNIX) 
  set(QT_FULL_SPEC_DIR ${QT_MKSPECS_DIR}/linux-g++)
else()
  set(QT_FULL_SPEC_DIR ${QT_MKSPECS_DIR}/default)
endif()

#путь куда собирается библиотека
SET(LIBRARY_OUTPUT_PATH ${CMAKE_SOURCE_DIR}/genticketsApp)

#путь куда компилируется ехе файл
set(EXECUTABLE_OUTPUT_PATH ${CMAKE_SOURCE_DIR}/genticketsApp) 

#инклуды проекта
  include_directories (
    ./
	./gentickets
	./genticketsApp
	${QT_FULL_SPEC_DIR}
	)

#в переменную LIBS прописываются пути к исходникам для библиотеки
SET(LIBS 
gentickets/gentickets.cpp
gentickets/gentickets.h
gentickets/gentickets_global.h
)
	
#в ЕХЕ пути для ехе файла
SET(EXE 
genticketsApp/abstractform.cpp
genticketsApp/abstractform.h
genticketsApp/depform.cpp
genticketsApp/depform.h
genticketsApp/discform.cpp
genticketsApp/discform.h
genticketsApp/main.cpp
genticketsApp/mainform.cpp
genticketsApp/mainform.h
genticketsApp/qcomboboxdelegate.cpp
genticketsApp/qcomboboxdelegate.h
genticketsApp/qlineeditdelegate.cpp
genticketsApp/qlineeditdelegate.h
genticketsApp/questionsdefiningform.cpp
genticketsApp/questionsdefiningform.h
genticketsApp/questionslistform.cpp
genticketsApp/questionslistform.h
genticketsApp/questionsloadingform.cpp
genticketsApp/questionsloadingform.h
genticketsApp/ticketslistform.cpp
genticketsApp/ticketslistform.h
)

#генерируем динамическую (SHARED) библиотеку с именем gentickets из содержимого LIBS
add_library ( gentickets SHARED ${LIBS} ${QTRLIB} )

#линкуем библиотеку с библиотеками QT
target_link_libraries ( gentickets  ${QT5_LIBRARIES})

#создаем ехе файл из содержимого ЕХЕ
#add_executable (main ${EXE})

#линкуем ехе файл с библиотеками qt и нашими (qttest библиотеки не решают проблему) 
#target_link_libraries ( main gentickets ${QT5_LIBRARIES} ${QT_QTTEST_LIBRARY})

	
