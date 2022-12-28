# Formatting guidelines
### Naming rules

Naming rules follow [Microsoft’s C# naming guidelines](https://learn.microsoft.com/en-us/dotnet/standard/design-guidelines/naming-guidelines). Where Microsoft’s naming guidelines are unspecified (e.g. private and local variables), rules are taken from the [CoreFX C# coding guidelines](https://github.com/dotnet/runtime/blob/main/docs/coding-guidelines/coding-style.md)

Rule summary:

**Code**

- Names of classes, methods, enumerations, public fields, public properties, namespaces: `PascalCase`.
- Names of local variables, parameters: `camelCase`.
- Names of private, protected, internal and protected internal fields and properties: `_camelCase`.
- Naming convention is unaffected by modifiers such as const, static, readonly, etc.
- For casing, a “word” is anything written without internal spaces, including acronyms. For example, `MyRpc` instead of ~~`MyRPC`~~.
- Names of interfaces start with `I`, e.g. `IInterface`.
- Avoid `this.` unless absolutely necessary.
- Always specify the visibility, even if it's the default (e.g. `private string _foo` not `string _foo`). Visibility should be the first modifier (e.g. `public abstract` not `abstract public`).
- `Crtl K + F` - to format selection to code

**Files**

- Filenames and directory names are `PascalCase`, e.g. `MyFile.cs`.
- Where possible the file name should be the same as the name of the main class in the file, e.g. ~MyClass.cs~.
- In general, prefer one core class per file.

### Organization

- Modifiers occur in the following order: public protected internal private new abstract virtual override sealed static readonly extern unsafe volatile async.
- Namespace using declarations go at the top, before any namespaces. using import order is alphabetical, apart from System imports which always go first.
- Class member ordering:
  - Group class members in the following order:
    - Nested classes, enums, delegates and events.
    - Static, const and readonly fields.
    - Fields and properties.
    - Constructors and finalizers.
    - Methods.
  - Within each group, elements should be in the following order:
    - Public.
    - Internal.
    - Protected internal.
    - Protected.
    - Private.
  - Where possible, group interface implementations together.

### Whitespace rules

  - A maximum of one statement per line.
  - A maximum of one assignment per statement.
  - Indentation of 4 spaces, no tabs.
  - Column limit: 100.
  - Line break before opening curly brace.
  - Line break between closing curly brace and `else`.
  - Braces used even when optional.
  - Space after commas.
  - No space after an opening parenthesis or before a closing parenthesis.
  - No space between a unary operator and its operand. One space between the operator and each operand of all other operators.
  - Avoid spurious free spaces. For example avoid `if (someVar == 0)...`, where the dots mark the spurious free spaces. 

### Example
```
using System;                                         // `using` goes at the top, outside the namespace.

namespace MyNamespace                                 // Namespaces are PascalCase.
{                                                   
    public interface IMyInterface                     // Interfaces start with 'I'
    {                   
      public int Calculate(float value, float exp);   // Methods are PascalCase and space after comma.

    }

    public enum MyEnum                                // Enumerations are PascalCase.
    {                              
        Yes,                                          // Enumerators are PascalCase.
        No,
    }

    public class MyClass                              // Classes are PascalCase.
    {                            
        public int Foo = 0;                           // Public member variables are PascalCase.

        public bool NoCounting = false;               // Field initializers are encouraged.
        private class Results
        {
            public int NumNegativeResults = 0;
            public int NumPositiveResults = 0;
        }
        private Results _results;                     // Private member variables are _camelCase.

        public static int NumTimesCalled = 0;
        private const int _bar = 100;                 // const does not affect naming convention.

        private int[] _someTable = 
        {                    
            2, 3, 4,                                  // Container initializers use a 4 space indent.
        }

        public MyClass()
        {
            _results = new Results                    // Object initializers use a 4 space indent.
            {
                NumNegativeResults = 1,                 
                NumPositiveResults = 1,                 
            };
        }

        public int CalculateValue(int mulNumber)        
        {                                             // Line break before opening brace.
            var resultValue = Foo * mulNumber;        // Local variables are camelCase.
            NumTimesCalled++;
            Foo += _bar;

            if (!NoCounting)                          // No space after unary operator and No space after 'if' statement
            {                                         
                if (resultValue < 0)                  // Braces used even when optional and spaces around comparison operator.
                {                      
                    _results.NumNegativeResults++;
                }
                else if (resultValue > 0)             // Newline between brace and else.
                {               
                    _results.NumPositiveResults++;
                }
            }

            return resultValue;
        }

        public void ExpressionBodies()
        {
            // For simple lambdas, fit on one line if possible, no brackets or braces required.
            Func<int, int> increment = x => x + 1;

            // Closing brace aligns with first character on line that includes the opening brace.
            Func<int, int, long> difference1 = (x, y) =>
            {
                long diff = (long)x - y;
                return diff >= 0 ? diff : -diff;
            };

            // If defining after a continuation line break, indent the whole body.
            Func<int, int, long> difference2 =
                (x, y) => 
                {
                    long diff = (long)x - y;
                    return diff >= 0 ? diff : -diff;
                };

            // Inline lambda arguments also follow these rules. Prefer a leading newline before groups of arguments if they include lambdas.
            CallWithDelegate(
                (x, y) =>
                {
                    long diff = (long)x - y;
                    return diff >= 0 ? diff : -diff;
                });
        }

        void DoNothing() {}                           // Empty blocks may be concise.

        // If possible, wrap arguments by aligning newlines with the first argument.
        void AVeryLongFunctionNameThatCausesLineWrappingProblems(int longArgumentName,
                                                                 int p1, int p2) {}

        // If aligning argument lines with the first argument doesn't fit, or is difficult to read, wrap all arguments on new lines with a 4 space indent.
        void AnotherLongFunctionNameThatCausesLineWrappingProblems(
            int longArgumentName, int longArgumentName2, int longArgumentName3) {}

        void CallingLongFunctionName()
        {
            int veryLongArgumentName = 1234;
            int shortArg = 1;
            // If possible, wrap arguments by aligning newlines with the first argument.
            AnotherLongFunctionNameThatCausesLineWrappingProblems(shortArg, shortArg,
                                                                  veryLongArgumentName);
            // If aligning argument lines with the first argument doesn't fit, or is difficult to read, wrap all arguments on new lines with a 4 space indent.
            AnotherLongFunctionNameThatCausesLineWrappingProblems(
                veryLongArgumentName, veryLongArgumentName, veryLongArgumentName);
        }
    }
}
```
