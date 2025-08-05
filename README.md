# LikeC4 Correctness Validation

Enhanced LikeC4 with correctness validation capabilities for detecting architectural issues.

## How to Clone

```bash
git clone https://github.com/Nikhil-CMU/likec4
cd likec4
pnpm install
```

## Initial Setup (Required)

After cloning and installing dependencies, you must generate the required language server files:

```bash
# Generate Langium language server files
cd packages/language-server
pnpm generate

# Generate icons for the language server
cd ../icons
pnpm generate

# Generate the remaining language server icons
cd ../language-server
pnpm tsx scripts/generate-icons.ts
```

**Note:** These steps are required before running any LikeC4 commands. Without these generated files, you'll encounter module not found errors.

### Quick Setup Script

For convenience, you can also run all setup steps with a single command from the project root:

```bash
# From the likec4 root directory
pnpm --filter @likec4/icons generate && \
pnpm --filter @likec4/language-server generate && \
cd packages/language-server && pnpm tsx scripts/generate-icons.ts
```

## What Files Were Changed

### CLI Implementation
- `packages/likec4/src/cli/correctness/` - New correctness command and validation logic
- `packages/likec4/src/cli/index.ts` - Registered correctness command

### Language Server Integration  
- `packages/language-server/src/validation/correctness/` - Real-time validation in editors
- `packages/language-server/src/validation/index.ts` - Registered validation checks

### Test Examples
- `correctness-example/` - Test files with validation tags

## How to Rebuild

If you make changes to the source code, rebuild with:

```bash
# First, regenerate language server files if needed
cd packages/language-server
pnpm generate
cd ../icons
pnpm generate
cd ../language-server
pnpm tsx scripts/generate-icons.ts

# Then build all packages
cd ../..
pnpm turbo run build
```

## How to Test the Changes

### CLI Testing
```bash
cd packages/likec4
pnpm start correctness ../../correctness-example
```

### Language Server Testing
```bash
./reinstall-local-language-server
code correctness-example/
```