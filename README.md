# Boolean Rule Language

## Project Overview
This repository contains a Domain-Specific Language (DSL) for expressing boolean logic and comparison operations. The Boolean Rule Language allows users to define rules and conditions in a clear and expressive manner, making it suitable for various applications such as decision-making systems, data validation, and configuration management.

## Features
- **Expressive Syntax**: Supports a wide range of boolean expressions and comparison operators.
- **User-Friendly**: Designed with simplicity in mind, allowing users to easily formulate complex rules.
- **Extensible**: Users can extend the language with additional operators and functionalities as needed.
- **Error Handling**: Robust mechanisms to handle syntax errors and provide meaningful feedback.

## Installation
To install the Boolean Rule Language package, follow these steps:

1. Clone the repository:
   ```bash
   git clone https://github.com/abdullahsherdy/boolean-rule-language.git
   ```
2. Navigate to the project directory:
   ```bash
   cd boolean-rule-language
   ```
3. Install the dependencies:
   ```bash
   npm install
   ```

## Quick Start

### Basic Usage
```javascript
const { parse } = require('boolean-rule-language');

const rule = "A AND B OR NOT C";
const parsedRule = parse(rule);
console.log(parsedRule);
```

### Advanced Usage
You can also define comparison expressions:
```javascript
const rule = "score >= 80 AND (status == 'active' OR NOT isAdmin)";
const parsedRule = parse(rule);
console.log(parsedRule);
```

## Language Syntax

### Operators
- **AND**: Logical conjunction
- **OR**: Logical disjunction
- **NOT**: Logical negation
- **==**: Equality comparison
- **!=**: Inequality comparison
- **>**: Greater than
- **<**: Less than
- **>=**: Greater than or equal
- **<=**: Less than or equal

### Operator Precedence (highest to lowest)
1. Parentheses `()`
2. NOT
3. AND
4. OR

## Parsing Concepts

The Boolean Rule Language uses a **recursive descent parser** that interprets rules based on the defined grammar. The parser handles:

- **Operator Precedence**: Ensures expressions are evaluated in the correct order
- **Associativity**: Manages how operators of the same precedence are grouped
- **Parentheses**: Supports explicit grouping with parentheses to override default precedence
- **Error Recovery**: Provides meaningful error messages for syntax errors

### Grammar Overview
```
Expression     := OrExpression
OrExpression   := AndExpression (OR AndExpression)*
AndExpression  := NotExpression (AND NotExpression)*
NotExpression  := NOT PrimaryExpression | PrimaryExpression
PrimaryExpression := Comparison | Identifier | Literal | '(' Expression ')'
Comparison     := Identifier ComparisonOp (Literal | Number)
```

## Architecture

The language implementation consists of the following components:

1. **Lexer**: Tokenizes the input string into meaningful tokens
2. **Parser**: Builds an Abstract Syntax Tree (AST) using recursive descent parsing
3. **Evaluator**: Evaluates the AST against provided variable values
4. **Error Handler**: Reports syntax and runtime errors with helpful messages

### Data Flow
```
Input String → Lexer → Tokens → Parser → AST → Evaluator → Result
```

## Use Cases

1. **User Permissions**: Define complex permission rules based on user roles and attributes
   ```javascript
   "isAdmin OR (isEditor AND hasApproval)"
   ```

2. **Product Filtering**: Filter products based on multiple criteria
   ```javascript
   "price <= 100 AND (category == 'electronics' OR category == 'software')"
   ```

3. **Business Logic**: Encode business rules for decision-making
   ```javascript
   "creditScore >= 750 AND (yearsEmployed >= 2 OR hasCollateral)"
   ```

## API Reference

### `parse(ruleString)`
Parses a rule string and returns an Abstract Syntax Tree (AST).

**Parameters:**
- `ruleString` (string): The rule to parse

**Returns:**
- (object): An AST representing the parsed rule

**Throws:**
- (Error): If the rule contains syntax errors

### `evaluate(ast, variables)`
Evaluates an AST against a set of variable values.

**Parameters:**
- `ast` (object): The Abstract Syntax Tree
- `variables` (object): Key-value pairs of variable names and their values

**Returns:**
- (boolean): The result of evaluating the rule

**Throws:**
- (Error): If required variables are missing

## Error Handling

The language provides clear error messages for common issues:

```javascript
try {
  parse("A AND AND B"); // Missing operand
} catch (error) {
  console.error(error.message); // "Unexpected token: AND"
}

try {
  parse("A OR"); // Incomplete expression
} catch (error) {
  console.error(error.message); // "Unexpected end of input"
}
```

## Performance Tips

1. **Cache Parsed Rules**: Parse rules once and reuse the AST:
   ```javascript
   const ast = parse(rule);
   const result1 = evaluate(ast, variables1);
   const result2 = evaluate(ast, variables2);
   ```

2. **Precompile Rules**: For frequently used rules, consider precompiling them into optimized forms.

3. **Avoid Deep Nesting**: Keep rule expressions reasonably nested to minimize evaluation time.

## Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/YourFeature`)
3. Commit your changes (`git commit -m 'Add YourFeature'`)
4. Push to the branch (`git push origin feature/YourFeature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Contact

For questions or suggestions, please open an issue on the [GitHub repository](https://github.com/abdullahsherdy/boolean-rule-language).