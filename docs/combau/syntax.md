# Syntax

## Rappi# Syntax

``` ebnf
rappi_sharp = {class_declaration}.

class_declaration = 
    "class" class_identifier [":" class_identifier] "{" {field_declaration} "}".

// not specified exactly, are numbers allowed?
class_identifier = uppercase_letter {letter}.

uppercase_letter = "A" | .. | "Z".

lowercase_letter = "a" | .. | "z".

letter = uppercase_letter | lowercase_letter.

field_declaration = variable_declaration | method_declaration.

variable_declaration = type variable_identifier ";".

type = // TODO: can be user defined or default type

// not specified exactly
variable_identifier = lowercase_letter {letter}.

method_declaration = 
    type method_identifier "(" [parameter_list] ")" "{" {statement} "}".

// TODO separate to class identifier?
method_identifer = class_identifier.

parameter_list = parameter {"," parameter}.

parameter = type variable_identifier.

statement = 
      ";"
    | variable_declaration
    | variable_assignment
    | if_statement
    | while_statement
    | method_call
    | return_statement.

variable_assignment = variable_identifier "=" expression ";".

if_statement = 
    "if" "(" expression ")" "{" {statement} "}" {"else" "{" {statement} "}"}.

while_statement = 
    "while" "(" expression ")" "{" {statement} "}".

method_call = 
    [designator "."] method_identifier "(" [argument_list] ")" ";".

return_statement = "return" [expression] ";".

argument_list = expression {"," expression}.

expression =
      constant_value
    | comparison_expression
    | logical_expression

// TODO: Extend expression definition

```