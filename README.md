# Spaghetti - Lightweight Documentation Tool

Automate explaining your code to AI (and maybe humans). 
Spaghetti is a tool designed to simplify the creation of project documentation, especially useful for generating structured content for AI review. This is done by creating Markdown files with a little help from PHP.

**Version 0.42** (Work in progress)

---

**Spaghetti** is a lightweight PHP-based tool for generating Markdown documentation. Unlike full-scale documentation tools like phpDocumentor, which focus on entire projects, Spaghetti is designed for narrower, context-specific tasks.  

With Spaghetti, you can easily combine your own explanations with dynamically generated content such as:  

- Database table schemas  
- Code snippets from specific files  
- Directory structures  

It’s perfect for creating quick, topic-focused documentation that’s both human-readable and AI-friendly. Whether you’re documenting a single class, explaining a database schema, or just need to save time describing your project in detail, Spaghetti has you covered.

## Example Usage

### Create a file like **about-pet-project.spaghetti.php**:

\# *Pet Adoption Project*

*Example description: This is a simple project for managing a pet adoption database. It includes tables for storing information about pets, their sweetness level (subjective but fun!), and more.*

### Now create your main file, **index.spaghetti.php**:

*`<?= $spaghetti->import('about-pet-project.spaghetti.php') ?>`*

*The most important table in this project is `pet`. Here's its structure:*

*`<?= $spaghetti->db->showTable('pet') ?>`*

*It’s connected to the `sweetness` table, which evaluates how adorable each pet is:*

*`<?= $spaghetti->db->showTable('sweetness') ?>`*

*Each pet is represented by the `Pet` entity:*

*`<?= $spaghetti->file('src/Entity/Pet.php') ?>`*


### Finally, build the documentation using Spaghetti:  

```bash
spaghetti index.spaghetti.php > index.md
```

This will generate a Markdown file (`index.md`) with all your descriptions, database schemas, and entity code snippets combined.

### Alternative: pass it straight to the AI:
 
```bash
ollama run llama3.2 "Could you please review my project? I’m looking for feedback on the code quality, suggestions for improvements, and insights on whether the design patterns used are appropriate. Here are the details: $(spaghetti prompts/index.spaghetti.php)"
```

## Example output

# Project Documentation

## Pet Adoption Project

This is a simple project for managing a pet adoption (...)

The most important table in this project is `pet`. Here's its structure:

| Column | Type   | Description          |
|--------|--------|----------------------|
| id     | int    | Primary key          |
| name   | varchar| Name of the pet      |
| ...    | ...    | ...                  |

It’s connected to the `sweetness` table, which evaluates how adorable each pet is:

| Column | Type   | Description                      |
|--------|--------|----------------------------------|
| id     | int    | Primary key                      |
| pet_id | int    | Foreign key referencing `pet`    |
| level  | int    | Sweetness level of the pet       |
| ...    | ...    | ...                              |


Each pet is represented by the `Pet` entity:

```php
class Pet {
    private $id;
    private $name;
    // ...
}
```
**...End of example output**

## Installation

Installing Spaghetti as a regular Composer dependency is NOT supported. Spaghetti is a tool, not a library. As such, it should be installed as a standalone package, so that Spaghetti's dependencies do not interfere with your project's dependencies.

Install globally with Composer:

```bash
composer global require kpion/spaghetti
```
Make sure your global Composer binaries directory is in your system's PATH.


## Details

### Including Other Spaghetti files

Easily include and parse contents of other **spaghetti** files. 

```php
<?= $spaghetti->import('src/Entity/User.php'); ?>
```

### Including External Files (sourcce)

Easily include contents of external files to keep your documentation updated without manual copy-pasting. For example:

```php
<?= $spaghetti->file('src/Entity/User.php'); ?>
```

This includes the contents of `User.php` directly in the Markdown output.

### Database Table Structure Example

You can use `showCreateTable` to generate a Markdown-friendly representation of your table’s SQL schema:

```php
<?= $spaghetti->showCreateTable('example_table'); ?>
```

Assuming `example_table` is defined in your database, the output might look like this:

```sql
CREATE TABLE `example_table` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `name` VARCHAR(255) DEFAULT NULL,
  `created_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

## Available Functions
>
Here are some of the core functions available in **Spaghetti** for generating documentation content:

- **Other spaghetti (or just plain) file Inclusion**:
  `$spaghetti->import('introduction.spaghetti.php')` - Includes the contents of the specified **spaghetti file** (which will be parsed by **spaghetti**). Relative to the parent directory of main file.

- **File Inclusion**:
  `$spaghetti->file('src/Entity/User.php')` - Includes the contents of the specified **snippet file** (which will **not** be parsed by **spaghetti**, it will be just inserted as it is). Relative to project directory (current working directory).

- **Directory Structure**:
  `$spaghetti->dir($directory, $depth = 2, $exclude = ['.git', 'vendor'])` - Shows a tree-like structure of directories with optional depth and exclusion parameters. Relative to the project directory (current working directory)

- **SQL Table Schema**:
  `$spaghetti->db->showCreateTable($tableName)` - Shows the SQL `CREATE TABLE` statement for the specified table.

- **SQL Query Execution**:
  `$spaghetti->db->sql($query, $valueLengthLimit = 1000)` - Executes an SQL query and returns results as a Markdown table.

- **Table Description**:
  `$spaghetti->db->describeTable($tableName)` - Displays column details of a table, such as types and keys.

- **Table Indexes**:
  `$spaghetti->db->showIndexes($tableName)` - Lists all indexes of the specified table in Markdown format.

- **Complete Table Documentation**:
  `$spaghetti->db->describeFullTable($tableName)` - Combines the table structure, `CREATE TABLE` statement, and indexes in a single output for complete table documentation.

## Why the Name "Spaghetti"?

While "spaghetti code" (mixing logic and presentation) is a horrible and evil practice in 99% of cases, it's actually a perfectly reasonable approach for this project's input files (documentation source files).

Plus, I absolutely love spaghetti in all its forms (the food, of course!).


## Contributing

This project was built with collaboration and feedback from the community. Contributions, suggestions, and improvements are always welcome!
