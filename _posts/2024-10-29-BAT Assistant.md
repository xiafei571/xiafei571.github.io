# Batch Script Generator

A Spring Boot web application that helps users create and manage Windows batch (.bat) scripts through a user-friendly web interface. The application uses YAML files as an intermediate format for better maintainability and version control.


## Github Repo
https://github.com/xiafei571/inHouseRelease

## Features

- Create and edit batch scripts through a web interface
- Manage script variables with descriptions
- Enable/disable individual command modules
- Control script echo settings (@echo on/off)
- Store scripts in YAML format for better maintainability
- Automatically generate .bat files from YAML configurations

## How It Works

1. Users create/edit scripts through the web interface
2. The application stores the configuration in YAML format (in `source` directory)
3. The application automatically generates corresponding .bat files (in `output` directory)

### Directory Structure

- `source/`: Contains YAML configuration files
- `output/`: Contains generated .bat files

## Prerequisites

- Java JDK 1.8 or later
- Maven 3.2+

## Getting Started

These instructions will get you a copy of the project up and running on your local machine.

### Installation

1. Clone the repository
2. Build the project:
```bash
mvn clean install
```
3. Run the application:
```bash
mvn spring-boot:run
```

The application will start running at `http://localhost:8080`.

## Usage

1. Access the web interface at `http://localhost:8080`
2. Create a new script or edit existing ones
3. Add variables and commands as needed
4. Save the form
5. Find the generated .bat file in the `output` directory

## File Format

### YAML Configuration (source/*.yml)
```yaml
name: script_name
echoOn: false
variables:
  - notes: Variable description
    key: VARIABLE_NAME
    value: variable_value
commands:
  - notes: Command description
    content: command content
    active: true
```

### Generated Batch File (output/*.bat)
```batch
@echo off

rem Variable description
set VARIABLE_NAME=variable_value

rem Command description
echo Command description
command content
```
