# Engineering notes

Repository review snapshot: **6 October 2026**. These notes describe inspected implementations and observed checks; they are not production-readiness or performance certifications.

## EarthCore — domain-heavy Java

A Java 21/Paper plugin with implemented services for economy, claims, clans, jobs, skills and integrations. The public tree contains **161 Java source files, including its test source**.

Start with [EconomyService](https://github.com/sbrunomello/EarthCore/blob/main/src/main/java/mello/economy/EconomyService.java), [EconomyRepository](https://github.com/sbrunomello/EarthCore/blob/main/src/main/java/mello/economy/EconomyRepository.java) and [SkillCurveTest](https://github.com/sbrunomello/EarthCore/blob/main/src/test/java/mello/skills/SkillCurveTest.java).

The current economy repository persists to **YAML**. Tests cover the skill progression curve; economy atomicity, persistence failures and broader server integration need stronger regression coverage. No Java build or physical/server deployment was executed in this review.

## trader-assistant — research and execution boundaries

The implementation separates data, features, labels, model registration, temporal validation, backtesting, risk and paper execution.

Inspect [walk-forward splits](https://github.com/sbrunomello/trader-assistant/blob/main/core/backtest/walk_forward.py), [OOS isolation test](https://github.com/sbrunomello/trader-assistant/blob/main/tests/test_backtest_oos.py) and [paper runner](https://github.com/sbrunomello/trader-assistant/blob/main/core/execution/runner.py).

**Observed locally: 16 tests passed.** This validates tested software behavior, not a profitable trading strategy. Dependency pinning, CI and operational validation remain useful next steps.

## fala-comigo — a focused, local-first product

A browser communication aid using native web APIs, IndexedDB, speech synthesis and a service worker. The browser experience does not require an application backend; Android packaging uses Capacitor.

Inspect [core modules](https://github.com/sbrunomello/fala-comigo/tree/main/src/core), [service worker](https://github.com/sbrunomello/fala-comigo/blob/main/service-worker.js), [accessibility decisions](https://github.com/sbrunomello/fala-comigo/blob/main/docs/ACCESSIBILITY.md) and [privacy model](https://github.com/sbrunomello/fala-comigo/blob/main/docs/PRIVACY.md).

**Observed locally: 8 tests passed, 14 JavaScript files passed syntax checking, and the static build completed.** [Public CI](https://github.com/sbrunomello/fala-comigo/actions/runs/31902085240) also succeeded. Browser accessibility, offline upgrades and device-specific speech behavior require separate end-to-end checks.

## urubu-vegas — server-authoritative interactive systems

A Reddit arcade parody using **virtual credits only**. Server code validates requests and processes results; the client presents them. Pure game logic accepts an injected RNG for deterministic tests.

Inspect [roundService](https://github.com/sbrunomello/urubu-vegas/blob/main/src/server/urubuVegas/roundService.ts), [transactional API routes](https://github.com/sbrunomello/urubu-vegas/blob/main/src/server/routes/api.ts) and [replay tests](https://github.com/sbrunomello/urubu-vegas/blob/main/src/server/urubuVegas/roundService.test.ts).

**Observed locally: 22 unit tests passed.** [Latest located push CI for main](https://github.com/sbrunomello/urubu-vegas/actions/runs/33106734181) succeeded. Real Redis contention and Reddit playtest behavior were not exercised locally in this review.

## pce-python-engine — experimental persistent runtime

A Python monorepo with plugin contracts, persistent state, a robotics twin, approval transitions and separate rover, trader and LLM assistant integrations.

Inspect [PluginRegistry](https://github.com/sbrunomello/pce-python-engine/blob/main/pce-core/src/pce/core/plugins.py), [state manager](https://github.com/sbrunomello/pce-python-engine/blob/main/pce-core/src/pce/sm/manager.py), [approval policy](https://github.com/sbrunomello/pce-python-engine/blob/main/pce-os/src/pce_os/policy.py) and [architecture decisions](https://github.com/sbrunomello/pce-python-engine/tree/main/docs/adrs).

**Observed locally: 73 tests passed.** CI and lint consolidation remain pending; the current workflow also omits the pce-os and trader test suites. This project is presented as experimental.

## NeuroMesh — core/edge contracts

An experimental distributed robotics foundation: the edge owns perception, state and actuator execution; a desktop core consumes events and sends bounded commands.

Inspect [actuator normalization](https://github.com/sbrunomello/NeuroMesh/blob/main/neuromesh/apps/neuromesh-edge/app/modules/motion/actuator_service.py), [command ingress](https://github.com/sbrunomello/NeuroMesh/blob/main/neuromesh/apps/neuromesh-edge/app/api/routes.py) and [core/edge tests](https://github.com/sbrunomello/NeuroMesh/blob/main/neuromesh/tests/test_edge_core_integration.py).

**Observed locally: 12 tests passed**, including a local edge HTTP process and core client flow. Real camera, wiring, PCA9685 and physical actuator behavior were not verified here. The design currently uses motion detection and a simple planner, not a complete multi-node mesh or general-purpose autonomy.

## Review environment

Local checks used Linux, Python 3.12 and Node.js 24. Dependencies were installed in an isolated audit environment; tests using simulated or mocked integrations do not validate physical hardware, live trading, or third-party production runtimes. Successful local tests and GitHub workflow results are reported separately.
