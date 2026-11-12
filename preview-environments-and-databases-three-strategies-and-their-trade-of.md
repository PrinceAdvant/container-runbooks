# Preview environments and databases: three strategies and their trade-offs

Preview deployments are easy for stateless frontends. The hard part is the database. These are the three strategies I see most.

## 1. Shared staging database

Every preview points at one staging database. Cheap and simple. Breaks down when two branches need different schemas, because migrations from one branch affect all previews.

## 2. Database per preview, seeded

Each preview gets its own small database created from migrations plus a seed script. Schemas never collide. Costs a little more and seed data must be maintained, but it is the cleanest option for most teams.

## 3. Database per preview, cloned from a snapshot

Each preview starts from a sanitized snapshot of production. Realistic data, which helps catch performance issues. Requires a reliable anonymization job, and snapshots can be large.

## Whatever you choose

- Never point previews at production.
- Tear down the preview database when the pull request closes. Orphaned databases are the main cost leak.
- Generate credentials per preview rather than reusing one password.

The Peon docs on [deployments and preview environments](https://peon.sh/docs/deployments-and-previews) explain how previews are created and cleaned up. Written by a contributor to [Peon](https://peon.sh/), an open-source self-hosted PaaS.
