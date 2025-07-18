# This source file is part of the Swift open source project
#
# Copyright (c) 2021 Apple Inc. and the Swift project authors
# Licensed under Apache License v2.0 with Runtime Library Exception
#
# See http://swift.org/LICENSE.txt for license information
# See http://swift.org/CONTRIBUTORS.txt for Swift project authors

add_library(PackageCollections
  API.swift
  Model/CVE.swift
  Model/Collection.swift
  Model/License.swift
  Model/PackageList.swift
  Model/PackageTypes.swift
  Model/Search.swift
  Model/TargetListResult.swift
  PackageCollections+CertificatePolicy.swift
  PackageCollections+Configuration.swift
  PackageCollections+Storage.swift
  PackageCollections+Validation.swift
  PackageCollections.swift
  PackageIndex+Configuration.swift
  PackageIndex.swift
  PackageIndexAndCollections.swift
  Providers/GitHubPackageMetadataProvider.swift
  Providers/JSONPackageCollectionProvider.swift
  Providers/PackageCollectionProvider.swift
  Providers/PackageMetadataProvider.swift
  Storage/FilePackageCollectionsSourcesStorage.swift
  Storage/PackageCollectionsSourcesStorage.swift
  Storage/PackageCollectionsStorage.swift
  Storage/SQLitePackageCollectionsStorage.swift
  Storage/Trie.swift
  Utility.swift)
# NOTE(compnerd) workaround for CMake not setting up include flags yet
set_target_properties(PackageCollections PROPERTIES
  INTERFACE_INCLUDE_DIRECTORIES ${CMAKE_Swift_MODULE_DIRECTORY})
target_link_libraries(PackageCollections PUBLIC
  Basics
  PackageCollectionsModel
  PackageCollectionsSigning
  PackageModel
  SourceControl
  TSCBasic
  TSCUtility)
if(NOT APPLE)
  if(Foundation_FOUND)
    target_link_directories(PackageCollections PRIVATE
      $<TARGET_LINKER_FILE_DIR:Fooundation>)
  endif()
  if(dispatch_FOUND)
    target_link_directories(PackageCollections PRIVATE
      $<TARGET_LINKER_FILE_DIR:Dispatch>)
  endif()
endif()

install(TARGETS PackageCollections
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin)
