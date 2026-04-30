# Boolean Rule Language

## Project Overview
This repository contains a Domain-Specific Language (DSL) for expressing boolean logic and comparison operations. The Boolean Rule Language allows users to define rules and conditions in a clear and expressive way using Java.

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
3. Build the project using Maven:
   ```bash
   mvn clean install
   ```

## Quick Start

### Basic Usage
```java
import com.booleanrule.BooleanRuleParser;

public class Main {
    public static void main(String[] args) {
        BooleanRuleParser parser = new BooleanRuleParser();
        
        String rule = "A AND B OR NOT C";
        Object parsedRule = parser.parse(rule);
        System.out.println(parsedRule);
    }
}
```

### Advanced Usage
You can also define comparison expressions:
```java
import com.booleanrule.BooleanRuleParser;

public class Advanced {
    public static void main(String[] args) {
        BooleanRuleParser parser = new BooleanRuleParser();
        
        String rule = "score >= 80 AND (status == 'active' OR NOT isAdmin)";
        Object parsedRule = parser.parse(rule);
        System.out.println(parsedRule);
    }
}
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
   ```java
   String rule = "isAdmin OR (isEditor AND hasApproval)";
   ```

2. **Product Filtering**: Filter products based on multiple criteria
   ```java
   String rule = "price <= 100 AND (category == 'electronics' OR category == 'software')";
   ```

3. **Business Logic**: Encode business rules for decision-making
   ```java
   String rule = "creditScore >= 750 AND (yearsEmployed >= 2 OR hasCollateral)";
   ```

## API Reference

### `parse(String ruleString)`
Parses a rule string and returns an Abstract Syntax Tree (AST).

**Parameters:**
- `ruleString` (String): The rule to parse

**Returns:**
- (Object): An AST representing the parsed rule

**Throws:**
- (Exception): If the rule contains syntax errors

### `evaluate(Object ast, Map<String, Object> variables)`
Evaluates an AST against a set of variable values.

**Parameters:**
- `ast` (Object): The Abstract Syntax Tree
- `variables` (Map<String, Object>): Key-value pairs of variable names and their values

**Returns:**
- (boolean): The result of evaluating the rule

**Throws:**
- (Exception): If required variables are missing

## Error Handling

The language provides clear error messages for common issues:

```java
try {
    parser.parse("A AND AND B"); // Missing operand
} catch (Exception error) {
    System.err.println(error.getMessage()); // "Unexpected token: AND"
}

try {
    parser.parse("A OR"); // Incomplete expression
} catch (Exception error) {
    System.err.println(error.getMessage()); // "Unexpected end of input"
}
```

## Performance Tips

1. **Cache Parsed Rules**: Parse rules once and reuse the AST:
   ```java
   Object ast = parser.parse(rule);
   boolean result1 = evaluator.evaluate(ast, variables1);
   boolean result2 = evaluator.evaluate(ast, variables2);
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
