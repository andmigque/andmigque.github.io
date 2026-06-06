 <h1 style="color: rgb(220, 166, 87);">🎆 Sharpdown</h1>

 > A polyglot commenting language for generated markdown from code comments

 ---

 | Language | File Types |
 | --- | --- |
 | `CSharp` | .cs |
 | `Sql` | .sql |
 | `JavaScript` | .js, .mjs, .ts, .tsx |
 | `PowerShell` | .ps1, .psm1 |
 <h2 style="color: #DCA657;">ConvertTo-RedactedPath</h2>

```powershell
function ConvertTo-RedactedPath
```
 Swap the user's home directory for `~` so log lines never spill private paths.

 <b style="color: #D2A8FF;">Parameters</b>
 - `[string]`: __Path__
     - *Path to redact.*

 <b style="color: #369FFF;">Returns</b>
 - `[string]`
     - *Same path with `$HOME` replaced by `~`.*
 ---
 <h2 style="color: #DCA657;">Convert-SharpDownContent</h2>

```powershell
function Convert-SharpDownContent
```
 Stream a source file's marker comments and declaration lines as Markdown.

 <b style="color: #D2A8FF;">Parameters</b>


 - `[string]`: __Source__
     - *Path to the source file.*
 - `[hashtable]`: __Config__
     - *Language entry from `$script:SharpDownConfig`.*
 - `[switch]`: __API__
     - *Suppress auto-generated declaration fences. Only marker lines emit.*

 | Key | Description |
 | --- | --- |
 | `Marker` | The line prefix that signals a doc comment. |
 | `Fence` | The code fence label applied to declarations. |
 | `DeclPattern` | The regex that marks a line as a declaration. |

 ---
 Read and trim each source line so the tests ignore indentation.

 ---
 - A marker line is doc text: emit it with the prefix removed.

 - Otherwise a declaration line becomes a fenced signature, unless `-API`.

 - Drop the trailing brace so the fence holds just the declaration.

 - Fence the cleaned signature with the language's label.

 <b style="color: #369FFF;">Returns</b>
 - `[string[]]`
     - *Doc lines, with declarations fenced unless `-API` is set.*
 ---
 <h2 style="color: #DCA657;">Resolve-SharpDownTarget</h2>

```powershell
function Resolve-SharpDownTarget
```
 Compute the mirrored Markdown output path for a source file under `-Recurse`.

 <b style="color: #D2A8FF;">Parameters</b>


 - `[string]`: __Root__
     - *Walk root passed to `-Recurse`.*
 - `[string]`: __Source__
     - *Absolute path of the source file.*
 - `[string]`: __ProjectName__
     - *Leaf of `Root`. Stripped if it leads the relative path.*
 - `[string]`: __OutRoot__
     - *Output root passed as `-OutPath`.*

 <b style="color: #369FFF;">Returns</b>
 - `[string]`
     - *Absolute path of the `.md` to write.*
 ---
 <h2 style="color: #DCA657;">Write-SharpDownFile</h2>

```powershell
function Write-SharpDownFile
```
 Convert a single source file and write the result. Skips with a warning when the source has no SharpDown content. Creates the output directory on demand.

 <b style="color: #D2A8FF;">Parameters</b>


 - `[string]`: __Source__
     - *Path to the source file.*
 - `[string]`: __Target__
     - *Path of the `.md` to write.*
 - `[hashtable]`: __Config__
     - *Language entry from `$script:SharpDownConfig`.*
 - `[switch]`: __API__
     - *Forwarded to `Convert-SharpDownContent`. Suppresses auto-fenced declarations.*

 <b style="color: #369FFF;">Returns</b>
 - `[PSCustomObject]`
     - `[string]`: __Source__
         - *Redacted source path.*
     - `[string]`: __Target__
         - *Redacted target path.*
     - `[int]`: __Lines__
         - *Number of lines emitted to the target.*
 ---
 <h2 style="color: #DCA657;">Invoke-SharpDownFile</h2>

```powershell
function Invoke-SharpDownFile
```
 Validate the File-mode contract and convert one source.

 <b style="color: #D2A8FF;">Parameters</b>


 - `[string]`: __Path__
     - *Source file path. Must be a leaf.*
 - `[string]`: __OutPath__
     - *Target `.md` path. Must not be an existing directory.*
 - `[hashtable]`: __Config__
     - *Language entry from `$script:SharpDownConfig`.*
 - `[switch]`: __API__
     - *Forwarded to `Write-SharpDownFile`.*

 <b style="color: #C22514;">Throws</b>
 - When `Path` is not a file.
 - When `OutPath` resolves to an existing directory.
 <h2 style="color: #DCA657;">Invoke-SharpDownTree</h2>

```powershell
function Invoke-SharpDownTree
```
 Validate the Directory-mode contract and walk a source tree.

 <b style="color: #D2A8FF;">Parameters</b>


 - `[string]`: __Path__
     - *Source root. Must be a directory.*
 - `[string]`: __OutPath__
     - *Output root. Must not be an existing file.*
 - `[hashtable]`: __Config__
     - *Language entry from `$script:SharpDownConfig`.*
 - `[string]`: __Language__
     - *Language name used in the no-files warning.*
 - `[switch]`: __API__
     - *Forwarded to `Write-SharpDownFile` for each source file under the tree.*

 <b style="color: #C22514;">Throws</b>
 - When `Path` is not a directory.
 - When `OutPath` resolves to an existing file.
 | Key | Description |
 | --- | --- |
 | `Extensions` | The set of file extensions walked under `-Recurse`. |
 | `ExcludeRegex` | The directory pattern skipped under `-Recurse`. |
 <h2 style="color: #DCA657;">ConvertTo-SharpDown</h2>

```powershell
function ConvertTo-SharpDown
```
 Public entry point. Convert a single source or a tree of sources to Markdown.

 Two parameter sets:

 - **File** is the default. `-Path` is a single source file, `-OutPath` is the `.md` target.
 - **Directory** requires `-Recurse`. `-Path` is a source directory, `-OutPath` is the output root. `-Path` accepts pipeline input from `Get-Item` and `Get-ChildItem -Directory`.

 <b style="color: #D2A8FF;">Parameters</b>

 - `[string]`: __Language__
     - *One of `CSharp`, `Sql`, `JavaScript`, `PowerShell`. Defaults to `CSharp`.*
 - `[string]`: __Path__
     - *Source file in File set, source directory in Directory set. Pipeline-bound only in Directory set.*
 - `[string]`: __OutPath__
     - *Output file in File set, output root in Directory set.*
 - `[switch]`: __Recurse__
     - *Selects the Directory set.*
 - `[switch]`: __API__
     - *Suppresses auto-generated declaration fences. Use for HTTP endpoint files where the doc IS the wire contract, not the language signatures.*

 <b style="color: #369FFF;">Returns</b>
 - `[PSCustomObject[]]`
     - `[string]`: __Source__
         - *Redacted source path.*
     - `[string]`: __Target__
         - *Redacted target path.*
     - `[int]`: __Lines__
         - *Number of lines emitted to the target.*
