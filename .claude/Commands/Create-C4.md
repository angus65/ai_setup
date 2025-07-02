Instruction: Generate Structurizr DSL (Full C4 Model)

You are given a source codebase. Your task is to generate a complete Structurizr DSL file that represents the full C4 model:
	•	System Context
	•	Containers
	•	Components
	•	Code

The output must be:
	•	Fully compliant with Structurizr DSL syntax
	•	Ready to paste into https://structurizr.com/dsl
	•	Free of HTML wrappers, markdown formatting, or commentary
	•	Contained in a single workspace {} block


Structurizr DSL Compliance Rules
rule: you cannot declare person or softwareSystem inside the softwareSystem(...) {} block or inside any nested block like that.

1. Top-Level Declarations
	•	All softwareSystem, person, and external system/container declarations must be placed directly inside the top-level model {} block.
	•	Do not nest a softwareSystem or container block inside another system or container.

2. Container Definitions
	•	Define containers inside their corresponding softwareSystem block.
	•	Do not declare containers directly at the top level of model {}.

3. Component Definitions
	•	Define components inside their corresponding container block.
	•	Each component must have a globally unique identifier (e.g., authRoute, tokenActions, userService) for relationship references.

4. Relationships
	•	Use the -> operator only after both source and destination elements have been declared.
	•	Do not create relationships between a container and its own components.
	•	Interactions with other systems or containers should originate from the container, not its components.
	•	Eliminate duplicate relationships; each unique source-destination pair should be connected only once.
	•	Remove all trailing comments on relationship lines to maintain DSL syntax compliance.

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

	•	The environment must be one of:
	•	development, live, production, or another valid predefined environment
	•	Do not use arbitrary quoted environment names or views with invalid keys.
	•	When unsure, omit deployment views entirely to avoid structural errors.

Output Requirements
	•	The entire result must be wrapped in a single workspace {} block.
	•	The DSL file must contain:
	•	model {} with all people, systems, containers, components, and relationships
	•	views {} for system context, containers, components, and deployment (if valid)
	•	Optional styles {} block for custom visual presentation
	•	No commentary, no markdown, no HTML—only valid Structurizr DSL.
	•	The result must be fully copy/paste ready for use in https://structurizr.com/dsl with no modifications required.
