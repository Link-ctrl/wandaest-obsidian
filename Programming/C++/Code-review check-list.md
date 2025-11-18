_I created a checklist of quick-to-verify items when evaluating new code (e.g., adding libraries or external components) to assess its quality. While some points may also apply to internal reviews, the focus here is on new integrations. Do you think anything is missing or unnecessary?_

# C++ Code Review Checklist

This checklist might look lengthy, but the items are quick to check. It helps assess code quality—not to find bugs, but to spot potential problems. The code could still be well-written.

## 1. Code Formatting

- **Looks Good**: Is the code visually appealing and easy to read?
    
    - _Why it matters_: Can you spot that developer care about the code?
        
    - _Check_: Is formatters used this is harder but if not and the code looks nice , it is a good sign.
        
- **Broken lines**: Are there lines broken just to fit a certain length?
    
    - _Why it matters_: Broken lines can disrupt flow and readability.
        
    - _Check_: Look for lines that are broken unnecessarily, especially in comments or long strings.
        
- **Consistent Style**: Is the code uniformly formatted (e.g., indentation, bracing, line lengths)? Does it follow patterns?
    
    - _Why it matters_: Consistent formatting improves readability and signals developer care.
        
    - _Check_: Look for similar code with different styles. It's ok if code in different areas has different styles, but it should be consistent within the same area.
        
- **Indentation Levels**: Are there excessive nested blocks (deep indentation)?
    
    - _Why it matters_: Deep indentation suggests complex logic that may need refactoring.
        
    - _Check_: Flag functions with more than 4-5 levels of nesting.
        
- **Message Chains**: Are there long chains of method calls (e.g., `obj.a().b().c()`)? Message chains looks nice, but they make code harder to maintain.
    
    - _Why it matters_: Long message chains indicate tight coupling, making code harder to modify or test.
        
    - _Check_: Look for chained calls that could be simplified or broken into intermediate variables.
        
- **Debug-Friendliness**: Does the code include intentional debugging support?
    
    - _Why it matters_: Debug-friendly code simplifies troubleshooting and reduces time spent on issues. It saves a lot of time.
        
    - _Check_: Look for debuggcode, try to find out if those that wrote the code understood how to help others to manage it. For example, are there temporary variables that help to understand the code flow? Assertions that trigger for developer errors?
        

## 2. Comments

- **Clarity**: Do comments explain _why_ code exists, especially for non-obvious logic?
    
    - _Why it matters_: Comments clarify intent, aiding maintenance and onboarding.
        
    - _Check_: Verify comments are concise, relevant, and avoid stating the obvious (e.g., avoid `i++ // increment i`). Look for documentation on functions/classes.
        
- **if and for loops**: Are comments used to explain complex conditions or logic and are they easy to read? When devlopers read code conditionals are important, so comments should be used to clarify them if not obvious.
    
    - _Why it matters_: Complex conditions can be hard to understand at a glance.
        
    - _Check_: Ensure comments clarify the purpose of intricate conditions (e.g., `if (x > 0 && y < 10) // Check if x is positive and y is less than 10`).
        

## 3. Variables

- **Meaningful Names**: Are variable names descriptive and self-explanatory?
    
    - _Why it matters_: Clear names reduce guesswork and improve comprehension.
        
    - _Check_: Avoid vague names (e.g., `tmp`, `data`) and prefer domain-specific names or a combination of type and domain name (e.g., `iUserAge`, `dOrderTotal`).
        
- **Abbreviations**: Are abbreviations minimal and widely understood?
    
    - _Why it matters_: Excessive or obscure abbreviations confuse readers.
        
    - _Check_: Flag cryptic abbreviations (e.g., `usrMngr` vs. `userManager`).
        
- **Scope and Isolation**: Are variables declared close to their point of use?
    
    - _Why it matters_: Localized variables reduce mental overhead and minimize errors.
        
    - _Check_: Look for variables declared far from usage or reused across unrelated scopes.
        
- **Magic Numbers/Strings**: Are hardcoded values replaced with named constants?
    
    - _Why it matters_: Magic numbers (e.g., `42`) obscure intent and hinder maintenance.
        
    - _Check_: Ensure constants like `const int MAX_USERS = 100;` are used.
        
- **Use of** `auto`: Is `auto` used judiciously, or does it obscure variable types?
    
    - _Why it matters_: Overuse of `auto` can make debugging harder by hiding types.
        
    - _Check_: Verify `auto` is used for clear cases (e.g., iterators, lambdas) but not where type clarity is critical (e.g., `auto x = GetValue();`).
        

