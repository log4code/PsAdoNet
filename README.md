# PsAdoNet

The goal of PsAdoNet is to encourage the development of composeable ADO.NET solutions using PowerShell. Adhering to Powershell's core philosophy of doing one thing and doing it well, the steps of interacting with a data source are broken into small [tools](https://donjones.com/PowerShell/), as opposed to a single-serving function.

## Getting Started

```powershell
# Installing for a specific user
Copy-Item -Path {rest of path}\PsAdoNet `
          -Destination $HOME\Documents\PowerShell\Modules

# Close & re-open shell window
# Verify it worked
Get-Command -Module PsAdoNet
```

> For more detail installation instructions see the [official docs](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_modules?view=powershell-7.1#how-to-install-a-module)

## Quick Start

A simple example demonstrating the execution of a simple query against a SQL Server Instance.

```powershell
# Create a DbConnection
$conn = New-SqlServerConnection -ServerInstance SQLVM01

# Create a DbCommand
$cmd = New-DbCommand -Query "SELECT * FROM Products"

# Invoke query and display result
$cmd | Invoke-DbCommand | Format-Table

# Properly dispose of objects
Close-Resource $conn, $cmd
```

An example demonstrating efficient and practical ETL pattern using `SqlBulkCopy` and an `IDataReader`.

```powershell
$source = New-SqlServerConnection -ServerInstance SQLVM01 -Database Northwind
$dest = New-SqlServerConnection -ServerInstance SQLVM02 -Database Northwind

$rd = $source | New-DbCommand -Query "SELECT * FROM Products" | Invoke-DbCommand -Reader
$bcp = Invoke-SqlServerBulkCopy $rd -Connection $dest -Table "Products" -BatchSize 5000 -BulkCopyTimeout 30

Close-Resource $source, $dest, $rd, $bcp
```

## Find a bug?

There's an [issue](https://github.com/pimbrouwers/PsAdoNet/issues) for that.

## License

Built with ♥ by [Pim Brouwers](https://github.com/pimbrouwers) in Toronto, ON. Licensed under [Apache License 2.0](https://github.com/pimbrouwers/PsAdoNet/blob/master/LICENSE).