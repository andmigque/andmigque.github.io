 <h1 style="color: #DCA657;">🧪 SharpDown.Tests</h1>

 > Pester unit tests for `ConvertTo-SharpDown`. Ported from the legacy Test-SharpDown.ps1 smoke harness.

 The temp-path helpers live in BeforeAll because Pester 5 only exposes functions defined there
 to the It blocks at run time. Top-level function definitions run during discovery and vanish.
```powershell
function New-TempLeafPath
```
```powershell
function New-TempWorkspace
```
 ---
 <h2 style="color: #DCA657;">CSharp file mode</h2>

 Single-file conversion of C# sources.

 <b style="color: #D2A8FF;">Cases</b>

```powershell
Describe 'CSharp file mode'
```
 - Strips the `////` marker and fences the declaration lines.

```powershell
It 'Strips //// markers and fences declaration lines'
```
 - Drops lines that are neither markers nor declarations.

```powershell
It 'Skips lines that are neither markers nor declarations'
```
 - Throws when the source path is not a file.

```powershell
It 'Throws when File path is not a file'
```
 - Throws when `-OutPath` points at an existing directory.

```powershell
It 'Throws when File OutPath points to an existing directory'
```
 - Warns when the source holds no SharpDown content.

```powershell
It 'Warns when source has no SharpDown content'
```
 ---
 <h2 style="color: #DCA657;">Language configs</h2>

 Per-language markers, fences, and the `-API` switch.

 <b style="color: #D2A8FF;">Cases</b>

```powershell
Describe 'Language configs'
```
 - SQL: the `----` marker fences `CREATE` statements.

```powershell
It 'Sql marker ---- fences CREATE statements'
```
 - PowerShell: the `####` marker fences function declarations.

```powershell
It 'PowerShell marker #### fences function declarations'
```
 - JavaScript: the `////` marker fences `export class` declarations.

```powershell
It 'JavaScript marker //// fences export class declarations'
```
 - `-API` suppresses every auto-generated declaration fence.

```powershell
It '-API suppresses all auto-generated declaration fences'
```
 - JavaScript keeps top-level function and type decls, drops interior locals.

```powershell
It 'JavaScript drops interior const/let locals but keeps top-level function and type declarations'
```
 ---
 <h2 style="color: #DCA657;">Directory mode</h2>

 Recursive tree walking, mirroring, and pipeline input.

 <b style="color: #D2A8FF;">Cases</b>

```powershell
Describe 'Directory mode'
```
 - Throws when the `-Recurse` path is not a directory.

```powershell
It 'Throws when -Recurse path is not a directory'
```
 - Throws when `-OutPath` points at an existing file.

```powershell
It 'Throws when -Recurse OutPath points to an existing file'
```
 - Without `-Recurse`, a directory path throws as not-a-file.

```powershell
It 'Without -Recurse, a directory path throws as not-a-file'
```
 - Mirrors the tree, stripping `src/` and project-name legs.

```powershell
It 'Mirrors tree, strips src/ and project name legs'
```
 - Accepts a piped `DirectoryInfo` as `-Path` under `-Recurse`.

```powershell
It 'Accepts a piped DirectoryInfo as -Path under -Recurse'
```
 - Processes multiple piped directories in one invocation.

```powershell
It 'Processes multiple directories piped into a single invocation'
```
 - Warns when no files match the language extensions.

```powershell
It 'Warns when directory has no matching extensions'
```
