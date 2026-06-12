# Discussion: Linting & Linters — ESLint vs Biome, what do you use?

hosted by @TimoReckling

## Questions and group answers on Linters and Linter migrations

- do we even need linters anymore in the age of AI agents?
  - yes! it is important to establish some rule set
    - keep it lean and only add custom config/rules were really needed
  - they are even a help for the AI agents
- what linters do you use?
  - ESLint + Plugins (some with airbnb plugin)
  - OXLint
  - biome
- should we migrate from ESLint to OXLint or biome?
  - motivations: 
    - huge speed up: DevX and in CI-pipeline
    - upgrade pain on ESLint and its plugins
  - potential stumbling blocks: custom rules and plugins
  - why biome if OXLint is just as quick and even closer to ESLint?
    - good experience in neighboring teams with biome and migration from ESLint
    - biome: needs less plugins, but potentially missing certain IDE integration?
    - OXLint: does support ESLint `disable-rule` syntax and plugins 
  - biome needs re-write syntax of all the `disable-rule-next-line` and `disable-rule-file` , but requires to actually give a reason for the disable
  
## Kinds of migrations and the new ease of tools migrations with help of AI agents

- migrate rule set or start fresh?
  - depends on the set goal: minimal code changes or chance to clean up the codebase and get rid of legacy rules
     - chance to overcome Chesterton's fence
- migrate with LLM / AI agent or without?
  - without: can be a good opportunity to rethink the rule set and not just migrate it 1:1, but also to get rid of rules that are no longer relevant or useful
  - with: 
    - can be a great help to understand the current rule set and its implications, and to find the right configuration for the new linter
    - go step by step
    - AI agent helps rewrite the `disable-rule` syntax
- reap the benefits of the migration by using extra things biome has that ESLint didn't have, e.g. cognitive-complexity findings
  - → improve code base as positive side effect of migration

## Other tools migrations and further concepts: depcheck → knip, infrastructure as code

- dependency check tool to detect 
  - migration from deprecated "depcheck" to "knip" with AI agent was easy
  - knip combines well with AI agent checking its own output (next to formatting, linting, typecheck, unit tests)
- talking about custom linter rules → looking at architecture tests
- CDK. what is CDK?
  - CDK (AWS) vs Pulumi (platform agnostic) vs Terraform (platform agnostic)
  - the benefits of infrastructure as code in general
    - version control and code review for infrastructure changes
    - easier to automate and integrate with CI/CD pipelines
    - disaster recovery
    - templating and/or cloning of resources or entire clusters/platforms/sets of accounts
  - fun and downsides of CDK 
    - troubleshooting and rollbacks in light of extra layer CDK → CloudFormation YAML → AWS Resources vs Terraform → AWS Resources
  - Env Variable injection and handler reference in CDK code for AWS Lambda vs handler implementation code
    - "type contracts" within code base → misspelled Env Variable name leads to TypeScript error