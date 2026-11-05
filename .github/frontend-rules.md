# Frontend Engineering Rules

These rules define the frontend coding standards used when reviewing Merge Requests.

The reviewer should consider these rules together with the existing patterns in the codebase.

---

# React

- Use functional components instead of class components.
- Prefer early returns over deeply nested `if` statements when doing so improves readability.
- Use function declarations for components, utility functions, and standalone functions.
- Use `export default` when a module contains only one primary export.
- Avoid argument destructuring.
- Avoid multiline ternary expressions.

---

# Naming

- Component files should be named after the component using the same letter casing.
- Files containing a single function should be named after the function using the same letter casing.
- By default, folders and files should use lowercase kebab-case.
- CSS IDs should use lowercase kebab-case.
- CSS class names should use lowercase kebab-case.
- Functions should use camelCase.
- Variables should use camelCase.
- Use meaningful and readable names for functions, variables, and components.
- Short variable names are acceptable for simple, single-line arrow functions where the meaning is obvious.

---

# JavaScript / TypeScript

- Every variable declaration inside a function or component should end with a semicolon.
- Avoid non-null assertions (`!`) unless there is a clear and justified reason.
- Prefer strict inequality (`!==`) over loose inequality (`!=`).
- Use `&&` for logical AND.
- Avoid unnecessary code duplication.
- Follow DRY principles when abstraction improves maintainability.
- Prefer established patterns already present in the codebase over reinventing functionality.
- Preserve backward compatibility unless the change intentionally introduces a breaking change.

---

# Documentation

Add JSDoc for:

- New services
- New endpoints
- Utility functions
- Complex functionality

Documentation should explain non-obvious behavior rather than adding unnecessary comments to straightforward code.

---

# JSX

- Use double quotes for JSX prop values.

---

# Rule Enforcement

Not every rule should automatically generate a review finding.

## Strong conventions

Normally report violations when they materially affect consistency, maintainability, correctness, or project conventions.

Examples:

- Component/file naming consistency
- File naming conventions
- Function and variable naming conventions
- Avoiding non-null assertions
- Avoiding unnecessary duplication
- Following established codebase patterns
- Preserving backward compatibility

## Preferred conventions

Treat these as preferences rather than absolute requirements:

- Early returns instead of nested conditionals
- Function declarations
- `export default`
- Short variable names for simple arrow functions
- Avoiding multiline ternaries

Only report preferred-rule violations when the implementation meaningfully affects readability, maintainability, or consistency.

Do not generate findings solely because of personal stylistic preference.