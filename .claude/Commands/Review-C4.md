# C4 DSL Review and Fix Engineering Prompt

## Usage
```
Review and fix the Structurizr DSL file: {FILE_PATH}
```

## Objective
Review and validate the specified Structurizr DSL file for C4 model compliance, fixing any syntax errors, structural issues, or validation problems to ensure the file is ready for use at https://structurizr.com/dsl.

## Process
1. **Read the file**: Use the Read tool to examine the contents of {FILE_PATH}
2. **Validate structure**: Check against all validation criteria below
3. **Apply fixes**: Make necessary corrections using Edit/MultiEdit tools
4. **Verify output**: Ensure the corrected DSL is syntactically valid

## Input Requirements
- A `.dsl` file path containing Structurizr DSL syntax
- The file should represent a complete C4 model (System Context, Containers, Components, Code)

## Automated Pre-Validation
Before manual review, run these regex checks:
- Workspace wrapper: `^workspace\s*\{.*\}$` (single line mode)
- Balanced braces: Count `{` and `}` - must be equal
- Valid identifiers: `^[a-zA-Z][a-zA-Z0-9_]*$`
- No trailing comments on relationships: `.*->\s*[^"]*"[^"]*"\s*$`

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

## Fix Strategy Priority Order
Apply fixes in this order to avoid cascading issues:

### Phase 1: Structural Foundation
1. **Workspace Wrapper**: Ensure entire content is in `workspace {}` block
2. **Missing Braces**: Fix all unmatched opening/closing braces
3. **Declaration Order**: Move all `person` and `softwareSystem` to model top-level

### Phase 2: Element Placement  
4. **Container Placement**: Move containers inside `softwareSystem` blocks only
5. **Component Placement**: Move components inside `container` blocks only
6. **Identifier Cleanup**: Fix invalid characters, ensure uniqueness

### Phase 3: Relationships
7. **Declaration Dependencies**: Ensure all referenced elements exist first
8. **Invalid Relationships**: Remove container-to-own-component relationships  
9. **Duplicate Relationships**: Consolidate multiple relationships between same elements
10. **Comment Cleanup**: Remove trailing comments from relationship lines

### Phase 4: Views and Final
11. **View Key Fixes**: Convert to valid format (lowercase, no spaces/quotes)
12. **View References**: Ensure all references point to existing model elements
13. **Deployment Issues**: Remove or fix invalid deployment views

## Error Recovery Strategies
If automated fixes fail:
- **Backup Strategy**: Save original before modifications
- **Incremental Fix**: Apply one fix category at a time  
- **Rollback Points**: Test after each phase
- **Manual Fallback**: Identify sections requiring human intervention

## Implementation Steps

1. **Initial Analysis**:
   ```
   Read {FILE_PATH} and identify:
   - Current structure and syntax issues
   - Missing required elements
   - Validation rule violations
   ```

2. **Apply Fixes** (in priority order):
   - Use `MultiEdit` for multiple related changes
   - Use `Edit` for single targeted fixes
   - Follow the Phase 1-4 priority order listed above

3. **Final Validation**:
   - Ensure all checklist items are satisfied
   - Verify the file is ready for https://structurizr.com/dsl

## Output Requirements

The corrected DSL file must:
- Be fully compliant with Structurizr DSL syntax
- Be ready to paste directly into https://structurizr.com/dsl
- Contain no HTML, markdown, or commentary
- Have no syntax errors or validation warnings
- Maintain the original architectural intent while fixing structural issues

## Validation Process

1. **Read File**: Use `Read` tool to examine {FILE_PATH} contents
2. **Parse Structure**: Verify workspace, model, and views blocks exist
3. **Check Declarations**: Validate all elements are declared in correct locations
4. **Validate Relationships**: Ensure all relationships reference existing elements
5. **Review Views**: Confirm all view references are valid
6. **Apply Fixes**: Use `Edit` or `MultiEdit` tools to correct issues following the priority order
7. **Test Syntax**: Verify the DSL would load successfully in Structurizr
8. **Clean Output**: Remove any non-DSL content or formatting

## Testing and Verification Steps

### Syntax Validation
1. Copy corrected DSL to https://structurizr.com/dsl
2. Verify no error messages appear in editor
3. Check all views render without warnings
4. Validate export functionality works

### Content Verification  
1. Confirm all intended architectural elements are present
2. Verify relationships match expected system interactions
3. Check that component groupings make logical sense
4. Ensure external systems are properly represented

### Common Error Messages to Watch For
- "Element not found" → Missing declaration
- "Circular reference" → Invalid relationship structure  
- "Invalid view key" → View naming issues
- "Syntax error" → Malformed DSL syntax

## Success Criteria

- File loads without errors in Structurizr DSL editor
- All C4 model levels are properly represented
- Views render correctly showing intended architecture
- No validation warnings or syntax errors
- Original architectural design intent is preserved