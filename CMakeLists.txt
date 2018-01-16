cmake_minimum_required(VERSION 2.8)

include(CryptoNoteWallet.cmake)

project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(CRYPTONOTE_LIB craycoin)

include_directories(${CMAKE_BINARY_DIR}
    src
    craycoin/external
    craycoin/include
    craycoin/src)


set (CMAKE_MODULE_PATH "C:\\Qt\\Qt5.10.0\\msvc2017_64\\")
SET(CMAKE_INCLUDE_PATH ${CMAKE_INCLUDE_PATH} "C:/boost_1_66_0")
SET(CMAKE_LIBRARY_PATH ${CMAKE_LIBRARY_PATH} "C:/boost_1_66_0")
SET(BOOST_LIBRARYDIR "C:/boost_1_66_0/stage/lib")
SET(Boost_DEBUG ON)

find_package(Qt5Core REQUIRED)

find_package(Qt5Widgets REQUIRED)


set(Boost_USE_STATIC_LIBS ON)
if(WIN32)
  set(Boost_USE_STATIC_RUNTIME ON)
else(WIN32)
  set(Boost_USE_STATIC_RUNTIME ON)
endif(WIN32)

find_package(Boost 1.55 REQUIRED COMPONENTS date_time filesystem program_options regex serialization system thread chrono)
if ((${Boost_MAJOR_VERSION} EQUAL 1) AND (${Boost_MINOR_VERSION} EQUAL 54))
	message(SEND_ERROR "Boost version 1.54 is unsupported, more details are available here http://goo.gl/RrCFmA")
endif ()

include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})

