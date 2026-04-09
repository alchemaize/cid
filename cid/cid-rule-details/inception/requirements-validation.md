# Requirements Validation — Detailed Steps

## Purpose
Validate the VOS against the codebase and gather any missing requirements. Depth adapts to complexity.

## Depth Determination

**Minimal** — Use when:
- Request is simple and clear
- Single component change
- Low risk, well-understood domain

**Standard** — Use when:
- Normal complexity
- Multiple components affected
- Some ambiguity in requirements

**Comprehensive** — Use when:
- Complex, high-risk change
- New domain or architecture
- Compliance requirements apply
- Multiple user types or personas

## Steps

### Step 1: Load Context
1. Load reverse engineering artifacts (if brownfield)
2. Load the VOS document (if exists)
3. Load any enabled extension opt-in files

### Step 2: Intent Analysis
1. Parse the user's request
2. Identify the domain, resource, and action
3. Assess complexity and risk
4. Determine depth level (minimal/standard/comprehensive)

### Step 3: Extension Opt-In (if extensions loaded)
1. Present each extension's opt-in question to the user
2. Record responses
3. Load full rule files for opted-in extensions
4. Log extension configuration

### Step 4: Requirements Gathering (depth-dependent)

**Minimal:**
- Validate intent against existing code
- Confirm scope with user
- Document any constraints

**Standard:**
- Gather functional requirements
- Gather non-functional requirements
- Identify dependencies and risks
- Ask clarifying questions (5-10 max)

**Comprehensive:**
- Full functional requirements with traceability
- Detailed NFR analysis
- Risk assessment
- Dependency mapping
- Compliance requirement identification
- Extensive clarifying questions

### Step 5: VOS Validation
1. If a VOS exists, validate its acceptance contract against gathered requirements
2. If no VOS exists, the gathered requirements inform VOS creation
3. Identify gaps between the VOS and the requirements

### Step 6: Approval
Present requirements summary and wait for user approval before proceeding.
