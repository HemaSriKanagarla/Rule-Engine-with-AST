

# Rule Engine with AST

## Overview

This is a simple 3-tier rule engine application to determine user eligibility based on attributes such as age, department, income, and spend. The rule engine uses an Abstract Syntax Tree (AST) to represent and evaluate conditional rules. It allows for the dynamic creation, combination, and modification of these rules.

## Features

- **AST Representation**: Rules are parsed into an AST structure for efficient evaluation.
- **Dynamic Rule Creation**: Users can create, modify, and combine rules dynamically.
- **Rule Evaluation**: Evaluate rules against user data.
- **API Support**: API endpoints for rule creation, combination, and evaluation.
- **Error Handling**: Graceful handling of invalid rule strings and data formats.
- **Extensibility**: Support for additional functionalities such as custom validations and user-defined functions.

## Data Structure

The AST is represented using a tree of `Node` objects. Each node can represent an operator (`AND`, `OR`) or an operand (comparison conditions). Below is a simplified representation of the `Node` structure:

```python
class Node:
    def __init__(self, type, left=None, right=None, value=None):
        self.type = type  # "operator" or "operand"
        self.left = left  # Left child node
        self.right = right  # Right child node
        self.value = value  # Operand value (e.g., number, string for conditions)
```
## Database

The database stores rules and metadata for the rule engine. You can choose a relational database such as MySQL, PostgreSQL, or a NoSQL solution like MongoDB. Below is a sample relational schema for the database.

## Sample Schema:
- **rules_table**:
  - rule_id (Primary Key, int)
  - rule_string (Text) - The original rule string
  - ast_json (Text) - JSON representation of the AST
- **metadata_table**:

  - metadata_id (Primary Key, int)
  - attribute_name (Varchar) - e.g., age, department
  - attribute_type (Varchar) - e.g., int, string
  - valid_values (Text) - Valid values for attributes
    
## API Design
1. ### create_rule(rule_string)
This API parses the rule string into an AST representation. It returns the root node of the AST.
  ```python
  def create_rule(rule_string):
      # Parse rule_string and create an AST
      root = parse_to_ast(rule_string)
      return root
  ```
2. ### combine_rules(rules)
This API takes a list of rule strings, parses them into ASTs, and combines them into a single AST. It should optimize the combination by minimizing redundant checks.
  ```python
  def combine_rules(rules):
      asts = [create_rule(rule) for rule in rules]
      combined_ast = combine_asts(asts)  # Implement AST combination logic
      return combined_ast
  ```
3. ### evaluate_rule(ast_json, data)
This API takes a JSON representing the combined AST and a dictionary of user data. It evaluates the rule against the provided data and returns True or False.
  ```python
  def evaluate_rule(ast_json, data):
      ast = json_to_ast(ast_json)
      result = evaluate_ast(ast, data)  # Implement AST evaluation logic
      return result
  ```

## Test Cases
1. ### Create Individual Rules
Use the create_rule function to parse the example rules and verify their AST structure.
  ```python
  rule1 = create_rule("((age > 30 AND department = 'Sales') OR (age < 25 AND department = 'Marketing')) AND (salary > 50000 OR experience > 5)")
  rule2 = create_rule("((age > 30 AND department = 'Marketing')) AND (salary > 20000 OR experience > 5)")
  ```
2. ### Combine Rules
Combine the parsed rules using the combine_rules function.
  ```python
  combined_rule = combine_rules([rule1, rule2])
  ```
3. ### Evaluate Rule
Test the evaluation of a rule against sample JSON data.
  ```python
  data = {
      "age": 35,
      "department": "Sales",
      "salary": 60000,
      "experience": 3
  }
  result = evaluate_rule(ast_json, data)
  ```
4. ### Error Handling and Validations
Ensure the system handles invalid rule strings or data formats gracefully.

5. ### Extensibility
Modify existing rules or add new sub-expressions dynamically using additional functionalities in the create_rule function.

## License
This project is licensed under the MIT License.