set(VERSION "1")
configure_file("craycoin/src/version.h.in" "version.h")
configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(CRYPTONOTE_SOURCES
  craycoin/external/miniupnpc/connecthostport.c
  craycoin/external/miniupnpc/igd_desc_parse.c
  craycoin/external/miniupnpc/minisoap.c
  craycoin/external/miniupnpc/miniupnpc.c
  craycoin/external/miniupnpc/miniwget.c
  craycoin/external/miniupnpc/minixml.c
  craycoin/external/miniupnpc/portlistingparse.c
  craycoin/external/miniupnpc/receivedata.c
  craycoin/external/miniupnpc/upnpcommands.c
  craycoin/external/miniupnpc/upnpreplyparse.c
  craycoin/src/Common/Base58.cpp
  craycoin/src/Common/CommandLine.cpp
  craycoin/src/Common/Util.cpp
  craycoin/src/Common/StringTools.cpp
  craycoin/src/Common/JsonValue.cpp
  craycoin/src/Common/ConsoleTools.cpp
  craycoin/src/Common/MemoryInputStream.cpp
  craycoin/src/Common/PathTools.cpp
  craycoin/src/Common/StdInputStream.cpp
  craycoin/src/Common/StdOutputStream.cpp
  craycoin/src/Common/StreamTools.cpp
  craycoin/src/Common/StringOutputStream.cpp
  craycoin/src/Common/StringView.cpp
  craycoin/src/Common/VectorOutputStream.cpp
  craycoin/src/crypto/blake256.c
  craycoin/src/crypto/chacha8.c
  craycoin/src/crypto/crypto-ops-data.c
  craycoin/src/crypto/crypto-ops.c
  craycoin/src/crypto/crypto.cpp
  craycoin/src/crypto/groestl.c
  craycoin/src/crypto/hash-extra-blake.c
  craycoin/src/crypto/hash-extra-groestl.c
  craycoin/src/crypto/hash-extra-jh.c
  craycoin/src/crypto/hash-extra-skein.c
  craycoin/src/crypto/hash.c
  craycoin/src/crypto/jh.c
  craycoin/src/crypto/keccak.c
  craycoin/src/crypto/oaes_lib.c
  craycoin/src/crypto/random.c
  craycoin/src/crypto/skein.c
  craycoin/src/crypto/slow-hash.c
  craycoin/src/crypto/slow-hash.cpp
  craycoin/src/crypto/tree-hash.c
  craycoin/src/CryptoNoteCore/BlockchainIndices.cpp
  craycoin/src/CryptoNoteCore/BlockchainMessages.cpp
  craycoin/src/CryptoNoteCore/BlockIndex.cpp
  craycoin/src/CryptoNoteCore/CoreConfig.cpp
  craycoin/src/CryptoNoteCore/CryptoNoteBasic.cpp
  craycoin/src/CryptoNoteCore/CryptoNoteTools.cpp
  craycoin/src/CryptoNoteCore/Currency.cpp
  craycoin/src/CryptoNoteCore/MinerConfig.cpp
  craycoin/src/CryptoNoteCore/Transaction.cpp
  craycoin/src/CryptoNoteCore/Account.cpp
  craycoin/src/CryptoNoteCore/Blockchain.cpp
  craycoin/src/CryptoNoteCore/Checkpoints.cpp
  craycoin/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  craycoin/src/CryptoNoteCore/Core.cpp
  craycoin/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  craycoin/src/CryptoNoteCore/CryptoNoteSerialization.cpp
  craycoin/src/CryptoNoteCore/Difficulty.cpp
  craycoin/src/CryptoNoteCore/IBlock.cpp
  craycoin/src/CryptoNoteCore/Miner.cpp
  craycoin/src/CryptoNoteCore/TransactionExtra.cpp
  craycoin/src/CryptoNoteCore/TransactionPool.cpp
  craycoin/src/CryptoNoteCore/TransactionPrefixImpl.cpp
  craycoin/src/CryptoNoteCore/TransactionUtils.cpp
  craycoin/src/InProcessNode/InProcessNode.cpp
  craycoin/src/InProcessNode/InProcessNodeErrors.cpp
  craycoin/src/NodeRpcProxy/NodeErrors.cpp
  craycoin/src/NodeRpcProxy/NodeRpcProxy.cpp
  craycoin/src/P2p/NetNodeConfig.cpp
  craycoin/src/Serialization/BinaryInputStreamSerializer.cpp
  craycoin/src/Serialization/BinaryOutputStreamSerializer.cpp
  craycoin/src/Serialization/JsonInputValueSerializer.cpp
  craycoin/src/Serialization/JsonOutputStreamSerializer.cpp
  craycoin/src/Serialization/KVBinaryInputStreamSerializer.cpp
  craycoin/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  craycoin/src/Serialization/SerializationOverloads.cpp
  craycoin/src/Transfers/BlockchainSynchronizer.cpp
  craycoin/src/Transfers/SynchronizationState.cpp
  craycoin/src/Transfers/TransfersConsumer.cpp
  craycoin/src/Transfers/TransfersContainer.cpp
  craycoin/src/Transfers/TransfersSubscription.cpp
  craycoin/src/Transfers/TransfersSynchronizer.cpp
  craycoin/src/Wallet/LegacyKeysImporter.cpp
  craycoin/src/Wallet/WalletAsyncContextCounter.cpp
  craycoin/src/Wallet/WalletErrors.cpp
  craycoin/src/WalletLegacy/KeysStorage.cpp
  craycoin/src/WalletLegacy/WalletLegacy.cpp
  craycoin/src/WalletLegacy/WalletHelper.cpp
  craycoin/src/WalletLegacy/WalletLegacySerializer.cpp
  craycoin/src/WalletLegacy/WalletLegacySerialization.cpp
  craycoin/src/WalletLegacy/WalletTransactionSender.cpp
  craycoin/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  craycoin/src/WalletLegacy/WalletUserTransactionsCache.cpp
  craycoin/src/System/ContextGroup.cpp
  craycoin/src/System/Event.cpp
  craycoin/src/System/EventLock.cpp
  craycoin/src/System/InterruptedException.cpp
  craycoin/src/System/Ipv4Address.cpp
  craycoin/src/System/TcpStream.cpp
  craycoin/src/HTTP/HttpRequest.cpp
  craycoin/src/HTTP/HttpParser.cpp
  craycoin/src/HTTP/HttpParserErrorCodes.cpp
  craycoin/src/HTTP/HttpResponse.cpp
  craycoin/src/Rpc/HttpClient.cpp
  craycoin/src/Rpc/JsonRpc.cpp
  craycoin/src/P2p/NetNode.cpp
  craycoin/src/P2p/LevinProtocol.cpp
  craycoin/src/P2p/NetNodeConfig.cpp
  craycoin/src/P2p/PeerListManager.cpp
  craycoin/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  craycoin/src/Logging/ILogger.cpp
  craycoin/src/Logging/LoggerGroup.cpp
  craycoin/src/Logging/CommonLogger.cpp
  craycoin/src/Logging/LoggerManager.cpp
  craycoin/src/Logging/FileLogger.cpp
  craycoin/src/Logging/StreamLogger.cpp
  craycoin/src/Logging/ConsoleLogger.cpp
  craycoin/src/Logging/LoggerMessage.cpp
  craycoin/src/Logging/LoggerRef.cpp
  craycoin/src/BlockchainExplorer/BlockchainExplorer.cpp
  craycoin/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  craycoin/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
)

