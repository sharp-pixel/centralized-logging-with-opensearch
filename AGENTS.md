# Repository Guidelines

## Project Structure & Module Organization

This AWS solution has two main applications under `source/`. `source/constructs/` contains the CDK infrastructure: entry points in `bin/`, stacks and constructs in `lib/`, GraphQL schemas and VTL templates in `graphql/`, Python Lambda packages in `lambda/`, and CDK tests in `test/`. `source/portal/` is the React/TypeScript UI; application code lives in `src/`, static files in `public/`, and test files are generally colocated in `test/` directories. Deployment scripts, container images, and distribution configuration live in `deployment/`. Architecture and contributor documentation remain at the repository root.

## Build, Test, and Development Commands

- `cd source/constructs && npm ci && npm run build` installs locked dependencies and type-checks/compiles the CDK project.
- `cd source/constructs && npm test` runs Jest CDK tests with coverage; `npm run eslint` fixes lint findings.
- `cd source/portal && npm ci && npm start` starts the development UI using `.env.development`.
- `cd source/portal && npm run build` creates the production UI; `npm run lint` and `npm run test:coverage` validate it.
- `cd deployment && ./run-unit-tests.sh` runs the full JavaScript and Python test suite and gathers coverage. It requires uv.
- `cd deployment && ./build-s3-dist.sh <bucket> <solution> <version>` creates deployable artifacts; see `CUSTOM_BUILD.md` for required environment variables.

Use Node.js 18+, Python 3.12, uv, and Docker.

## Coding Style & Naming Conventions

TypeScript is compiled in strict mode. Use two-space indentation, `PascalCase` for React components/classes, `camelCase` for functions and variables, and kebab-case filenames such as `api-stack.ts`. Run the package ESLint command and follow the quote/style conventions of neighboring files. Python uses four spaces, `snake_case` functions/modules, and `PascalCase` classes. Preserve the existing AWS copyright and SPDX headers.

## Testing Guidelines

Jest/Testing Library cover TypeScript and React; pytest with `pytest-cov` covers Python. Name CDK tests `*.test.ts`, React tests `*.test.tsx`, and Python tests `test_*.py`. Add focused tests beside the changed component. No numeric coverage threshold is declared, so avoid reducing relevant coverage.

## Commit & Pull Request Guidelines

History favors short, imperative summaries (`Update README...`, `chore: add...`) and release messages (`Release v2.4.12`). Keep commits focused and avoid unrelated reformatting. Base work on `main`; discuss significant changes in an issue first. PRs must identify an issue when available, explain the change, pass local tests and CI, and complete `.github/PULL_REQUEST_TEMPLATE.md`. Include screenshots for visible portal changes. Report vulnerabilities through AWS Security as described in `SECURITY.md`, never in a public issue.
