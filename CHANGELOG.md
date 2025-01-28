# Changelog

## [3.2.0] - 2025-01-23

- Target .NET 8 and 9.

## [3.1.1] - 2024-03-05

### Fixed

- Updated `GetFilesAsync` in the `FileSystem` implementation so that it doesn't throw an exception if the path doesn't
  exist, making it consistent with the other implementations.

## [3.1.0] - 2024-01-25

### Changed

- Added `ConfigureAwait(false)` call to awaited tasks.

## [3.0.0] - 2024-01-25

### Added

- Added a method to list files in a specified path.

### Changed

- **Breaking change** This release changes how paths are built when using Azure Blob Storage in order to avoid platform
  inconsistencies. It's possible this could result in `FileExistsAsync` and `GetFilesAsync` failing for existing files.
  Thorough testing is recommended when updating.

## [2.1.0] - 2024-01-16

### Added

- Added an extension method for registering and configuring file services.

## [2.0.0] - 2024-01-02

- Upgraded to .NET 8.0.

## [1.0.0] - 2023-10-25

_Initial release._

[3.2.0]: https://github.com/gaepdit/file-service/releases/tag/v3.2.0
[3.1.1]: https://github.com/gaepdit/file-service/releases/tag/v3.1.1
[3.1.0]: https://github.com/gaepdit/file-service/releases/tag/v3.1.0
[3.0.0]: https://github.com/gaepdit/file-service/releases/tag/v3.0.0
[2.1.0]: https://github.com/gaepdit/app-library/releases/tag/f%2Fv2.1.0
[2.0.0]: https://github.com/gaepdit/app-library/releases/tag/fs%2Fv2.0.0
[1.0.0]: https://github.com/gaepdit/app-library/releases/tag/fs%2Fv1.0.0
