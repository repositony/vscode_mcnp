# MCNP6 syntax highlighting and snippets

**An extension providing syntax highlighting and code snippets for MCNP input decks**

Cards, keywords, and constants were taken from the [MCNPv6.3.0 user manual](https://mcnp.lanl.gov/manual.html) but is compatible with most version 6.x input files. By default this applies to files with the `.i`, `.ip`, or `.mcnp` file extension. The source is available on [github](https://github.com/repositony/vscode_mcnp).

*Note: The default theme colours are not fantastic, but are easily customised with the template [here](#customising-syntax-colours)*
  
## Syntax highlighting

All the expected syntax rules are implemented for cards, keywords, constants, and comments. Syntax highlighting rules are case insesnitive, and apply across multiple lines where appropriate, such as with the `SDEF` and `FMESH` cards seen in the example below.

![example_input_deck](/images/example_input_deck.png)

Highlighting rules are contextualised and will generally not apply to incorrect syntax or invalid states. In the following example:

- Only tallies ending 1-8 are valid, so `FC9` is will not be highlighted
- The `XYZ` geometry constant is only valid for `GEOM=`, and fails to highlight on `OUT=`
  
![fmesh_examples](/images/example_fmesh.png)

Note that while this is a useful indication of problems, it guarantees nothing for runtime.

*There are plans to implement a fully featured language server with detailed error reporting, linting, etc... but this is not a trivial task and will take time.*

## Autocomplete code snippets

Lines may be commented out with the usual `ctrl+/` shortcut, adding a `c` to the start of the selected line(s). The typical bracket matching and quotation autocomplete also included.

Code snippets are defined for input cards where appropriate. Simply start typing and hit `enter` to generate the card with default values. Tab-stops are defined for all snippets, so pressing `tab` will then cycle through keywords on the card for editing.

For example, selecting `ACT` from the suggestions,

![act_card_prompt](/images/act_card_prompt.png)

will generate the following:

![act_card_example](/images/act_card_example.png)

**For productivity, keywords and values are set to default or a simple hint where ambiguous.**

A comprehensive guide for most cards would take up the entire screen. As a compromise, every line simply has a small comment for context. This is still very useful for cards such as `PHYS`, which are just sets of values.

![Alt text](/images/physn_completed.png)

The template above is far easier than trying to remember whatever `"phys:n 100 0 0 j j j 0 -1 j j 1 0 0"` means.

Of course, card snippets are fairly subjective in their usefulness. Any feedback on what defaults, flags, or comments should be used for certain cards would be appreciated.

## Customising syntax colours

The textmate [naming conventions](https://macromates.com/manual/en/language_grammars) are used for tokenisation to support most themes. MCNP specific syntax highlighting colours are customisable by overwriting the theme in your `settings.json`.

<details>
  <summary> Click to show example settings.json template </summary>
  
```json
"editor.tokenColorCustomizations": {
"textMateRules": [
        {
            "scope": "comment.line.mcnp",
            "settings": { "foreground": "#676767" }
        },
        {
            "scope": "keyword.cellnumber.mcnp",
            "settings": { "foreground": "#82AAFF" }
        },
        {
            "scope": "constant.numeric.cellmaterial.mcnp",
            "settings": { "foreground": "#E74856" }
        },
        {
            "scope": "constant.numeric.celldensity.mcnp",
            "settings": { "foreground": "#2AB5CA" }
        },
        {
            "scope": "keyword.surfacenumber.mcnp",
            "settings": { "foreground": "#ea82ff" }
        },
        {
            "scope": "constant.language.surfacetype.mcnp",
            "settings": { "foreground": "#4ecc86" }
        },
        {
            "scope": "constant.language.zaidlib.mcnp",
            "settings": { "foreground": "#e88e96" }
        },
        {
            "scope": "keyword.mcnp",
            "settings": { "foreground": "#82AAFF" }
        },
        {
            "scope": "constant.numeric.mcnp",
            "settings": { "foreground": "#B987E1" }
        },
        {
            "scope": "variable.mcnp",
            "settings": { "foreground": "#a3d3fb" }
        },
        {
            "scope": "constant.language.mcnp",
            "settings": { "foreground": "#4ecc86" }
        },
        {
            "scope": "string.mcnp",
            "settings": { "foreground": "#ea6cc7" }  
        }
    ]
}
```

</details>

<details>
  <summary> Click to show table of scopes </summary>

Values for the MCNP syntax `"scope"` are described in the table below.

| Syntax description  | Example | Scope |
| :----------------   | :--------- | :------ |
| Comments            | **c example comment line** | comment.line.mcnp   |
| Cell number         | **99** 5 -0.26 surface numbers...| keyword.cellnumber.mcnp   |
| Cell material number | 99 **5** -0.26 surface numbers... | constant.numeric.cellmaterial.mcnp |
| Cell material density | 99 5 **-0.26** surface numbers... | constant.numeric.celldensity.mcnp  |
| Surface number      | **20** PX values... | keyword.surfacenumber.mcnp   |
| Surface type        | 20 **PX** values... | constant.language.surfacetype.mcnp   |
| ZAID suffix         | M1 08016.**32c** 1.0 | constant.language.zaidlib.mcnp   |
| Data card name      | **fmesh**34:n geom=rzt | keyword.mcnp      |
| Numeric identifiers | fmesh**34**:n geom=rzt | constant.numeric.mcnp     |
| General variable    | fmesh34:n **geom**=rzt  | variable.mcnp     |
| General constants   | fmesh34:n geom=**rzt** | constant.language.mcnp        |
| Strings             | fc34 **example tally comment** | string.mcnp       |

</details>

## Issues and bugs

It is guaranteed that there are missing cards or rules that are not working as intended, so if you find anything please [raise an issue](https://github.com/repositony/vscode_mcnp/issues).

Suggestions for improvements and features are also welcome.

<details>
  <summary> Notes for developers </summary>

The JSON files quickly spiral out of control with all the nuances of the various input cards and their edge cases.

The YAML file format is much easier to deal with, so it is suggested that you just install `js-yaml` and convert updates from the YAML versions.

```shell
    # Install js-yaml
    $ npm install js-yaml

    # Use the command-line tool to convert the yaml files to json
    $ npx js-yaml syntaxes/mcnp.tmLanguage.yaml > syntaxes/mcnp.tmLanguage.json
    $ npx js-yaml snippets/mcnp.tmSnippets.yaml > snippets/mcnp.tmSnippets.json
```

The included `.vscode/launch.json` allows for easy development. Hit `F5` to bring up a debug instance with the extension loaded, and `ctrl+R` to reload the window whenever a change is made.

</details>
