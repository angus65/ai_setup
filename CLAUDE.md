# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a template repository for setting up and using Claude Code in other repositories. It serves as a centralized place to save and refine engineering prompts for future use, similar to Claude Taskmaster. This repository contains configuration templates, setup scripts, and refined prompts that can be copied to new projects.

## Purpose

- **Template Repository**: Provides boilerplate configuration for Claude Code setup
- **Prompt Library**: Collection of refined engineering prompts for various development tasks
- **Configuration Management**: Standardized Claude Code settings and permissions
- **Reusable Setup**: Easy way to bootstrap new projects with Claude Code integration

## Current Configuration

- `.gitignore`: Template for Node.js/JavaScript projects with common ignore patterns
- `.claude/settings.local.json`: Template permission settings allowing `find` command usage
- `.mcp.json`: MCP server configuration template

## Usage

When starting a new project:

1. Copy relevant configuration files from this template
2. Adapt the CLAUDE.md file for the specific project
3. Use refined prompts from this repository's prompt library
4. Customize settings based on project requirements

## Template Commands

Common commands that may be relevant in projects using this template:

```bash
# Install dependencies
npm install

# Development server
npm run dev

# Build project
npm run build

# Run tests
npm test

# Linting
npm run lint
```

## Notes for Claude Code Instances

- This is a template/configuration repository, not an active development project
- Copy and adapt configurations rather than modifying this template directly
- Refer to saved prompts and patterns when working on similar tasks in other repositories
- Update this template when discovering better practices or configurations
