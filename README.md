# Android XML Layout Validator

A Flex/Bison project that parses and validates a simplified Android XML layout language.

The project was developed for an undergraduate course in the Department of Computer Engineering and Informatics at the University of Patras. It uses Flex for lexical analysis and Bison for grammar parsing and semantic validation.

---

## Overview

This repository implements a command-line parser for Android-like XML layout files.

The program reads an input file, tokenizes it using Flex, parses it using Bison, and checks whether the file follows the supported layout rules. If the input is valid, the program prints a success message. If there is a syntax or semantic error, it reports the line where the problem was detected and prints an explanatory message.

The project should be described as a parser or validator for a subset of Android XML layouts, not as a complete Android application generator.

---

## Main Functionality

The validator supports a simplified XML structure based on common Android UI components.

Supported layout containers:

* `LinearLayout`
* `RelativeLayout`

Supported UI elements:

* `TextView`
* `ImageView`
* `Button`
* `RadioGroup`
* `RadioButton`
* `ProgressBar`

Supported Android-style attributes include:

* `android:id`
* `android:layout_width`
* `android:layout_height`
* `android:orientation`
* `android:text`
* `android:textColor`
* `android:src`
* `android:padding`
* `android:button_count`
* `android:checkedButton`
* `android:max`
* `android:progress`

---

## Validation Rules

The parser performs both syntactic and semantic checks.

### Layout Validation

A valid input file must start with either a `LinearLayout` or a `RelativeLayout`.

Both layout types must include:

* `android:layout_width`
* `android:layout_height`

`LinearLayout` can also include an `android:orientation` attribute.

The parser checks for missing required attributes and duplicate attributes.

---

### TextView Validation

A `TextView` must include:

* `android:layout_width`
* `android:layout_height`
* `android:text`

Optional attributes include:

* `android:id`
* `android:textColor`

---

### ImageView Validation

An `ImageView` must include:

* `android:layout_width`
* `android:layout_height`
* `android:src`

Optional attributes include:

* `android:id`
* `android:padding`

---

### Button Validation

A `Button` must include:

* `android:layout_width`
* `android:layout_height`
* `android:text`

Optional attributes include:

* `android:id`
* `android:padding`

---

### RadioGroup and RadioButton Validation

A `RadioGroup` must include:

* `android:layout_width`
* `android:layout_height`
* `android:button_count`

The parser also checks that:

* The value of `android:button_count` matches the number of nested `RadioButton` elements.
* The value of `android:checkedButton`, when used, matches the ID of one of the nested radio buttons.

Each `RadioButton` must include:

* `android:layout_width`
* `android:layout_height`
* `android:text`

---

### ProgressBar Validation

A `ProgressBar` must include:

* `android:layout_width`
* `android:layout_height`

The parser also checks that:

* `android:max` and `android:progress` must either both exist or both be absent.
* `android:progress` cannot be greater than `android:max`.

---

### ID Validation

The parser checks that ID values are unique across the input file.

If the same ID value is used more than once, the program reports an error.

---

## Accepted Attribute Values

For `android:layout_width` and `android:layout_height`, the accepted values are:

* A positive integer
* `"wrap_content"`
* `"match_parent"`

The project uses a simplified value format and does not fully support all Android XML value types such as `@+id/...`, `@string/...`, color resources, dimensions, namespaces, or XML headers.

---

## Example Input

```xml
<LinearLayout android:layout_width="match_parent" android:layout_height="match_parent" android:orientation="vertical">
    <TextView android:layout_width="wrap_content" android:layout_height="wrap_content" android:text="Hello" />
    <Button android:layout_width="100" android:layout_height="50" android:text="Submit" />
</LinearLayout>
```

---

## Project Structure

```text
android-xml-layout-validator/
├── myparser.l        # Flex lexical analyzer
├── myparser.y        # Bison grammar and semantic validation rules
├── lex.yy.c          # Generated C file from Flex
├── myparser.tab.c    # Generated C parser from Bison
├── myparser.tab.h    # Generated Bison header file
├── config.txt        # Sample input file
└── run.exe           # Precompiled Windows executable
```

---

## Tech Stack

| Technology | Purpose                                              |
| ---------- | ---------------------------------------------------- |
| C          | Generated parser implementation and semantic actions |
| Flex       | Lexical analysis/token generation                    |
| Bison      | Grammar parsing and syntax validation                |
| GCC        | Compilation                                          |

---

## How to Build

Install Flex, Bison, and GCC.

Then generate and compile the parser:

```bash
flex myparser.l
bison -d myparser.y
gcc lex.yy.c myparser.tab.c -o android_xml_validator
```

On Windows, you can compile to an `.exe` file:

```bash
gcc lex.yy.c myparser.tab.c -o run.exe
```

---

## How to Run

Run the validator by passing an input file as an argument:

```bash
./android_xml_validator input.xml
```

On Windows:

```bash
run.exe input.xml
```

If the file is valid, the program prints a message that the input file was correctly written.

If the file contains an error, the program prints the input and reports the line where the error was detected.

---

## Current Limitations

This project validates a simplified XML-like language and does not support the full Android XML layout specification.

Current limitations include:

* No support for XML declarations or namespace declarations
* No support for standard Android resource references such as `@+id/name`
* No support for complex values such as colors, dimensions, strings, or drawables
* Limited set of supported UI components
* Generated files and a compiled executable are included in the repository
* No automated test suite or example collection of valid and invalid inputs

---

## License

This project was developed for academic purposes.
