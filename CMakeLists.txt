cmake_minimum_required(VERSION 2.8)

include(CryptoNoteWallet.cmake)

project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(CRYPTONOTE_LIB cryptonote)

include_directories(${CMAKE_BINARY_DIR}
    src
    bytcoins/external
    bytcoins/include
    bytcoins/src)

find_package(Qt5Gui REQUIRED)
find_package(Qt5Widgets REQUIRED)

set(Boost_USE_STATIC_LIBS ON)
if(WIN32)
  set(Boost_USE_STATIC_RUNTIME OFF)
else(WIN32)
  set(Boost_USE_STATIC_RUNTIME ON)
endif(WIN32)

find_package(Boost 1.55 REQUIRED COMPONENTS date_time filesystem program_options regex serialization system thread chrono)
if ((${Boost_MAJOR_VERSION} EQUAL 1) AND (${Boost_MINOR_VERSION} EQUAL 54))
	message(SEND_ERROR "Boost version 1.54 is unsupported, more details are available here http://goo.gl/RrCFmA")
endif ()

include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})

set(VERSION "")
configure_file("bytcoins/src/version.h.in" "version.h")
configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(CRYPTONOTE_SOURCES
  bytcoins/external/miniupnpc/connecthostport.c
  bytcoins/external/miniupnpc/igd_desc_parse.c
  bytcoins/external/miniupnpc/minisoap.c
  bytcoins/external/miniupnpc/miniupnpc.c
  bytcoins/external/miniupnpc/miniwget.c
  bytcoins/external/miniupnpc/minixml.c
  bytcoins/external/miniupnpc/portlistingparse.c
  bytcoins/external/miniupnpc/receivedata.c
  bytcoins/external/miniupnpc/upnpcommands.c
  bytcoins/external/miniupnpc/upnpreplyparse.c
  bytcoins/src/Common/Base58.cpp
  bytcoins/src/Common/CommandLine.cpp
  bytcoins/src/Common/Util.cpp
  bytcoins/src/Common/StringTools.cpp
  bytcoins/src/Common/JsonValue.cpp
  bytcoins/src/Common/ConsoleTools.cpp
  bytcoins/src/Common/MemoryInputStream.cpp
  bytcoins/src/Common/PathTools.cpp
  bytcoins/src/Common/StdInputStream.cpp
  bytcoins/src/Common/StdOutputStream.cpp
  bytcoins/src/Common/StreamTools.cpp
  bytcoins/src/Common/StringOutputStream.cpp
  bytcoins/src/Common/StringView.cpp
  bytcoins/src/Common/VectorOutputStream.cpp
  bytcoins/src/crypto/blake256.c
  bytcoins/src/crypto/chacha8.c
  bytcoins/src/crypto/crypto-ops-data.c
  bytcoins/src/crypto/crypto-ops.c
  bytcoins/src/crypto/crypto.cpp
  bytcoins/src/crypto/groestl.c
  bytcoins/src/crypto/hash-extra-blake.c
  bytcoins/src/crypto/hash-extra-groestl.c
  bytcoins/src/crypto/hash-extra-jh.c
  bytcoins/src/crypto/hash-extra-skein.c
  bytcoins/src/crypto/hash.c
  bytcoins/src/crypto/jh.c
  bytcoins/src/crypto/keccak.c
  bytcoins/src/crypto/oaes_lib.c
  bytcoins/src/crypto/random.c
  bytcoins/src/crypto/skein.c
  bytcoins/src/crypto/slow-hash.c
  bytcoins/src/crypto/slow-hash.cpp
  bytcoins/src/crypto/tree-hash.c
  bytcoins/src/CryptoNoteCore/BlockchainIndices.cpp
  bytcoins/src/CryptoNoteCore/BlockchainMessages.cpp
  bytcoins/src/CryptoNoteCore/BlockIndex.cpp
  bytcoins/src/CryptoNoteCore/CoreConfig.cpp
  bytcoins/src/CryptoNoteCore/CryptoNoteBasic.cpp
  bytcoins/src/CryptoNoteCore/CryptoNoteTools.cpp
  bytcoins/src/CryptoNoteCore/Currency.cpp
  bytcoins/src/CryptoNoteCore/MinerConfig.cpp
  bytcoins/src/CryptoNoteCore/Transaction.cpp
  bytcoins/src/CryptoNoteCore/Account.cpp
  bytcoins/src/CryptoNoteCore/Blockchain.cpp
  bytcoins/src/CryptoNoteCore/Checkpoints.cpp
  bytcoins/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  bytcoins/src/CryptoNoteCore/Core.cpp
  bytcoins/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  bytcoins/src/CryptoNoteCore/CryptoNoteSerialization.cpp
  bytcoins/src/CryptoNoteCore/Difficulty.cpp
  bytcoins/src/CryptoNoteCore/IBlock.cpp
  bytcoins/src/CryptoNoteCore/Miner.cpp
  bytcoins/src/CryptoNoteCore/TransactionExtra.cpp
  bytcoins/src/CryptoNoteCore/TransactionPool.cpp
  bytcoins/src/CryptoNoteCore/TransactionPrefixImpl.cpp
  bytcoins/src/CryptoNoteCore/TransactionUtils.cpp
  bytcoins/src/InProcessNode/InProcessNode.cpp
  bytcoins/src/InProcessNode/InProcessNodeErrors.cpp
  bytcoins/src/NodeRpcProxy/NodeErrors.cpp
  bytcoins/src/NodeRpcProxy/NodeRpcProxy.cpp
  bytcoins/src/P2p/NetNodeConfig.cpp
  bytcoins/src/Serialization/BinaryInputStreamSerializer.cpp
  bytcoins/src/Serialization/BinaryOutputStreamSerializer.cpp
  bytcoins/src/Serialization/JsonInputValueSerializer.cpp
  bytcoins/src/Serialization/JsonOutputStreamSerializer.cpp
  bytcoins/src/Serialization/KVBinaryInputStreamSerializer.cpp
  bytcoins/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  bytcoins/src/Serialization/SerializationOverloads.cpp
  bytcoins/src/Transfers/BlockchainSynchronizer.cpp
  bytcoins/src/Transfers/SynchronizationState.cpp
  bytcoins/src/Transfers/TransfersConsumer.cpp
  bytcoins/src/Transfers/TransfersContainer.cpp
  bytcoins/src/Transfers/TransfersSubscription.cpp
  bytcoins/src/Transfers/TransfersSynchronizer.cpp
  bytcoins/src/Wallet/LegacyKeysImporter.cpp
  bytcoins/src/Wallet/WalletAsyncContextCounter.cpp
  bytcoins/src/Wallet/WalletErrors.cpp
  bytcoins/src/WalletLegacy/KeysStorage.cpp
  bytcoins/src/WalletLegacy/WalletLegacy.cpp
  bytcoins/src/WalletLegacy/WalletHelper.cpp
  bytcoins/src/WalletLegacy/WalletLegacySerializer.cpp
  bytcoins/src/WalletLegacy/WalletLegacySerialization.cpp
  bytcoins/src/WalletLegacy/WalletTransactionSender.cpp
  bytcoins/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  bytcoins/src/WalletLegacy/WalletUserTransactionsCache.cpp
  bytcoins/src/System/ContextGroup.cpp
  bytcoins/src/System/Event.cpp
  bytcoins/src/System/EventLock.cpp
  bytcoins/src/System/InterruptedException.cpp
  bytcoins/src/System/Ipv4Address.cpp
  bytcoins/src/System/TcpStream.cpp
  bytcoins/src/HTTP/HttpRequest.cpp
  bytcoins/src/HTTP/HttpParser.cpp
  bytcoins/src/HTTP/HttpParserErrorCodes.cpp
  bytcoins/src/HTTP/HttpResponse.cpp
  bytcoins/src/Rpc/HttpClient.cpp
  bytcoins/src/Rpc/JsonRpc.cpp
  bytcoins/src/P2p/NetNode.cpp
  bytcoins/src/P2p/LevinProtocol.cpp
  bytcoins/src/P2p/NetNodeConfig.cpp
  bytcoins/src/P2p/PeerListManager.cpp
  bytcoins/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  bytcoins/src/Logging/ILogger.cpp
  bytcoins/src/Logging/LoggerGroup.cpp
  bytcoins/src/Logging/CommonLogger.cpp
  bytcoins/src/Logging/LoggerManager.cpp
  bytcoins/src/Logging/FileLogger.cpp
  bytcoins/src/Logging/StreamLogger.cpp
  bytcoins/src/Logging/ConsoleLogger.cpp
  bytcoins/src/Logging/LoggerMessage.cpp
  bytcoins/src/Logging/LoggerRef.cpp
  bytcoins/src/BlockchainExplorer/BlockchainExplorer.cpp
  bytcoins/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  bytcoins/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
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
  include_directories(bytcoins/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/cryptonotewallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} bytcoins/external/miniupnpc/minissdpc.c)
  if (APPLE)
    enable_language(ASM)
    file(GLOB_RECURSE OBJC_SOURCES src/*.mm)
    set(SOURCES ${SOURCES} ${OBJC_SOURCES})
    set(PLATFORM_DIR OSX)
    set(MACOSX_BUNDLE_INFO_STRING "Bytcoins GUI wallet")
    set(MACOSX_BUNDLE_LONG_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_NAME BytcoinsWallet)
    set(MACOSX_BUNDLE_SHORT_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_VERSION "$${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")

    find_package(Qt5PrintSupport REQUIRED)

    include_directories(/usr/include/malloc)

    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11 -stdlib=libc++")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c11 -maes -D_DARWIN_C_SOURCE")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -framework Cocoa -framework OpenGL -framework CoreFoundation -framework Carbon -framework IOKit -L/usr/lib")

    set(MACOSX_BUNDLE_ICON_FILE cryptonote.icns)
    set(APPLICATION_ICON src/images/cryptonote.icns)
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

include_directories(bytcoins/src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES bytcoins/src/Platform/${PLATFORM_DIR}/System/*)
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
