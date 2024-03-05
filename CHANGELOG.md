# Changelog

## [3.1.1]

### Fixed

- Updated `GetFilesAsync` in the `FileSystem` implementation so that it doesn't throw an exception if the path doesn't
  exist, making it consistent with the other implementations.

## [3.1.0]

### Changed

- Added `ConfigureAwait(false)` call to awaited tasks.

## [3.0.0]

### Added

- Added a method to list files in a specified path.

### Changed

- **Breaking change** This release changes how paths are built when using Azure Blob Storage in order to avoid platform
  inconsistencies. It's possible this could result in `FileExistsAsync` and `GetFilesAsync` failing for existing files.
  Thorough testing is recommended when updating.

## [2.1.0]

### Added

- Added an extension method for registering and configuring file services.

## [2.0.0]

- Upgraded to .NET 8.0.

## [1.0.0]

- Initial release.