file(GLOB_RECURSE SOURCES src/*.cpp)
file(GLOB_RECURSE HEADERS src/*.h)
file(GLOB_RECURSE FORMS src/gui/ui/*.ui)

set(QRC src/resources.qrc)

qt5_wrap_ui(UIS ${FORMS})
qt5_add_resources(RCC ${QRC})


if (WIN32)
  if (NOT MSVC)
	message(FATAL_ERROR "Only MSVC is supported on this platform")
  endif ()
  add_definitions(/D_CRT_SECURE_NO_WARNINGS /D_WIN32_WINNT=0x0600 /DSTATICLIB)
  include_directories(craycoin/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/craycoinwallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} craycoin/external/miniupnpc/minissdpc.c)
  if (APPLE)
    enable_language(ASM)
    file(GLOB_RECURSE OBJC_SOURCES src/*.mm)
    set(SOURCES ${SOURCES} ${OBJC_SOURCES})
    set(PLATFORM_DIR OSX)
    set(MACOSX_BUNDLE_INFO_STRING "Cryptonote GUI wallet")
    set(MACOSX_BUNDLE_LONG_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_NAME CryptonoteWallet)
    set(MACOSX_BUNDLE_SHORT_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_VERSION "$${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")

    find_package(Qt5PrintSupport REQUIRED)

    include_directories(/usr/include/malloc)

    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11 -stdlib=libc++")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c11 -maes -D_DARWIN_C_SOURCE")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -framework Cocoa -framework OpenGL -framework CoreFoundation -framework Carbon -framework IOKit -L/usr/lib")

    set(MACOSX_BUNDLE_ICON_FILE craycoin.icns)
    set(APPLICATION_ICON src/images/craycoin.icns)
    set_source_files_properties(${APPLICATION_ICON} PROPERTIES MACOSX_PACKAGE_LOCATION "Resources")

    set(BUILD_PLATFORM MACOSX_BUNDLE)
    set(BUILD_RESOURCES ${APPLICATION_ICON})
	
    GET_TARGET_PROPERTY(QT_LIB_DIR "${Qt5Widgets_LIBRARIES}" LOCATION)
    GET_FILENAME_COMPONENT(QT_LIB_DIR "${QT_LIB_DIR}" PATH)
  else(APPLE)
    set(PLATFORM_DIR Linux)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -maes -std=c11")
  endif (APPLE)
endif ()

include_directories(craycoin/src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES craycoin/src/Platform/${PLATFORM_DIR}/System/*)
set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} ${PLATFORM_SOURCES})

add_library(${CRYPTONOTE_LIB} STATIC ${CRYPTONOTE_SOURCES})
set_target_properties(${CRYPTONOTE_LIB} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${CRYPTONOTE_LIB} ${Boost_LIBRARIES})

add_executable(${PROJECT_NAME} ${BUILD_PLATFORM} ${BUILD_RESOURCES} ${SOURCES} ${HEADERS} ${UIS} ${RCC})
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES} ${QTMAIN} ${CRYPTONOTE_LIB})

if (APPLE)
  qt5_use_modules(${PROJECT_NAME} PrintSupport)
elseif (UNIX)
  target_link_libraries(${PROJECT_NAME} -lpthread)
elseif (WIN32)
  target_link_libraries(${PROJECT_NAME} Imm32 Iphlpapi Winmm)
endif (APPLE)

qt5_use_modules(${PROJECT_NAME} Widgets Gui)
