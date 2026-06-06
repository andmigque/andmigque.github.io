 <h1 style="color: #DCA657;">🔨 Sharpdown.Build</h1>

 > InvokeBuild tasks for dependency installation, tests, analyzer output, and generated docs.

 ---

 | Task | Purpose |
 | --- | --- |
 | `install_modules` | Install manifest-declared module dependencies for the current user. |
 | `test_pester` | Run the Pester suite and write result artifacts under `Generated`. |
 | `test_ps_script_analyzer` | Run PSScriptAnalyzer and write its report under `Generated`. |
 | `build_sharpdown` | Regenerate the generated Markdown docs from PowerShell source. |
 | `.` | Run the full build chain. |

 ---
 <h2 style="color: #DCA657;">install_modules</h2>

 Installs each module listed in the manifest's `RequiredModules` field.

 <b style="color: #D2A8FF;">Inputs</b>
 - `Sharpdown.psd1`
     - *The build reads `RequiredModules` from the module manifest.*

 <b style="color: #369FFF;">Outputs</b>
 - Installed modules in the current user's module scope.
 ---
 <h2 style="color: #DCA657;">test_pester</h2>

 Runs the Pester 5 suite and fails the build on any failing test.

 <b style="color: #D2A8FF;">Inputs</b>
 - `Tests`
     - *The Pester test tree.*
 - `Sharpdown.psm1`
     - *The module file measured by code coverage.*

 <b style="color: #369FFF;">Outputs</b>
 - `Generated/PesterTests.xml`
     - *Pester test result artifact.*
 - `Generated/PesterCodeCoverage.xml`
     - *Code coverage artifact for the module.*
 ---
 <h2 style="color: #DCA657;">test_ps_script_analyzer</h2>

 Runs PSScriptAnalyzer across the repository and writes a report.

 <b style="color: #D2A8FF;">Inputs</b>
 - Repository PowerShell files.
     - *The analyzer walks from the repository root.*

 <b style="color: #369FFF;">Outputs</b>
 - `Generated/ScriptAnalyzer.txt`
     - *Analyzer report with the default rules and suppressed diagnostics included.*
 ---
 <h2 style="color: #DCA657;">build_sharpdown</h2>

 Regenerates the `Docs` tree from every PowerShell source file.

 <b style="color: #D2A8FF;">Inputs</b>
 - Repository PowerShell files.
     - *`ConvertTo-SharpDown` walks the repository in PowerShell mode.*

 <b style="color: #369FFF;">Outputs</b>
 - `Docs`
     - *Generated Markdown mirror of the documented PowerShell source.*
 ---
 <h2 style="color: #DCA657;">default</h2>

 Runs the full build chain in dependency, test, analyzer, and docs order.

 <b style="color: #D2A8FF;">Tasks</b>
 - `install_modules`
 - `test_pester`
 - `test_ps_script_analyzer`
 - `build_sharpdown`
