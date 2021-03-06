

### Qt Gui stuff ###
SET(headers_ui 
	../include/${PROJECT_PREFIX}/gui/MainWindow.h
	../include/${PROJECT_PREFIX}/gui/PreferencesDialog.h
	../include/${PROJECT_PREFIX}/gui/DatabaseViewer.h
	./AboutDialog.h
	./ConsoleWidget.h
	../include/${PROJECT_PREFIX}/gui/ImageView.h
	./PdfPlot.h
	./StatsToolBox.h
	./DetailedProgressDialog.h
	./utilite/UPlot.h
	../include/${PROJECT_PREFIX}/gui/CloudViewer.h
	../include/${PROJECT_PREFIX}/gui/OdometryViewer.h
	../include/${PROJECT_PREFIX}/gui/LoopClosureViewer.h
	../include/${PROJECT_PREFIX}/gui/DataRecorder.h
	./ExportDialog.h
	./MapVisibilityWidget.h
	./GraphViewer.h
)

SET(uis
    ./ui/mainWindow.ui
    ./ui/preferencesDialog.ui
    ./ui/aboutDialog.ui
    ./ui/consoleWidget.ui
    ./ui/DatabaseViewer.ui
    ./ui/loopClosureViewer.ui
    ./ui/exportDialog.ui
)

SET(qrc 
    ./GuiLib.qrc
)

# generate rules for building source files from the resources
QT4_ADD_RESOURCES(srcs_qrc ${qrc})

#Generate .h files from the .ui files
QT4_WRAP_UI(moc_uis ${uis})

#This will generate moc_* for Qt
QT4_WRAP_CPP(moc_srcs ${headers_ui})
### Qt Gui stuff  end###




SET(SRC_FILES 
	./MainWindow.cpp
	./PreferencesDialog.cpp
	./KeypointItem.cpp
	./ImageView.cpp
	./PdfPlot.cpp
	./StatsToolBox.cpp
	./DetailedProgressDialog.cpp
	./AboutDialog.cpp
	./ConsoleWidget.cpp
	./DatabaseViewer.cpp
	./utilite/UPlot.cpp
	./CloudViewer.cpp
	./OdometryViewer.cpp
	./LoopClosureViewer.cpp
	./DataRecorder.cpp
	./ExportDialog.cpp
	./MapVisibilityWidget.cpp
	./GraphViewer.cpp
	${moc_srcs} 
	${moc_uis} 
	${srcs_qrc}
)

SET(INCLUDE_DIRS
	${PROJECT_SOURCE_DIR}/corelib/include
	${PROJECT_SOURCE_DIR}/utilite/include
	${CMAKE_CURRENT_SOURCE_DIR}/../include
	${CMAKE_CURRENT_SOURCE_DIR}
	${OpenCV_INCLUDE_DIRS}
	${CMAKE_CURRENT_BINARY_DIR} # for qt ui generated in binary dir
	${PCL_INCLUDE_DIRS}
)

INCLUDE(${QT_USE_FILE})

INCLUDE(${VTK_USE_FILE})

SET(LIBRARIES
    ${QT_LIBRARIES} 
	${OpenCV_LIBS} 
	${PCL_LIBRARIES}
	QVTK 
	vtkHybrid
)

#include files
INCLUDE_DIRECTORIES(${INCLUDE_DIRS})

add_definitions(${PCL_DEFINITIONS})

# create a library from the source files
ADD_LIBRARY(rtabmap_gui ${SRC_FILES})
# Linking with Qt libraries
TARGET_LINK_LIBRARIES(rtabmap_gui rtabmap_core rtabmap_utilite ${LIBRARIES})

INSTALL(TARGETS rtabmap_gui
        RUNTIME DESTINATION "${INSTALL_BIN_DIR}" COMPONENT runtime
        LIBRARY DESTINATION "${INSTALL_LIB_DIR}" COMPONENT devel
        ARCHIVE DESTINATION "${INSTALL_LIB_DIR}" COMPONENT devel)
        
install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/../include/ DESTINATION "${INSTALL_INCLUDE_DIR}" COMPONENT devel FILES_MATCHING PATTERN "*.h" PATTERN ".svn" EXCLUDE)
