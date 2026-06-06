---
name: sharpdown
description: Make sure you use this skill whenever the following keywords apply -> SharpDown is a polyglot documentation convention, doc comment, doc generation, doc extraction, polyglot, multi-language, source as document, SharpDown, marker, fenced code block, code fence, declaration pattern, DeclPattern, mirror tree, extractor, inline docs, convention, csharp, .cs, public, protected, internal, static, abstract, sealed, override, async, javascript, js, .js, .mjs, export, import, class, function, const, let, sql, .sql, CREATE, ALTER, DROP, SELECT, INSERT, UPDATE, DELETE, GRANT, REVOKE, MERGE, powershell, .ps1, .psm1, filter, enum, workflow, ConvertTo-SharpDown
---

<h1 style="color: #DCA657;">🎆 Sharpdown</h1>

> Sharpdown is a source-as-document convention.
> Marker comments become Markdown.
> Declaration lines become fenced code.

---

| Read This First | Why It Matters |
| --- | --- |
| Source file | The comments you type. |
| Rendered Markdown | The document that ships. |
| Test fixture | The behavior contract. |

The source and the render are a pair.

Do not optimize one while making the other worse.

The output must read cleanly, top to bottom.

<h2 style="color: #DCA657;">Quick Start</h2>

```powershell
Import-Module <module-manifest> -Force
ConvertTo-SharpDown -Language PowerShell -Path <source-file> -OutPath <output-file>
```

File mode is the default.
Add `-Recurse` to mirror a directory tree.

<h2 style="color: #DCA657;">Renderer Contract</h2>

| Rule | Result |
| --- | --- |
| Marker line | Emits Markdown. |
| Bare marker | Emits a blank line. |
| Declaration line | Emits a fenced code block. |
| Everything else | Drops out. |

The renderer trims each line before classification.
Only spaces after the marker survive into the output.

```powershell
#### - top level
####     - nested once
####         - nested twice
```

<h2 style="color: #DCA657;">Markers</h2>

| Language | Marker | Carrier |
| --- | --- | --- |
| `PowerShell` | `####` | four hashtags |
| `CSharp` | `////` | four slashes |
| `JavaScript` | `////` | four slashes |
| `Sql` | `----` | four dashes |

PowerShell keeps `#` for ordinary comments.
C# keeps `///` for XML docs.
SQL keeps `--` for ordinary comments.

<h2 style="color: #DCA657;">Visual Language</h2>

Use the same landmarks as the generated docs.

| Role | Fragment |
| --- | --- |
| File title | `<h1 style="color: #DCA657;">🎆 Title</h1>` |
| Symbol title | `<h2 style="color: #DCA657;">Verb-Noun</h2>` |
| Inputs | `<b style="color: #D2A8FF;">Parameters</b>` |
| Outputs | `<b style="color: #369FFF;">Returns</b>` |
| Failures | `<b style="color: #C22514;">Throws</b>` |

Gold is navigation.
Purple is input.
Blue is output.
Red is failure.

Never invent a new color for the same job.

<h2 style="color: #DCA657;">File Header Pattern</h2>

```powershell
#### <h1 style="color: #DCA657;">🎆 ModuleName</h1>
####
#### > One sentence on what this file is.
####
#### ---
####
#### | Function | Purpose |
#### | --- | --- |
#### | `Verb-Noun` | One line on what it does. |
```

Keep summary tables narrow.
Two columns are usually enough.

<h2 style="color: #DCA657;">Function Pattern</h2>

```powershell
#### <h2 style="color: #DCA657;">Verb-Noun</h2>
####
function Verb-Noun {
    #### One sentence on what it does.
    ####
    #### <b style="color: #D2A8FF;">Parameters</b>
    ####
    param(
        #### - `[string]`: __Name__
        ####     - *What the parameter is.*
        [Parameter(Mandatory)][string]$Name
    )
    #### <b style="color: #369FFF;">Returns</b>
    #### - `[PSCustomObject]`
    ####     - *What comes back.*
}
#### ---
```

The declaration has no marker.
The renderer removes the trailing `{`.
The signature becomes a fenced block.

<h2 style="color: #DCA657;">Parameter Shape</h2>

```powershell
#### - `[type]`: __Name__
####     - *Description, italic, one sentence.*
```

| Part | Shape |
| --- | --- |
| Type | Backticked square brackets. |
| Name | Bold underscore. |
| Description | Italic nested bullet. |

Use one sentence per description.
Use one field per row.

<h2 style="color: #DCA657;">Return Shape</h2>

```powershell
#### - `[PSCustomObject]`
####     - `[string]`: __Source__
####         - *Redacted source path.*
####     - `[int]`: __Lines__
####         - *Number of lines written.*
```

Return fields mirror parameters.
They nest one level deeper.

<h2 style="color: #DCA657;">Throws Shape</h2>

```powershell
#### <b style="color: #C22514;">Throws</b>
#### - When `Path` is not a file.
#### - When `OutPath` is an existing directory.
if (-not (Test-Path $Path -PathType Leaf)) {
    throw "Path is not a file."
}
```

The guard clauses drop out.
The red landmark and bullets stay.

<h2 style="color: #DCA657;">Declaration Keywords</h2>

CSharp:

```text
public, protected, internal, static,
abstract, sealed, override, async
```

JavaScript:

```text
export, import, class, function,
async function, type, interface
```

PowerShell:

```text
function, filter, class, enum,
workflow, describe, it
```

Sql:

```text
CREATE, ALTER, DROP, SELECT, INSERT,
UPDATE, DELETE, GRANT, REVOKE, WITH,
COMMENT, CALL, TRUNCATE, MERGE
```

Use `-API` when the document is the wire contract.
In API mode, only marker lines emit.

<h2 style="color: #DCA657;">Pester Test Pattern</h2>

Use the same cadence as a module file.

| Test Element | Sharpdown Shape |
| --- | --- |
| File header | Gold `<h1>` and short blockquote. |
| Suite | Gold `<h2>` before `Describe`. |
| Cases | Purple `Cases` landmark. |
| Assertion | One bullet before each `It`. |

`Describe` and `It` fence on their own.
Write the heading, description, cases label, and assertion bullets.

<h2 style="color: #DCA657;">Vertical Flow</h2>

Vertical flow is not sparse text.

Vertical flow is readable rendered Markdown.

Use tables when comparison is the clearest shape.
Use lists when order or grouping is the clearest shape.
Use code blocks when syntax is the anchor.
Use whitespace when concepts need separation.

The test is the rendered page.

<h2 style="color: #DCA657;">Regenerate</h2>

Never hand-edit generated Markdown.

Change the source comments.
Then regenerate.

```powershell
Import-Module <module-manifest> -Force
ConvertTo-SharpDown -Language PowerShell -Path <source-file> -OutPath <output-file>
```

For a tree:

```powershell
ConvertTo-SharpDown -Language PowerShell -Path <source-root> -OutPath <output-root> -Recurse
```

Read the generated doc before calling the source done.

If a color, blank line, table, or list looks wrong,
fix the marker comments in the source.

<h2 style="color: #DCA657;">Tests</h2>

```powershell
Invoke-Pester <test-root>
```

Grow the tests first when adding a language, marker, or declaration pattern.
