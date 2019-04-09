# Markdown Guide
This is a quick guide [^1] for Markdown, a lightweight and easy-to-use syntax for writing.

## What is Markdown?
Markdown is a way to style text via a few simple marker characters. You could write the document in plain text and then read it with a beautiful typesetting.

There is no a standard Markdown syntax and many editor will add its own additional syntax. VNote supports only the widely-used basic syntax.

## How to Use Markdown?
If you are new to Markdown, it is better to learn the syntax elements step by step. Knowing headers and emphasis is enough to make you survive. You could learn another new syntax and practise it every one or two days.

## Syntax Guide
Here is an overview of Markdown syntax supported by VNote.

### Headers
```md
# This is an <h1> tag
## This is an <h2> tag
###### This is an <h6> tag
```

**Notes**:

- At least one space is needed after the `#`;
- A header should occupy one entire line;

### Emphasis
```md
*This text will be italic*  
_This text will be italic_  

**This text will be bold**  
__This text will be bold__
```

**Notes**:

- `*` is recommended in VNote;
- If the render failed, try to add an additional space before the first `*` and after the last `*`. The space is necessary if the surrounded text begins or ends with full width punctuation;
- VNote provides shortcuts `Ctrl+I` and `Ctrl+B` to insert italic and bold text;

### Lists
#### Unordered
```md
* Item 1  
This is a text under Item 1. Notice that there are two spaces at the end above.
* Item 2
    * Item 2a
    * Item 2b
* Item 3

To end a list, there should be one empty line above.
```

#### Ordered
```md
1. Item 1
1. Item 2  
Notice that the sequence number is irrelevant. Markdown will change the sequence automatically when renderring.
3. Item 3
    1. Item 3a
    2. Item 3b
4. Item 4
```

### Tables
```md
| col 1 | col 2 | col 3 |
| --- | --- | --- |
| cell1 | cell2 | cell3 |
| cell4 | cell5 | cell6 |
```

### Images and Links
```md
![Image Alt Text](/url/to/image.png "Optional Text")

![Image Alt Text](/url/to/image.png "Image specified with width and height" =800x600)

![Image Alt Text](/url/to/image.png =800x600)

![Image Alt Text](/url/to/image.png "Image specified with width" =800x)

![Image Alt Text](/url/to/image.png "Image specified with height" =x600)

[Link Text](/url/of/the/link)
```

**Notes**:

- It is not recommended to use image links in reference format. VNote will not preview those images.
- Specifying size of image is supported only in **markdown-it**.

### Blockquotes
```md
As VNote suggests:

> VNote is the best Markdown note-taking application
> ever.  
>
> THere is two spaces after `ever.` above to insert a
> new line.
```

**Notes**:

- Space is needed after the marker `>`;
- You could just add only one `>` at the first line;

### Fenced Code Block
    ```lang
    This is a fenced code block.
    ```

    ~~~
    This is another fenced code block.
    ~~~

**Notes**:

- `lang` is optional to specify the language of the code; if not specified, VNote won't highlight the code;

### Diagrams

> You need to enable Flowchart.js or Mermaid or WaveDrom in the `Markdown` menu and restart current opened tabs.

VNote supports the following engines to draw diagrams. You should specify particular language of the fenced code block and write the definition of your diagram within it.

