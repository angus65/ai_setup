Instruction: Generate Structurizr DSL (Full C4 Model)

You are given a source codebase. Your task is to generate a complete Structurizr DSL file that represents the full C4 model:
	•	System Context
	•	Containers
	•	Components
	•	Code

## Pre-Analysis Steps
Before generating the DSL, analyze the codebase structure:
1. Examine package.json, requirements.txt, or similar files to identify technologies
2. Review directory structure to understand container boundaries
3. Identify external dependencies and integrations
4. Map code modules to logical components
5. Determine deployment patterns from configuration files

## Technology-to-Container Mapping
- Frontend apps (React, Vue, Angular) → Web Application container
- Backend APIs (Express, FastAPI, Spring) → API Application container  
- Databases (PostgreSQL, MongoDB) → Database container
- Message queues (Redis, RabbitMQ) → Message Broker container
- Mobile apps → Mobile App container

The output must be:
	•	Fully compliant with Structurizr DSL syntax
	•	Ready to paste into https://structurizr.com/dsl
	•	Free of HTML wrappers, markdown formatting, or commentary
	•	Contained in a single workspace {} block


Structurizr DSL Compliance Rules
rule: you cannot declare person or softwareSystem inside another softwareSystem(...) {} block or inside any nested container/component block. Elements must be declared at the appropriate hierarchical level.

1. Top-Level Declarations
	•	All softwareSystem, person, and external system/container declarations must be placed directly inside the top-level model {} block.
	•	Do not nest a softwareSystem or container block inside another system or container.

2. Container Definitions
	•	Define containers inside their corresponding softwareSystem block.
	•	Do not declare containers directly at the top level of model {}.

3. Component Definitions
	•	Define components inside their corresponding container block.
	•	Each component must have a globally unique identifier (e.g., authRoute, tokenActions, userService) for relationship references.

## Naming Conventions
- Use camelCase for identifiers (webApp, apiService, userDatabase)
- Avoid spaces, special characters, and reserved words
- Keep names descriptive but concise
- Ensure global uniqueness across entire model

### Component Identification Patterns
- Controllers/Routes → nameController, nameRoute
- Services/Business Logic → nameService, nameManager  
- Data Access → nameRepository, nameDAO
- UI Components → nameComponent, nameView

4. Relationships
	•	Use the -> operator only after both source and destination elements have been declared.
	•	Do not create relationships between a container and its own components.
	•	Interactions with other systems or containers should originate from the container, not its components.
	•	Eliminate duplicate relationships; each unique source-destination pair should be connected only once.
	•	Remove all trailing comments on relationship lines to maintain DSL syntax compliance.

### Relationship Examples
```
// CORRECT: External system to container
user -> webApp "Uses"
webApp -> apiApp "Makes API calls to"
apiApp -> database "Reads from and writes to"

// INCORRECT: Container to its own components
webApp -> loginComponent "Contains" // WRONG

// INCORRECT: Component to external systems  
loginComponent -> apiApp "Calls" // WRONG - should be webApp -> apiApp
```

5. View Definitions

Use the following structure in your views {} block:

views {
  systemContext softwareSystemName {
    include *
  }
  container softwareSystemName {
    include *
  }
  component containerName {
    include *
  }
  deployment softwareSystemName {
    include *
  }
}

	•	Do not redefine elements in views; reference declared model elements only.
	•	All containers and components must be declared before they are referenced in views or relationships.
	•	View keys must only use lowercase letters, numbers, hyphens, or underscores (a-z, 0-9, -, _).
	•	Do not use quotes or spaces in view names.
	•	Valid: dynamic careAgent_auth_flow
	•	Invalid: dynamic careAgent "Authentication Flow"



Deployment View Rules
	•	Only define deployment views if you are also specifying valid deployment environments, container instances, and infrastructure nodes using correct syntax.
	•	Valid deployment view syntax must follow this structure:

deployment environment softwareSystemName {
  // define containerInstance, deploymentNode, infrastructureNode, etc.
}

	•	The environment must be a valid environment identifier such as:
	•	development, live, production, staging, test, or other environment names (without quotes)
	•	Do not use arbitrary quoted environment names or views with invalid keys.
	•	When unsure, omit deployment views entirely to avoid structural errors.

## Pre-Output Validation Checklist
Before generating the final DSL, verify:
- [ ] All identifiers are unique and follow naming conventions
- [ ] All relationships reference existing elements
- [ ] No circular dependencies in declarations
- [ ] All containers have at least one component
- [ ] All views reference valid model elements
- [ ] No nested person/softwareSystem declarations

## Output Requirements
	•	The entire result must be wrapped in a single workspace {} block.
	•	The DSL file must contain:
	•	model {} with all people, systems, containers, components, and relationships
	•	views {} for system context, containers, components, and deployment (if valid)
	•	Optional styles {} block for custom visual presentation
	•	No commentary, no markdown, no HTML—only valid Structurizr DSL.
	•	The result must be fully copy/paste ready for use in https://structurizr.com/dsl with no modifications required.

## Example Minimal Structure
```
workspace {
    model {
        user = person "User"
        system = softwareSystem "System Name" {
            webapp = container "Web App" {
                loginComponent = component "Login Component"
            }
        }
        user -> system "Uses"
    }
    views {
        systemContext system {
            include *
            autoLayout lr
        }
        container system {
            include *
            autoLayout lr
        }
    }
}
```
