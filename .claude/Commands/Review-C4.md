# C4 DSL Review and Fix Engineering Prompt

## Objective
Review and validate a Structurizr DSL file for C4 model compliance, fixing any syntax errors, structural issues, or validation problems to ensure the file is ready for use at https://structurizr.com/dsl.

## Input Requirements
- A `.dsl` file containing Structurizr DSL syntax
- The file should represent a complete C4 model (System Context, Containers, Components, Code)

## Validation Checklist

### 1. Top-Level Structure Validation
- [ ] Entire content is wrapped in a single `workspace {}` block
- [ ] Contains required `model {}` block
- [ ] Contains required `views {}` block
- [ ] Optional `configuration {}` and `styles {}` blocks are properly structured

### 2. Model Block Validation
- [ ] All `person` declarations are at the top level of `model {}`
- [ ] All `softwareSystem` declarations are at the top level of `model {}`
- [ ] External systems are declared at the top level of `model {}`
- [ ] No nested `softwareSystem` or `person` declarations inside other blocks

### 3. Container and Component Structure
- [ ] Containers are defined inside their corresponding `softwareSystem` block only
- [ ] Components are defined inside their corresponding `container` block only
- [ ] All identifiers are globally unique and follow naming conventions
- [ ] No containers declared directly at the top level of `model {}`

### 4. Relationship Validation
- [ ] All relationships use the `->` operator correctly
- [ ] Source and destination elements exist before relationship declaration
- [ ] No relationships between containers and their own components
- [ ] No duplicate relationships (each unique source-destination pair appears once)
- [ ] No trailing comments on relationship lines
- [ ] External interactions originate from containers, not components

### 5. Views Block Validation
- [ ] `systemContext` views reference valid software systems
- [ ] `container` views reference valid software systems
- [ ] `component` views reference valid containers
- [ ] View keys use only lowercase letters, numbers, hyphens, or underscores
- [ ] No quotes or spaces in view names
- [ ] All referenced elements exist in the model

### 6. Deployment Views (If Present)
- [ ] Environment names are valid (`development`, `live`, `production`)
- [ ] Proper deployment syntax with `deploymentNode`, `containerInstance`
- [ ] Infrastructure nodes are properly defined
- [ ] If deployment views are invalid or incomplete, remove them entirely

## Common Issues to Fix

### Structural Fixes
1. **Nested System Declarations**: Move any nested `person` or `softwareSystem` to top level
2. **Container Placement**: Ensure containers are only inside `softwareSystem` blocks
3. **Component Placement**: Ensure components are only inside `container` blocks

### Relationship Fixes
1. **Order Dependencies**: Reorder declarations so all elements exist before relationships
2. **Invalid Relationships**: Remove container-to-own-component relationships
3. **Duplicate Relationships**: Consolidate multiple relationships between same elements
4. **Comment Cleanup**: Remove trailing comments from relationship lines

### View Fixes
1. **Invalid Keys**: Convert view names to valid format (lowercase, no spaces/quotes)
2. **Missing References**: Ensure all view references point to existing model elements
3. **Deployment Issues**: Remove or fix invalid deployment views

### Syntax Fixes
1. **Missing Braces**: Ensure all blocks have matching opening/closing braces
2. **Invalid Characters**: Remove or escape invalid characters in identifiers
3. **Workspace Wrapper**: Ensure entire content is in `workspace {}` block

## Output Requirements

The corrected DSL file must:
- Be fully compliant with Structurizr DSL syntax
- Be ready to paste directly into https://structurizr.com/dsl
- Contain no HTML, markdown, or commentary
- Have no syntax errors or validation warnings
- Maintain the original architectural intent while fixing structural issues

## Validation Process

1. **Parse Structure**: Verify workspace, model, and views blocks exist
2. **Check Declarations**: Validate all elements are declared in correct locations
3. **Validate Relationships**: Ensure all relationships reference existing elements
4. **Review Views**: Confirm all view references are valid
5. **Test Syntax**: Verify the DSL would load successfully in Structurizr
6. **Clean Output**: Remove any non-DSL content or formatting

## Success Criteria

- File loads without errors in Structurizr DSL editor
- All C4 model levels are properly represented
- Views render correctly showing intended architecture
- No validation warnings or syntax errors
- Original architectural design intent is preserved2