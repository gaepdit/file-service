# Georgia EPD-IT File Service Library

This library was created by Georgia EPD-IT to provide common file services for our web applications.

[![Georgia EPD-IT](https://raw.githubusercontent.com/gaepdit/gaepd-brand/main/blinkies/blinkies.cafe-gaepdit.gif)](https://github.com/gaepdit)
[![.NET Test](https://github.com/gaepdit/file-service/actions/workflows/dotnet-test.yml/badge.svg)](https://github.com/gaepdit/file-service/actions/workflows/dotnet-test.yml)
[![SonarCloud Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=gaepdit_file-service&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=gaepdit_file-service)
[![Lines of Code](https://sonarcloud.io/api/project_badges/measure?project=gaepdit_file-service&metric=ncloc)](https://sonarcloud.io/summary/new_code?id=gaepdit_file-service)

## Installation

[![Nuget](https://img.shields.io/nuget/v/GaEpd.FileService)](https://www.nuget.org/packages/GaEpd.FileService)

To install, search for "GaEpd.FileService" in the NuGet package manager or run the following command:

`dotnet add package GaEpd.FileService`

## Interface

An `IFileService` interface is used to abstract out common file persistence operations:

* `SaveFileAsync`
* `FileExistsAsync`
* `ListFilesAsync`
* `GetFileAsync`
* `TryGetFileAsync`
* `DeleteFileAsync`

## Implementations

The library includes three useful File Service implementations that store files in memory, the file system, or Azure
Blob Storage. A description of each implementation and its parameters is below.

### Registration

The simplest way to register one of the implementations is to use the `AddFileServices()` extension method and configure
the desired implementation in the app configuration. Add the following line to your Program file:

```csharp
builder.AddFileServices();
```

And add the following section to your `appsettings.json` configuration file:

```json
{
  "FileServiceSettings": {
    "FileService": "",
    "FileSystemBasePath": "",
    "NetworkUsername": "",
    "NetworkDomain": "",
    "NetworkPassword": "",
    "AzureAccountName": "",
    "AzureTenantId": "",
    "BlobContainer": "",
    "BlobBasePath": ""
  }
}
```

The `FileService` setting must be included and must be set to either `InMemory`, `FileSystem`, or `AzureBlobStorage`.
Other settings only need to be included if required by the selected implementation.

* If `InMemory` is chosen, all other settings are ignored and can be left out.

* If `FileSystem` is chosen, then `FileSystemBasePath` is required. `NetworkUsername`, `NetworkDomain`, and
  `NetworkPassword` can be provided if needed. Other settings are ignored.

* If `AzureBlobStorage` is chosen, then `AzureAccountName` and `BlobContainer` are required. `BlobBasePath` and
  `AzureTenantId` can be provided if needed. Other settings are ignored.

Alternatively, you can directly register one of the implementations as shown in the sections below.

### In Memory

The in-memory implementation stores files in memory. All data will be lost when the app restarts (only useful for
development).

```csharp
builder.Services.AddSingleton<IFileService, InMemory>();
```

### File System

The file system implementation writes files to a local or network drive. The `basePath` parameter is required and
defines where the files will be stored. If `basePath` doesn't exist in the file system, it will be created.

```csharp
builder.Services.AddTransient<IFileService, FileSystem>(_ => new FileSystem(basePath));
```

If a Windows Identity is required to access the desired file location, use the overload that accepts `username`,
`domain`, and `password` parameters in the constructor.

```csharp
builder.Services.AddTransient<IFileService, FileSystem>(_ => new FileSystem(basePath, username, domain, password));
```

### Azure Blob Storage

The Azure Blob Storage service requires an Azure account and an existing Blob Storage container. (The service does not
attempt to create the container if it does not exist.) The `basePath` parameter is optional and is prepended to file
names as a path segment.

The `tenantId` parameter is also optional and specifies the ID of the tenant to which the credential will authenticate
by default. (This is useful in multi-tenant situations where the desired Tenant ID is not the default.)

```csharp
builder.Services.AddSingleton<IFileService, AzureBlobStorage>(_ => new AzureBlobStorage(accountName, container, basePath, tenantId));
```

#### Azure Blob Storage Authentication

This library uses the
[`DefaultAzureCredential`](https://learn.microsoft.com/en-us/dotnet/azure/sdk/authentication/?tabs=command-line#defaultazurecredential)
class to authenticate with Azure Blob Storage. Review the documentation
on [using developer accounts during local development](https://learn.microsoft.com/en-us/dotnet/azure/sdk/authentication/local-development-dev-accounts?tabs=azure-portal%2Csign-in-azure-powershell%2Ccommand-line).
