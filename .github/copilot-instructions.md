# VS Code Code Runner - AI Development Guide

This guide helps AI agents understand key aspects of the Code Runner extension codebase.

## Architecture Overview

The extension follows a simple but effective architecture:

- `extension.ts`: Entry point that registers VS Code commands and initializes the CodeManager
- `codeManager.ts`: Core class managing code execution, terminal handling, and output
- `utility.ts`: Helper functions for file operations and command generation
- `constants.ts`: Shared configuration constants and executor mappings
- `appInsightsClient.ts`: Telemetry integration

### Key Components

1. **CodeManager**: Central orchestrator that:
   - Manages code execution state and terminal instances
   - Handles file/selection execution via `run()` method
   - Supports custom commands and language-specific execution
   - Controls output channel for execution results

2. **Executor System**:
   - Configurable language-to-command mapping in `package.json`
   - Supports placeholders like `$dir`, `$fileName`, `$workspaceRoot`
   - Glob pattern matching for filename-based execution rules

## Development Workflow

1. **Build Process**:
   ```powershell
   npm run compile  # Build TypeScript files
   ```

2. **Testing**:
   - Tests are in `test/extension.test.ts`
   - Run via `npm test` command
   - Tests focus on command generation and executor mapping

## Project Conventions

1. **Code Execution Patterns**:
   - Always use `CodeManager.run()` for code execution
   - Handle terminal cleanup in `onDidCloseTerminal`
   - Check `_isRunning` flag before execution

2. **Configuration Handling**:
   - Executor mappings in `package.json` under `contributes.configuration`
   - User settings extend/override default executors
   - Support both global and workspace-specific configs

3. **Error Handling**:
   - Execution errors captured in output channel
   - Terminal errors handled through VS Code's terminal API
   - User feedback via `vscode.window.showInformationMessage`

## Integration Points

1. **VS Code API**:
   - Commands: `code-runner.run`, `code-runner.stop`, `code-runner.runCustomCommand`
   - Terminal API for process execution
   - Output Channel for execution results

2. **External Dependencies**:
   - `micromatch` for glob pattern matching
   - Language executors (configured by users)

## Common Tasks

1. **Adding Language Support**:
   - Update `executorMap` in `package.json`
   - Add file extension mapping
   - Consider platform-specific commands

2. **Modifying Execution Behavior**:
   - Edit `CodeManager.run()` in `codeManager.ts`
   - Update command generation in `utility.ts`
   - Consider terminal vs output channel display

3. **Debugging**:
   - Use VS Code's extension development host
   - Check output in "Code" output channel
   - Monitor terminal state via `_isRunning` flag