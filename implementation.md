# Implementation Plan: d365-agent-odataclient-ts

This document outlines the phased implementation plan for the `d365-agent-odataclient-ts` repository. Its purpose is to generate and package a TypeScript OData V4 client library for Microsoft Dynamics 365.

## Overall Goal
To establish a reliable and automated process for generating a type-safe TypeScript OData V4 client from Dynamics 365 metadata and publishing it as a versioned npm package. This client could potentially be used by `d365-agent-mcpserver-ts` or directly by TypeScript components in `d365-agent-orchestrator` if functional and preferred.

**Current Status Note:** There are known issues with the reliability/functionality of the generated TypeScript OData client for Dynamics 365. The primary D365 access path in the current architecture uses the .NET OData client via `d365-agent-mcpserver-dotnet`. Addressing these issues is a key part of this implementation plan.

---

## Phase 1: Tooling Setup, Initial Generation & Issue Assessment

*   **Objectives:**
    *   Set up the TypeScript library project structure for OData client generation.
    *   Integrate and configure the chosen OData client generator (e.g., `odata2ts`).
    *   Perform an initial client generation against D365 metadata.
    *   Thoroughly assess and document current issues with the generated client for D365.
    *   Set up basic CI for building and testing.
*   **Tasks:**
    1.  **Project Initialization:**
        *   Initialize a TypeScript library project (e.g., using `tsup`, `tsc`, or a similar build system).
        *   Set up linting (ESLint), formatting (Prettier), and a testing framework (e.g., Jest, Vitest).
    2.  **OData Client Generator Setup:**
        *   Integrate the chosen TypeScript OData generator tool (e.g., `odata2ts` from `submodules/odata2ts` or `submodules/odata2ts-fork`, or SAP Cloud SDK OData Generator).
        *   Configure the generator to use the D365 OData $metadata file (e.g., `d365-agent/asset/d365_metadata.xml`).
    3.  **Initial Client Generation:**
        *   Run the generation process.
        *   Review the generated TypeScript code (entity interfaces, query builders, service classes).
    4.  **Issue Identification and Documentation:**
        *   Systematically test the generated client against a D365 sandbox environment for common OData operations (CRUD, queries, function/action imports).
        *   Document all identified issues, errors, or limitations specifically related to D365 compatibility (e.g., handling of D365-specific data types, authentication complexities, query limitations).
    5.  **NPM Package Configuration:**
        *   Configure `package.json` for publishing (e.g., as `@d365-agent/odataclient-ts`), clearly marking it as experimental or with caveats if issues persist.
    6.  **Basic CI Pipeline:**
        *   Set up a CI pipeline (e.g., GitHub Actions) to:
            *   Fetch/update D365 metadata.
            *   Run code generation.
            *   Build the library.
            *   Run any initial unit tests (even if they only test non-D365 specific parts initially).
*   **Testing:**
    *   Verify successful code generation.
    *   Compile the generated library.
    *   Detailed testing against D365 to identify and reproduce issues.
*   **Deliverables:**
    *   A generated TypeScript OData client.
    *   Comprehensive list of known issues and limitations when used with Dynamics 365.
    *   Basic project structure and CI pipeline.

---

## Phase 2: Issue Resolution & Usability Improvements

*   **Objectives:**
    *   Prioritize and attempt to resolve the most critical issues identified in Phase 1 that prevent effective D365 interaction.
    *   Improve the usability and reliability of the generated client for D365.
    *   Establish a process for publishing stable (or clearly marked experimental) versions.
*   **Tasks:**
    1.  **Troubleshooting & Bug Fixing:**
        *   Investigate the root causes of the identified D365 compatibility issues. This may involve debugging the generator, the generated code, or interactions with the D365 OData API.
        *   Contribute fixes to the `odata2ts` fork or core library if applicable, or implement workarounds in the generation configuration/post-processing scripts.
    2.  **D365-Specific Adaptations:**
        *   If necessary, customize the generation templates or add post-generation scripts to better handle D365-specific OData behaviors or types.
    3.  **Authentication Integration:**
        *   Provide clear examples or helper utilities for integrating D365 authentication (e.g., MSAL for TypeScript) with the generated client.
    4.  **Enhanced Testing:**
        *   Develop a comprehensive suite of integration tests specifically targeting D365 OData endpoints, covering various CRUD operations, querying, and function/action calls.
    5.  **Documentation:**
        *   Update README with usage instructions, known limitations, and D365-specific considerations.
    6.  **Publishing Strategy:**
        *   Define a versioning and publishing strategy for the npm package, clearly indicating its stability level for D365 use.
        *   Automate publishing in the CI/CD pipeline.
*   **Testing:**
    *   Rigorous integration testing against a D365 sandbox for all core OData operations.
    *   Testing of any D365-specific adaptations.
*   **Deliverables:**
    *   An improved TypeScript OData client with fewer D365 compatibility issues.
    *   Updated documentation and test suite.
    *   Regularly published npm packages.

---

## Phase 3 & 4: Long-term Maintenance & Advanced Features

*   **Objectives:**
    *   Maintain compatibility with D365 OData API updates.
    *   Explore advanced client features.
*   **Tasks:**
    1.  **Ongoing Maintenance:**
        *   Regularly update D365 metadata and regenerate the client.
        *   Address new issues as they arise.
        *   Keep generator tooling and dependencies updated.
    2.  **Advanced Querying Features:**
        *   Ensure full support for complex OData query options (`$filter`, `$expand`, `$select`, `$orderby`, etc.) as supported by D365.
    3.  **Batch Request Support:**
        *   Implement or verify support for OData batch requests if needed for performance.
    4.  **Community Contributions:**
        *   If issues are in underlying libraries (e.g., `odata2ts`), contribute fixes upstream.
*   **Testing:**
    *   Continuous integration testing with each D365 version update or metadata change.
    *   Tests for any new advanced features.

This plan prioritizes addressing the current limitations of the TypeScript OData client for Dynamics 365, with the long-term goal of providing a viable alternative to the .NET OData client for TypeScript-based components in the D365 AI Agent ecosystem.
