# AnyPackage.WinGet
AnyPackage.WinGet is an AnyPackage provider that facilitates installing WinGet packages from any compatible repository.

## Requirements
Your machine must have at least Windows 10 1709 or Windows 11 and either PowerShell 5.1+ or PowerShell 7.0.1+, and the WinGet CLI utility installed. It may be already installed on your machine, but if not, Microsoft's recommended method for installing WinGet is via the Microsoft Store as part of the [App Installer](https://www.microsoft.com/en-us/p/app-installer/9nblggh4nns1?activetab=pivot:overviewtab) package.

## Install AnyPackage.WinGet
```PowerShell
Install-Module AnyPackage.WinGet -Force
```

## Import AnyPackage.WinGet
```PowerShell
Import-Module AnyPackage.WinGet
```

## Sample usages

### Search for a package
```PowerShell
Find-Package -Name OpenJS.NodeJS

Find-Package -Name Mozilla.Firefox
```

### Install a package
```PowerShell
Find-Package OpenJS.NodeJS | Install-Package

Install-Package -Name Git.Git
```

### Get list of installed packages (with wildcard search support)
```PowerShell
Get-Package Microsoft.*
```

### Uninstall a package
```PowerShell
Get-Package OpenJS.NodeJS| Uninstall-Package
Uninstall-Package Git.Git
```

### Manage package sources
```PowerShell
Register-PackageSource privateRepo -Provider WinGet -Location 'https://somewhere/out/there/cache'
Find-Package OpenJS.NodeJS -Source privateRepo | Install-Package
Unregister-PackageSource privateRepo
```
AnyPackage.WinGet integrates with WinGet.exe to manage and store source information

## Known Issues
### Stability
WinGet does not currently have an official module available on PowerShell Gallery, therefore this provider currently depends on a  Cresendo module that is a best-effort attempt at parsing WinGet's output. Due to WinGet still being heavily in development, it's output patterns fluctuate regularly, making a Cresdendo-based implementation very brittle. As such, currently this provider **should not** be used in production scenarios. 

### Output Consistency
Due to WinGet's ability to return lists of installed packages that have no version number, and the AnyPackage framework's requirement that all packages have a version, this provider sets the version to 0 for packages that have no version number.

### Search Specificity
Due to WinGet's ambiguous search behavior and the risks of unintended outcomes, this provider only allows searching of packages on remote repositories by exact package ID. Fuzzy searches and searches by moniker, display name, and tag are not supported. 

## Legal and Licensing
AnyPackage.WinGet is licensed under the [MIT license](./LICENSE.txt).
