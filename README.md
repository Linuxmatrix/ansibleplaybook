# ansibleplaybook
playbooks








What’s Inside a YAML File
A YAML file is just structured data. It includes:

Scalars (strings, numbers, booleans)
Lists (arrays)
Maps (dictionaries)
Nested maps/lists
Multi-line strings
References (anchors and aliases)
Comments(#)
---
# Scalars (Strings, Numbers, Booleans)
Scalars are the basic data types in YAML — a single value like a string, number, or boolean.
### Strings
* Can be plain (unquoted), or quoted with single `' '` or double `" "` quotes.
* Double quotes allow escape sequences like `\n`, single quotes are literal.
```yaml
# Plain string
name: John Doe
# Single quoted (literal, no escape sequences)
greeting: 'Hello, YAML!'
# Double quoted (allows escape sequences)
message: "Hello\nWorld"
```
### Numbers
* Integers or floats are written plainly.
```yaml
age: 30
pi: 3.14159
```
### Booleans
* Use `true` / `false` (case-insensitive).
```yaml
is_active: true
is_admin: FALSE
```
---
# Lists (Arrays)
Lists represent **ordered sequences** of items.
### Block style (recommended)
```yaml
fruits:
  - Apple
  - Banana
  - Cherry
```
### Flow style (inline)
```yaml
fruits: [Apple, Banana, Cherry]
```
### List of complex items
```yaml
users:
  - name: Alice
    age: 30
  - name: Bob
    age: 25
```
# Maps (Dictionaries)
Maps represent **key-value pairs** (hash/dict).
### Simple map
```yaml
person:
  name: John
  age: 40
  city: New York
```
### Inline map (flow style)
```yaml
person: {name: John, age: 40, city: New York}
```
# Nested Maps and Lists
YAML supports arbitrary nesting of maps and lists.
```yaml
company:
  name: TechCorp
  employees:
    - name: Alice
      skills:
        - Python
        - Kubernetes
    - name: Bob
      skills:
        - Java
        - Docker
```
* Here, `employees` is a **list** of maps; each map has a name and a list of skills.
---
# Multi-line Strings
YAML offers ways to write multi-line text blocks.
### Literal block (`|`)
* Preserves **line breaks** exactly as written.
```yaml
description: |
  This is a multi-line string.
  Line breaks are preserved.
  Used for formatted text.
```
Output:
```
This is a multi-line string.
Line breaks are preserved.
Used for formatted text.
```
### Folded block (`>`)
* Converts newlines to spaces (folds lines), except blank lines become newlines.
```yaml
summary: >
  This is a folded
  multi-line string.
  Newlines are folded
  into spaces.
```
Output:

```
This is a folded multi-line string. Newlines are folded into spaces.
```
### Indentation and trailing newlines
* Indentation matters for multi-line strings.
* You can add indicators like `|+` or `|-` to **keep or strip** trailing newlines.
```yaml
text_keep: |+
  Line one
  Line two
text_strip: |-
  Line one
  Line two
```
# References (Anchors & Aliases)
YAML allows reusing or referencing parts of your document with anchors and aliases to avoid repetition.
### Define an anchor with `&` and reference with `*`
```yaml
default_settings: &defaults
  retries: 3
  timeout: 30
server1:
  <<: *defaults
  host: server1.example.com
server2:
  <<: *defaults
  host: server2.example.com
  timeout: 60   # override default timeout
```
* `&defaults` defines an anchor on the map.
* `*defaults` references (aliases) it.
* `<<:` is the **merge key** to merge the referenced map into current map.
### Anchors with lists
```yaml
common_ports: &common_ports
  - 80
  - 443
service1:
  ports: *common_ports

service2:
  ports:
    - 8080
    - *common_ports  # NOT valid! You can't mix list and alias inside another list directly
```
### Reusing strings
```yaml
hello: &greet "Hello, World!"
message1: *greet
message2: *greet
```
#  YAML Practice Cheat Sheet
| Rule                           | Example                          |
| ------------------------------ | -------------------------------- |
| Use 2 spaces only              | No tabs allowed                  |
| Quote special characters       | `"abc@123"`                      |
| Lists must be properly aligned | `- item1`                        |
| Always validate your YAML      | `yamllint`,  `ansible`           |
| Avoid duplicate keys           | Use unique keys                  |
| Start with `---` (optional)    | Especially in Ansible/Kubernetes |
| Comment with `#`               | `# this is a comment`            |

| Task                      | YAML Syntax                 |   |
| ------------------------- | --------------------------- | - |
| List                      | `- item1`                   |   |
| Key-Value                 | `name: value`               |   |
| Nested Dictionary         | `parent:\n  child: value`   |   |
| Comment                   | `# this is a comment`       |   |
| Boolean                   | `true`, `false` (lowercase) |   |
| Null value                | `key: null` or just `key:`  |   |
| Multi-line (preserve)     | `|`                         |   |
| Multi-line (folded)       | `>`                         |   |
| Include block from anchor | `<<: *anchor_name`          |   |
 YAML Types

| YAML Type         | Symbol / Syntax      | Description                   | Example                     |           |
| ----------------- | -------------------- | ----------------------------- | --------------------------- | --------- |
| Scalar            | plain / quoted       | single value (string, number) | `name: John`                |           |
| List (Array)      | `-` or `[]`          | ordered sequence              | `fruits: [a, b, c]`         |           |
| Map (Dictionary)  | `key: value` or `{}` | key-value pairs               | `person: {name: John}`      |           |
| Multi-line String | `| `or`>`            | block style for text          | see above |
| Anchor            | `&anchor`            | assign a reference            | `defaults: &defaults {...}` |           |
| Alias             | `*alias`             | reference anchor              | `<<: *defaults`             |           |
Common Gotchas & Tips

| Topic                                                | Tip / Best Practice                                               |                                             |
| ---------------------------------------------------- | ----------------------------------------------------------------- | ------------------------------------------- |
| Quoting strings                                      | Use double quotes if string has escape sequences or special chars |                                             |
| Indentation                                          | Use consistent spaces (2 or 4), no tabs                           |                                             |
| Lists of complex types                               | Indent nested maps inside lists properly                          |                                             |
| Multi-line strings                                   | Use literal (` | `) if you want to preserve newlines exactly      |
| Folded strings                                       | Use folded (`>`) for paragraph-style text                         |                                             |
| Anchors and aliases                                  | Avoid deep or circular references (can confuse parsers)           |                                             |
| Merging maps                                         | Use `<<:` merge key to combine anchors                            |                                             |
| Avoid mixing flow and block styles in same structure | For readability and consistency                                   |       

#  Quick Syntax Reference
| Syntax                   | Usage                    | Example                      |        |   |
| ------------------------ | ------------------------ | ---------------------------- | ------ | - |
| `key: value`             | Scalar key-value pair    | `name: John`                 |        |   |
| `- item`                 | List item                | `fruits: <br> - apple`       |        |   |
| `[a, b, c]`              | Inline list              | `colors: [red, green, blue]` |        |   |
| `{key: val, key2: val2}` | Inline map               | `server: {ip: 127.0.0.1}`    |        |   |
| `                        | `                        | Literal multi-line string    | `desc: | ` |
| `>`                      | Folded multi-line string | `summary: >`                 |        |   |
| `&anchor_name`           | Define anchor            | `defaults: &def {...}`       |        |   |
| `*alias_name`            | Reference alias          | `copy: *def`                 |        |   |
| `<<: *alias_name`        | Merge map from alias     | `map: <<: *def`              |        |   |
---
### Scalars (Strings, Numbers, Booleans)
| Concept       | Example                    | Practice                                                                |
| ------------- | -------------------------- | ----------------------------------------------------------------------- |
| Plain String  | `name: John`               | Write a key `city` with value `New York`                                |
| Quoted String | `greeting: "Hello\nWorld"` | Create a key `quote` with value `"Life is \"beautiful\""` (with quotes) |
| Number        | `age: 32`                  | Add `pi: 3.14`                                                          |
| Boolean       | `isActive: true`           | Add a boolean key `isAdmin` with false                                  |

---
### Lists (Arrays)
| Concept          | Example                                | Practice                                                                                    |
| ---------------- | -------------------------------------- | ------------------------------------------------------------------------------------------- |
| Block Style List | `fruits: <br> - Apple <br> - Banana`   | Create a list `colors` with `red`, `green`, `blue`                                          |
| Inline List      | `numbers: [1, 2, 3, 4]`                | Write an inline list of 3 cities                                                            |
| List of Maps     | `users: <br> - name: Bob <br> age: 25` | Write a list `pets` with two items: `{type: dog, name: Rex}`, `{type: cat, name: Whiskers}` |
---
### Maps (Dictionaries)

| Concept    | Example                                     | Practice                                           |
| ---------- | ------------------------------------------- | -------------------------------------------------- |
| Simple Map | `person: <br> name: Alice <br> age: 30`     | Write a map `database` with keys: host, port, user |
| Inline Map | `settings: {env: production, debug: false}` | Create an inline map `server` with IP and location |
---
### Nested Maps & Lists

| Concept          | Example                                                                            | Practice                                 |
|----------------  | ---------------------------------------------------------------------------------- | ---------------------------------------- |
| Nested Structure | `team: <br> - name: Dev <br> members: <br> - Alice <br> - Bob`                     | Write a map `school` with classes, each having a list of students |
| Complex Nesting  | `company: <br> employees: <br> - name: Tom <br> skills: <br> - Java <br> - Docker` | Create a list`projects` where each project has a name & team members |

---

### Multi-line Strings

| Concept                | Example                                                        | Practice                                                      |                                                               |                                                   |
| ---------------------- | -------------------------------------------------------------- | ------------------------------------------------------------- | 
| Literal Block  (` | `) | `description   | <br> This is line 1.<br> This is line 2.`           | Write a literal block `note` with 3 lines of text |
| Folded Block (`>`)     | `summary: > <br> This is a folded string.<br> New lines fold.` | Create a folded block `message` with 2 lines that should fold |        |                                                   |
| Keep trailing newline  | Use `| +` to keep newline after block     | Write a multi-line string `poem` that keeps trailing newlin                                   |
| Strip trailing newline | Use ` | -` to remove trailing newline   | Write a multi-line string `log` that strips trailing newlin                                       |

---

### Anchors & Aliases (References)

| Concept             | Example                                            | Practice                                                        |
| ------------------- | -------------------------------------------------- | --------------------------------------------------------------- |
| Define anchor (`&`) | `defaults: &def <br> replicas: 2 <br> timeout: 30` | Create an anchor `&base` with keys `host` and `port`            |
| Use alias (`*`)     | `server1: <br> <<: *def <br> host: server1.local`  | Use the anchor `*base` in `server2` and override port           |
| Merge maps (`<<:`)  | Used to merge anchor map into current map          | Write two maps `dev` and `prod` that merge from the same anchor |

---



