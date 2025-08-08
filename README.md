# LikeC4 Correctness Validation

Enhanced LikeC4 with correctness validation capabilities for detecting architectural issues.

## Prerequisites

- **Node.js** 20.19.3+ 
- **pnpm** 10.11.1+

### Install Prerequisites

**Node.js:**
```bash
# Check current version
node --version

# If you need to update, download from https://nodejs.org/
# Or use nvm:
nvm install 20.19.3
nvm use 20.19.3
```

**pnpm:**
```bash
# Install pnpm globally
npm install -g pnpm
```

## How to Clone

```bash
git clone https://github.com/Nikhil-CMU/likec4
cd likec4
pnpm install
```

## Initial Setup (Required)

After cloning and installing dependencies, you must generate the required files in the correct order:

```bash
# Step 1: Generate icons first (required dependency)
cd packages/icons
pnpm generate

# Step 2: Generate Langium language server files
cd ../language-server
pnpm langium generate

# Step 3: Generate the remaining language server icons
pnpm tsx scripts/generate-icons.ts

# Step 4: Return to project root
cd ../..
```

**Note:** These steps must be run in this exact order. The language server generation depends on icons being available first.

### Quick Setup Script

For convenience, you can also run all setup steps with a single command from the project root:

```bash
# From the likec4 root directory
cd packages/icons && pnpm generate && \
cd ../language-server && pnpm langium generate && pnpm tsx scripts/generate-icons.ts && \
cd ../..
```

### Verification

To verify the setup completed successfully, check that these files exist:

```bash
# Check generated files exist
ls packages/icons/all.js
ls packages/language-server/src/generated/module.ts
ls packages/language-server/src/generated-lib/icons.ts
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
# First, regenerate files in correct order if needed
cd packages/icons
pnpm generate
cd ../language-server
pnpm langium generate
pnpm tsx scripts/generate-icons.ts

# Then build all packages (optional - CLI works without this)
cd ../..
pnpm turbo run build
```

**Note:** The build step is optional for CLI usage. If you encounter Node.js version compatibility issues with the build step, you can still use the CLI functionality by skipping `pnpm turbo run build`.

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

## Troubleshooting

### Common Issues and Solutions

1. **Error: Cannot find module '...generated/module'**
   - **Cause:** Language server files not generated or generated in wrong order
   - **Solution:** Run the setup steps in correct order, icons first then language server

2. **Error: Cannot find module '@likec4/icons/all.js'**
   - **Cause:** Icons not generated before language server generation
   - **Solution:** Run `cd packages/icons && pnpm generate` first

3. **Node.js version warnings**
   - **Cause:** Using Node.js version older than 20.19.3
   - **Impact:** CLI functionality works, but build may fail
   - **Solution:** Either upgrade Node.js or skip the `pnpm turbo run build` step

4. **Build fails with 'addAbortListener' error**
   - **Cause:** Node.js version compatibility issue
   - **Solution:** CLI works without building - skip `pnpm turbo run build`

### Quick Fix for Setup Issues

If you encounter any setup issues, try this complete reset:

```bash
# Clean and restart
rm -rf packages/icons/all.js packages/icons/all.d.ts
rm -rf packages/language-server/src/generated/*
rm -rf packages/language-server/src/generated-lib/*

# Run setup in correct order
cd packages/icons && pnpm generate
cd ../language-server && pnpm langium generate && pnpm tsx scripts/generate-icons.ts
cd ../..

# Test CLI
cd packages/likec4 && pnpm start correctness ../../correctness-example
```