- [Flowchart.js](http://flowchart.js.org/) for *flowchart* with language `flow` or `flowchart`;
- [Mermaid](https://mermaidjs.github.io/) with language `mermaid`;
- [WaveDrom](https://wavedrom.com/) for *digital timing diagram* with language `wavedrom`;

For example,

    ```flowchart
    st=>start: Start:>http://www.google.com[blank]
    e=>end:>http://www.google.com
    op1=>operation: My Operation
    sub1=>subroutine: My Subroutine
    cond=>condition: Yes
    or No?:>http://www.google.com
    io=>inputoutput: catch something...

    st->op1->cond
    cond(yes)->io->e
    cond(no)->sub1(right)->op1
    ```

#### UML

> You need to enable PlantUML in the settings. Pay attention to the privacy issue if you use online PlantUML server. You may need to prepare Java runtime, PlantUML, and Graphviz if you choose local PlantUML.

VNote supports [PlantUML](http://plantuml.com/) to draw UML diagrams. You should use `puml` specified as the language of the fenced code block and write the definition of your diagram within it.

    ```puml
    @startuml
    Bob -> Alice : hello
    @enduml
    ```

#### Graphviz

> You need to enable Graphviz in the settings.

VNote supports [Graphviz](http://www.graphviz.org/) to draw diagrams. You should use `dot` specified as the language of the fenced code block and write the definition of your diagram within it.

### Math Formulas

> You need to enable MathJax in the `Markdown` menu and restart VNote.

VNote supports math formulas via [MathJax](https://www.mathjax.org/). The default math delimiters are `$$...$$` for **displayed mathematics**, and `$...$` for **inline mathematics**.

- Inline mathematics should not cross multiple lines;
- Forms like `3$abc$`, `$abc$4`, `$ abc$`, and `$abc $` will not be treated as mathematics;
- Use `\` to escape `$`;
- There should be only space chars before opening `$$` and after closing `$$`;

VNote also supports displayed mathematics via fenced code block with language `mathjax` specified.

    ```mathjax
    $$
    J(\theta) = \frac 1 2 \sum_{i=1}^m (h_\theta(x^{(i)})-y^{(i)})^2
    $$
    ```

Equation number of displayed mathematics is supported:

    $$vnote x markdown = awesome$$ (1.2.1)

### Inline Code
```md
Here is a `inline code`.
```

To insert one `` ` ``, you need to use two `` ` `` to enclose it, such as ``` `` ` `` ```. To insert two `` ` ``, you need to use three `` ` ``.

### Strikethrough
```md
Here is a ~~text~~ with strikethrough.
```

**Notes**:

- VNote provides shortcuts `Ctrl+D` to insert text with strikethrough;

### Task Lists
```md
- [x] this is a complete item.
- [ ] this is an incomplete item.
```

### Footnote
```md
This is a footnote [^1].

[^1]: Here is the detail of the footnote.
```

### Superscript and Subscript

> You need to enable Superscript or Subscript in the `Markdown` menu and use `Markdown-it` renderer.

```md
This is the 1^st^ superscript.

This is the H~2~O subscript.
```

### Alert
With `Markdown-it` renderer, you can add some alert text.

```md
::: alert-info

This is an info text.

:::

::: alert-danger

This is a danger text.

:::
```

Available variants:

```
alert-primary
alert-secondary
alert-success
alert-info
alert-warning
alert-danger
alert-light
alert-dark
```

### New Line and Paragraph
If you want to enter a new line, you should add two spaces after current line and then continue your input. VNote provides `Shift+Enter` to help.

If you want to enter a new paragraph, you should add an empty line and then continue entering the new paragraph.

Generally, you need to add an empty line after a block element (such as code block, lists, blockquote) to explicitly end it.

StackEdit extends the standard Markdown syntax by adding extra **Markdown extensions**, providing you with some nice features.

> **ProTip:** You can disable any **Markdown extension** in the **File properties** dialog.


## SmartyPants

SmartyPants converts ASCII punctuation characters into "smart" typographic punctuation HTML entities. For example:

```
|                |ASCII                          |HTML                         |
|----------------|-------------------------------|-----------------------------|
|Single backticks|`'Isn't this fun?'`            |'Isn't this fun?'            |
|Quotes          |`"Isn't this fun?"`            |"Isn't this fun?"            |
|Dashes          |`-- is en-dash, --- is em-dash`|-- is en-dash, --- is em-dash|
```

|                |ASCII                          |HTML                         |
|----------------|-------------------------------|-----------------------------|
|Single backticks|`'Isn't this fun?'`            |'Isn't this fun?'            |
|Quotes          |`"Isn't this fun?"`            |"Isn't this fun?"            |
|Dashes          |`-- is en-dash, --- is em-dash`|-- is en-dash, --- is em-dash|


## KaTeX

You can render LaTeX mathematical expressions using [KaTeX](https://khan.github.io/KaTeX/):

```
The *Gamma function* satisfying $\Gamma(n) = (n-1)!\quad\forall n\in\mathbb N$ is via the Euler integral

$$
\Gamma(z) = \int_0^\infty t^{z-1}e^{-t}dt\,.
$$
```

The *Gamma function* satisfying $\Gamma(n) = (n-1)!\quad\forall n\in\mathbb N$ is via the Euler integral

$$
\Gamma(z) = \int_0^\infty t^{z-1}e^{-t}dt\,.
$$

> You can find more information about **LaTeX** mathematical expressions [here](http://meta.math.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference).


## UML diagrams

You can render UML diagrams using [Mermaid](https://mermaidjs.github.io/). For example, this will produce a sequence diagram:

    ```mermaid
    sequenceDiagram
    Alice ->> Bob: Hello Bob, how are you?
    Bob-->>John: How about you John?
    Bob--x Alice: I am good thanks!
    Bob-x John: I am good thanks!
    Note right of John: Bob thinks a long<br/>long time, so long<br/>that the text does<br/>not fit on a row.

    Bob-->Alice: Checking with John...
    Alice->John: Yes... John, how are you?
    ```

```mermaid
sequenceDiagram
Alice ->> Bob: Hello Bob, how are you?
Bob-->>John: How about you John?
Bob--x Alice: I am good thanks!
Bob-x John: I am good thanks!
Note right of John: Bob thinks a long<br/>long time, so long<br/>that the text does<br/>not fit on a row.

Bob-->Alice: Checking with John...
Alice->John: Yes... John, how are you?
```

And this will produce a flow chart:

    ```mermaid
    graph LR
    A[Square Rect] -- Link text --> B((Circle))
    A --> C(Round Rect)
    B --> D{Rhombus}
    C --> D
    ```

```mermaid
graph LR
A[Square Rect] -- Link text --> B((Circle))
A --> C(Round Rect)
B --> D{Rhombus}
C --> D
```

[^1]: This guide references [Mastering Markdown](https://guides.github.com/features/mastering-markdown/).