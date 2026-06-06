 <h1 style="color: #DCA657;">🧪 Module.Tests</h1>

 > Pester unit tests for the manifest and the exported public surface.

 ---

 | Suite | Scope |
 | --- | --- |
 | `Manifest` | Module manifest and Gallery metadata. |
 | `Exported surface` | Public entry point and helper privacy. |

 ---
 <h2 style="color: #DCA657;">Manifest</h2>

 Validates the module manifest and its Gallery metadata.

 <b style="color: #D2A8FF;">Cases</b>

```powershell
Describe 'Manifest'
```
 - Parses cleanly through `Test-ModuleManifest`.

```powershell
It 'Passes Test-ModuleManifest'
```
 - Pins the module version at `1.0.0`.

```powershell
It 'Declares version 1.0.0'
```
 - Targets the PowerShell `Core` edition.

```powershell
It 'Targets the Core edition'
```
 - Carries the `LicenseUri`, `ProjectUri`, and `SharpDown` tag the Gallery needs.

```powershell
It 'Carries Gallery metadata'
```
 ---
 <h2 style="color: #DCA657;">Exported surface</h2>

 Confirms only the public entry point escapes the module.

 <b style="color: #D2A8FF;">Cases</b>

```powershell
Describe 'Exported surface'
```
 - Exports the public `ConvertTo-SharpDown` command.

```powershell
It 'Exports ConvertTo-SharpDown'
```
 - Keeps the six internal helpers private.

```powershell
It 'Does not export the internal helpers'
```