## 4. Bad code

- **Lots of getters and setters**: Are there many getters and setters that could be simplified?
    
    - _Why it matters_: Excessive getters/setters can indicate poor encapsulation or design and tight coupling.
        
    - _Check_: Look for classes with numerous trivial getters/setters that could be replaced with direct access or better abstractions.
        
- **Direct member access**: Are there instances where class members are accessed directly instead of through methods?
    
    - _Why it matters_: Direct access can break encapsulation and lead to maintenance issues.
        
    - _Check_: Identify cases where class members are accessed directly (e.g., `obj.member`) instead of using methods (e.g., `obj.GetMember()`).
        
- **Complex Expressions**: Are there overly complex expressions that could be simplified?
    

## 5. Templates

- **Effective Use**: Are templates used to improve code reuse without adding complexity?
    
    - _Why it matters_: Templates enhance flexibility but can reduce readability if overused or make code hard to understand.
        
    - _Check_: Review template parameters and constraints (e.g., C++20 concepts). Ensure they solve a real problem and aren’t overly generic.
        

## 6. Inheritance

- **Justification**: Is inheritance used for true “is-a” relationships, or is it overused?
    
    - _Why it matters_: Misused inheritance creates tight coupling, complicating refactoring.
        
    - _Check_: Verify inheritance follows the Liskov Substitution Principle. Prefer composition where possible. Flag deep hierarchies or concrete base classes.
        

## 7. Type Aliases (using/typedef)

- **Intuitive Names**: Are aliases clear and domain-relevant, or do they obscure meaning?
    
    - _Why it matters_: Good aliases can clarify intent; but more often confuse readers. Remember that alias are often domain-specific. And domain-specific names is not always good.
        
    - _Check_: Ensure names like `using Distance = double;` are meaningful.
        

## 8. Methods and Functions

- **Redundant naming**: Does a method name unnecessarily repeat the class name or describe its parameters? A method's identity is defined by its name and parameters—not by restating what’s already clear.
    
    - _Why it matters_: Duplicate names can lead to confusion and errors.
        
    - _Check_: Ensure method names are distinct and meaningful without duplicating class or parameter context.
        
- **Concise Names**: Are method names descriptive yet concise, avoiding verbosity?
    
    - _Why it matters_: Long names (e.g., `calculateTotalPriceAndApplyDiscounts`) suggest methods do too much.
        
    - _Check_: Ensure names reflect a single purpose (e.g., `calculateTotal`, `ApplyDiscounts`).
        
- **Single Responsibility**: Does each method perform only one task as implied by its name?
    
    - _Why it matters_: Methods doing multiple tasks are harder to test and maintain (much harder).
        
    - _Check_: Flag methods longer than 50-60 lines or with multiple logical tasks.
        
- **Parameter Count**: Are methods limited to 3-4 parameters?
    
- _Why it matters_: Too many parameters complicate method signatures and usage.
    
- _Check_: Look for methods with more than 4 parameters. Consider using structs or classes to group related parameters.
    

## 9. Error Handling

- **Explicit and Debuggable**: Are errors handled clearly?
    
    - _Why it matters_: Robust error handling prevents crashes and aids debugging.
        
    - _Check_: Verify consistent error mechanisms and proper logging of issues.
        

## 10. STL and Standard Library

- **Effective Use**: Does the code leverage STL (e.g., `std::vector`, `std::algorithm`) appropriately? Does the code merge well with the standard library?
    
    - _Why it matters_: Using STL simplifies code, becuse most C++ knows about STL. It's also well thought out.
        
    - _Check_: Look for proper use of containers, algorithms, and modern features (e.g., `std::optional`, `std::string_view`). Are stl types used like `value_type`, `iterator`, etc.?
        

## 11. File and Project Structure

- **Logical Organization**: Are files and directories grouped by module, feature, or layer?
    
    - _Why it matters_: A clear structure simplifies navigation and scalability.
        
    - _Check_: Verify meaningful file names, proper header/source separation, and use of header guards or `#pragma once`. Flag circular dependencies.
        

## 12. Codebase Navigation

- **Ease of Exploration**: Is the code easy to navigate and test?
    
    - _Why it matters_: A navigable codebase speeds up development and debugging.
        
    - _Check_: Ensure clear module boundaries, consistent naming, and testable units. Verify unit tests exist for critical functionality.