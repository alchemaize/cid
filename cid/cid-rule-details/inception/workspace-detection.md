# Workspace Detection — Detailed Steps

## Purpose
Analyze the workspace to determine project type, existing state, and next steps.

## Steps

### Step 1: Check for Existing VOS State
1. Look for `.vos/` directory
2. If found, check for VOSs in `in-progress/` — these indicate resumed work
3. If found, check for VOSs in `todo/` — these indicate queued work
4. Log findings

### Step 2: Scan Workspace
1. Check for existing source code (src/, lib/, app/, etc.)
2. Check for package manifests (package.json, Cargo.toml, pyproject.toml, mix.exs, etc.)
3. Check for infrastructure code (cdk/, terraform/, cloudformation/, etc.)
4. Check for test directories (tests/, test/, __tests__/, spec/)
5. Check for CI/CD configuration (.github/workflows/, .gitlab-ci.yml, etc.)

### Step 3: Determine Project Type
- **Greenfield**: No existing source code. Proceed to Requirements Validation.
- **Brownfield**: Existing source code found. Check for reverse engineering artifacts.
  - If no RE artifacts exist → proceed to Reverse Engineering
  - If RE artifacts exist → proceed to Requirements Validation

### Step 4: Log and Proceed
1. Log the workspace detection results
2. Present findings to user:
   - Project type (greenfield/brownfield)
   - Languages and frameworks detected
   - Existing VOS state (if any)
   - Recommended next phase
3. Automatically proceed to the determined next phase
