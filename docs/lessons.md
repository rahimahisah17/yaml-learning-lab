# YAML Configuration Reference

A practical reference covering YAML syntax, data structures, data types, multiline strings, anchors, aliases, multi-document streams, GitHub Actions workflows, and YAML validation.

---

## Table of Contents

1. [YAML Fundamentals](#1-yaml-fundamentals)
2. [Mappings and Nested Structures](#2-mappings-and-nested-structures)
3. [Sequences](#3-sequences)
4. [Lists of Objects](#4-lists-of-objects)
5. [YAML Data Types](#5-yaml-data-types)
6. [Comments and Quoted Strings](#6-comments-and-quoted-strings)
7. [Multiline Strings](#7-multiline-strings)
8. [Anchors, Aliases, and Merge Keys](#8-anchors-aliases-and-merge-keys)
9. [Multi-Document YAML](#9-multi-document-yaml)
10. [GitHub Actions Workflow](#10-github-actions-workflow)
11. [YAML Validation](#11-yaml-validation)
12. [Final Configuration Challenge](#12-final-configuration-challenge)
13. [Key Takeaways](#13-key-takeaways)

---

## 1. YAML Fundamentals

YAML is a human-readable data serialization format commonly used for configuration files, cloud infrastructure, CI/CD pipelines, Kubernetes resources, and automation tools.

YAML primarily represents information using **key-value pairs**.

### Example

```yaml
name: Raphael
role: Cloud Engineer
country: Nigeria
```

Each key is associated with a value:

| Key | Value |
|---|---|
| `name` | `Raphael` |
| `role` | `Cloud Engineer` |
| `country` | `Nigeria` |

The example is contained in:

```text
lesson1.yaml
```

### YAML indentation

YAML uses indentation to represent hierarchy.

Spaces should be used for indentation rather than tabs.

---

## 2. Mappings and Nested Structures

A mapping groups related properties under a common key.

### Example

```yaml
person:
  name: Raphael
  role: Cloud Engineer
```

Here, `person` contains two nested properties:

```text
person
├── name
└── role
```

Mappings can contain additional properties:

```yaml
person:
  name: Raphael
  age: 30
  country: Nigeria
```

These examples are contained in:

```text
lesson2.yaml
lesson3.yaml
```

### Why indentation matters

Indentation establishes relationships between keys.

```yaml
person:
  name: Raphael
```

In this example, `name` belongs to `person`.

Incorrect indentation can change the structure of the data or cause a YAML parsing error.

---

## 3. Sequences

A sequence represents an ordered list of values.

YAML uses a hyphen (`-`) to identify each item.

### Example

```yaml
skills:
  - Azure
  - Docker
  - Kubernetes
  - Git
```

The `skills` key contains four values.

The example is contained in:

```text
lesson4.yaml
```

Sequences are commonly used to represent collections such as:

- Skills
- Ports
- Packages
- Hosts
- Resources
- Configuration options

---

## 4. Lists of Objects

A YAML sequence can contain mappings, allowing each list item to have multiple properties.

### Example

```yaml
employees:
  - name: Raphael
    role: Cloud Engineer
  - name: Sarah
    role: DevOps Engineer
  - name: David
    role: Solution Architect
```

Each employee is an object containing a `name` and a `role`.

Conceptually:

```text
employees
├── employee
│   ├── name
│   └── role
├── employee
│   ├── name
│   └── role
└── employee
    ├── name
    └── role
```

The example is contained in:

```text
lesson5.yaml
```

This structure is useful for representing collections of structured resources in configuration files.

---

## 5. YAML Data Types

YAML supports several common data types.

### Example

```yaml
person:
  name: Raphael
  age: 30
  salary: 4500.50
  active: true
  verified: false
  phone: null
```

The example demonstrates:

| Value | Data Type |
|---|---|
| `Raphael` | String |
| `30` | Integer |
| `4500.50` | Floating-point number |
| `true` | Boolean |
| `false` | Boolean |
| `null` | Null |

YAML also supports nested mappings:

```yaml
address:
  city: Abuja
  country: Nigeria
```

The complete example is contained in:

```text
lesson6.yaml
```

---

## 6. Comments and Quoted Strings

Comments begin with the `#` character.

### Example

```yaml
# Employee Information Configuration
city: Lagos
```

Comments are ignored by YAML parsers and can be used to explain configuration.

YAML also supports quoted strings.

```yaml
message: "Welcome: Cloud Engineers!"
```

Quoting can make the intended value explicit, especially when the value contains characters that may have special meaning in YAML.

The example is contained in:

```text
lesson7.yaml
```

---

## 7. Multiline Strings

YAML provides block scalar syntax for storing multiline text.

There are two important styles:

- `|` — literal block
- `>` — folded block

### Literal block: `|`

The pipe character preserves line breaks.

```yaml
bio_literal: |
  I enjoy learning Azure.
  I build cloud solutions.
  I teach DevOps.
```

This is useful when the original line structure should be retained.

### Folded block: `>`

The greater-than character folds consecutive lines into a single paragraph-like value.

```yaml
bio_folded: >
  I enjoy learning Azure.
  I build cloud solutions.
  I teach DevOps.
```

This is useful when the text is logically one paragraph but is split across multiple lines in the YAML file for readability.

The examples are contained in:

```text
multiline.yaml
```

---

## 8. Anchors, Aliases, and Merge Keys

YAML anchors allow a configuration block to be defined once and reused.

An anchor is created using `&`.

### Defining an anchor

```yaml
defaults: &default
  country: Nigeria
  role: Student
```

The configuration can then be referenced using an alias with `*`.

### Using an alias

```yaml
student1:
  <<: *default
  name: Raphael
```

The `<<` merge key incorporates the anchored properties into the new mapping.

Another object can reuse the same configuration:

```yaml
student2:
  <<: *default
  name: Sarah
```

This avoids repeating common configuration values.

The complete example is contained in:

```text
anchors.yaml
```

### Why anchors are useful

Anchors and aliases can help:

- Reduce configuration duplication
- Reuse common settings
- Keep related configuration consistent
- Make large configuration files easier to maintain

---

## 9. Multi-Document YAML

A single YAML file can contain multiple independent YAML documents.

The document separator is:

```yaml
---
```

### Example

```yaml
student1:
  name: Raphael

---
name: Third Document Stream
```

The `---` separator marks the beginning of another YAML document.

The `anchors.yaml` file demonstrates this feature.

### Important validation consideration

Because `anchors.yaml` contains multiple documents, it must be parsed using a YAML loader that supports multiple documents.

This repository uses:

```python
yaml.safe_load_all()
```

instead of:

```python
yaml.safe_load()
```

when validating all YAML files.

---

## 10. GitHub Actions Workflow

YAML is widely used to define CI/CD workflows.

The repository includes a GitHub Actions workflow in:

```text
workflow.yaml
```

### Complete workflow

```yaml
name: Build Project

on:
  push:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: echo "Building project..."
```

### Workflow name

```yaml
name: Build Project
```

Defines the name of the workflow.

### Trigger

```yaml
on:
  push:
```

Specifies that the workflow is triggered when changes are pushed to the repository.

### Job

```yaml
jobs:
  build:
```

Defines a job named `build`.

### Runner

```yaml
runs-on: ubuntu-latest
```

Specifies the GitHub-hosted runner used to execute the job.

### Steps

```yaml
steps:
  - uses: actions/checkout@v4
  - run: echo "Building project..."
```

The first step uses the GitHub Actions checkout action to retrieve repository contents.

The second step executes a shell command.

---

## 11. YAML Validation

YAML configuration should be validated before being used by automation systems.

This repository uses Python and PyYAML to validate the YAML files.

### Validate all YAML documents

```bash
python3 -c 'import yaml, glob; [list(yaml.safe_load_all(open(f))) for f in glob.glob("*.yaml")]; print("All YAML files valid!")'
```

The command:

1. Finds all `.yaml` files.
2. Opens each file.
3. Parses YAML documents using PyYAML.
4. Supports files containing multiple documents.
5. Prints a success message if parsing completes successfully.

### Check for illegal tab characters

YAML indentation should use spaces rather than tabs.

The repository checks for tab characters with:

```bash
grep -n $'\t' *.yaml || echo 'No illegal tab characters found!'
```

If no tabs are found, the command prints:

```text
No illegal tab characters found!
```

---

## 12. Final Configuration Challenge

The `student-profile.yaml` file combines multiple YAML concepts into a single configuration.

It demonstrates:

- Mappings
- Nested mappings
- Sequences
- Strings
- Integers
- Floating-point values
- Booleans
- Multiline strings
- Folded strings
- Anchors
- Aliases
- Merge keys

### Shared configuration

```yaml
default_profile: &default
  role: Student
  country: Nigeria
  active: true
  score: 95.5
```

The `&default` anchor allows this configuration to be reused.

### Reusing the configuration

```yaml
user1:
  <<: *default
  name: Sarah

user2:
  <<: *default
  name: David
```

The merge key `<<` imports the values defined by the `default` anchor.

### Sequences

The file also contains lists:

```yaml
skills:
  - Azure
  - Docker
  - Kubernetes

hobbies:
  - Coding
  - Reading
```

### Nested mappings

The address is represented as a nested mapping:

```yaml
address:
  street: 12 Cloud Way
  city: Abuja
  country: Nigeria
```

### Multiline content

The configuration also demonstrates both multiline styles:

```yaml
bio_literal: |
  I enjoy learning Azure.
  I build cloud solutions.
  I teach DevOps.

bio_folded: >
  I enjoy learning Azure.
  I build cloud solutions.
  I teach DevOps.
```

This makes `student-profile.yaml` a consolidated example of the YAML structures and syntax covered throughout the repository.

---

## 13. Key Takeaways

This repository demonstrates practical YAML configuration patterns used across cloud and DevOps environments.

### Core YAML concepts

- Key-value pairs
- Mappings
- Nested mappings
- Sequences
- Lists of objects
- Data types
- Comments
- Quoted strings
- Multiline strings

### Advanced YAML concepts

- Anchors
- Aliases
- Merge keys
- Multi-document YAML streams

### DevOps applications

- GitHub Actions workflow configuration
- Configuration management
- YAML validation
- Automation configuration
- Structured cloud and DevOps data

The combination of these patterns provides a foundation for working with YAML-based configuration across modern cloud and DevOps tooling.