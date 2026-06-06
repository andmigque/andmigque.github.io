---
name: vertical-flow
description: Use this skill when writing, editing, or reviewing Markdown, generated documentation, skill files, README prose, source-comment documentation, or any rendered text where viewport width, visual hierarchy, scan rhythm, chunking, line length, tables, lists, code anchors, or mobile readability matters.
---

<h1 style="color: #DCA657;">🎆 Vertical Flow</h1>

> Vertical flow is the discipline of making rendered Markdown read cleanly.
> It optimizes the page the reader sees, not only the source text you type.

---

| Principle | Outcome |
| --- | --- |
| Fit the viewport | No horizontal displacement. |
| Stack the hierarchy | The eye always has a next place to land. |
| Chunk one idea | The reader never carries two burdens at once. |
| Anchor with code | Syntax becomes a visual landmark. |

Vertical flow is not sparse text.

Vertical flow is not tables versus lists.

Vertical flow is the combination that gives the best rendered output.

<h2 style="color: #DCA657;">Rendering Goal</h2>

The rendered page is the artifact.

Source formatting matters because it shapes the render.

Rendered clarity wins when source neatness and output quality disagree.

Use the form that makes the page easiest to scan:

| Content Shape | Best Form |
| --- | --- |
| Compare two axes | Narrow table. |
| Show ordered steps | Numbered list. |
| Group related facts | Bulleted list. |
| Teach syntax | Code block. |
| Mark a section | Colored heading. |

Use this selector when choosing a shape:

```powershell
function Select-VerticalShape {
    param(
        [Parameter(Mandatory)]
        [ValidateSet('Compare', 'Procedure', 'Group', 'Syntax', 'Warning')]
        [string]$ContentKind
    )

    switch ($ContentKind) {
        'Compare' {
            'Table'
        }
        'Procedure' {
            'NumberedList'
        }
        'Group' {
            'BulletList'
        }
        'Syntax' {
            'CodeBlock'
        }
        'Warning' {
            'RedLandmark'
        }
    }
}
```

<h2 style="color: #DCA657;">Viewport Contract</h2>

The reader should never need horizontal navigation to understand the document.

| Element | Target |
| --- | --- |
| Headers | 40 to 60 characters. |
| Prose | 60 to 80 characters. |
| Tables | Two or three narrow columns. |
| Lists | One idea per item. |
| Code | As narrow as the syntax allows. |

Long code can exist.

Long prose should not.

Score a draft before calling it done:

```powershell
$verticalFlowScore = [ordered]@{
    HeaderUnderSixty = $true
    ProseUnderEighty = $true
    TablesAreNarrow = $true
    CodeAnchorsSyntax = $true
    OneIdeaPerBlock = $true
}
```

Any `$false` value is a rewrite signal.

<h2 style="color: #DCA657;">Visual Language</h2>

Use repeated landmarks.

Readers learn the page through rhythm.

| Role | Shape |
| --- | --- |
| Primary section | Gold heading. |
| Supporting idea | Short paragraph. |
| Comparison | Compact table. |
| Procedure | Numbered steps. |
| Syntax | Fenced code block. |
| Warning | Red landmark. |

Keep the same visual role in the same visual shape.

Do not make the reader relearn the page section by section.

<h2 style="color: #DCA657;">Gold Heading Pattern</h2>

Use gold headings as navigation anchors.

```html
<h1 style="color: #DCA657;">🎆 Document Title</h1>
```

```html
<h2 style="color: #DCA657;">Section Title</h2>
```

The heading should name the thing.

Do not use decorative headings that hide the content.

Use this section skeleton:

```markdown
<h2 style="color: #DCA657;">Section Name</h2>

One sentence states the point.

| Signal | Meaning |
| --- | --- |
| Concrete thing | Concrete meaning. |

One sentence closes the block.
```

<h2 style="color: #DCA657;">Compact Table Pattern</h2>

Use a table when comparison is the point.

```markdown
| Signal | Meaning |
| --- | --- |
| Gold heading | Navigation. |
| Code block | Syntax anchor. |
| Blank line | Concept boundary. |
```

Keep tables narrow.

Prefer two columns.

Use three only when the third column earns its width.

Do not do this:

```markdown
| Signal | Meaning | Notes | Reader Action |
| --- | --- | --- | --- |
| Gold heading | Navigation | Use everywhere | Scan section starts |
```

Do this:

```markdown
| Signal | Meaning |
| --- | --- |
| Gold heading | Navigation. |
```

<h2 style="color: #DCA657;">List Pattern</h2>

Use a list when items belong to one concept.

```markdown
- one signal
- one meaning
- one action
```

One item should carry one idea.

If an item needs a paragraph, split the concept.

Do not do this:

```markdown
- Use headings for navigation, tables for comparison, lists for groups,
  and code blocks for syntax because each one solves a different job.
```

Do this:

```markdown
- Headings mark navigation.
- Tables compare.
- Lists group facts.
- Code blocks anchor syntax.
```

<h2 style="color: #DCA657;">Code Anchor Pattern</h2>

Use code blocks when syntax is the thing being taught.

```powershell
switch ($Task) {
    'test' {
        Invoke-Pester <test-root>
    }
    'docs' {
        ConvertTo-SharpDown -Path <source-root> -OutPath <output-root> -Recurse
    }
}
```

Code blocks are visual anchors.

They should clarify structure, not decorate the page.

Prefer code that encodes the rule:

```powershell
if ($Line.Length -gt 80 -and -not $LineIsCode) {
    'Rewrite'
}
```

Avoid code that is only atmospheric:

```powershell
Write-Host 'Looks clean'
```

<h2 style="color: #DCA657;">Chunking Pattern</h2>

Each block should carry one primary concept.

| Block | Carries |
| --- | --- |
| Heading | The destination. |
| Paragraph | The claim. |
| Table | The comparison. |
| List | The grouped facts. |
| Code | The syntax. |

Whitespace separates concepts.

Do not use whitespace as decoration.

Use one block per concept:

```markdown
<h2 style="color: #DCA657;">One Concept</h2>

One claim.

One supporting shape.

One closing sentence.
```

<h2 style="color: #DCA657;">Progressive Density</h2>

Move from identity to proof.

| Layer | Purpose |
| --- | --- |
| One | Name the document. |
| Two | State the rule. |
| Three | Show the pattern. |
| Four | Prove with an example. |

The reader should understand the page before reaching the example.

The example should confirm the rule, not rescue it.

<h2 style="color: #DCA657;">Anti Patterns</h2>

<b style="color: #C22514;">Avoid</b>

- Wide prose blocks.
- Wide tables with repeated phrases.
- Dense bullets with multiple ideas.
- Headings that end in colons.
- Decorative code blocks.
- Sparse text that looks clean but reads thin.
- Source formatting that damages rendered output.

<h2 style="color: #DCA657;">Validation</h2>

Read the rendered page.

Ask these questions:

| Question | Passing Answer |
| --- | --- |
| Can I read without zooming? | Yes. |
| Can I scroll without losing context? | Yes. |
| Do headings form a map? | Yes. |
| Do tables compare instead of sprawl? | Yes. |
| Do code blocks anchor syntax? | Yes. |

The final test is visual.

If the render looks worse, the source is not done.

Run this review loop:

```powershell
foreach ($Section in $RenderedDocument.Sections) {
    Test-HeaderMap $Section
    Test-LineLength $Section
    Test-ShapeChoice $Section
    Test-ConceptBoundary $Section
}
```

The names are pseudo-code.
The discipline is real.
