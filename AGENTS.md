# AGENTS.md

## CI/CD: no JVM / nbb (JUN-290, removed in JUN-717)

Workspace rule (owner, 2026-10-22, JUN-290): JVM and nbb are not used, so CI/CD
must not start setup-java, setup-clojure, clojure, nbb, babashka or GraalVM.
Such workflows are deleted, not disabled or kept as a fallback. The canonical
rule is the "CI/CD（CDCI）に JVM と nbb を置かない" section of the workspace
`AGENTS.md` (canonical source: `com-junkawasaki/root/AGENTS.md`, "CDCI 停止中").

- **Removed:** `.github/workflows/ci.yml` (job `test`, matrix JDK 17 / 21:
  setup-java + setup-clojure + `~/.m2`/`~/.gitlibs` cache, then
  `kbb -M:test` and `kbb -M:lint`). It was the only workflow in this
  repository.
- **Native gate (Amu selfhost):** none in CI. `package.json` has local
  `amu compile` scripts (`test:kotobase`, `test:genko-query`), but they are
  not a CI gate and have no recorded negative control; the native gate is
  recorded as blocked.
- **No longer covered in CI:** the `test/` suite via cognitect test-runner
  (`:test`, including the page->storyboard -> analyzeExpression bridge test)
  and clj-kondo lint of `src` and `test` (`:lint`, errors fail), on both
  JDK 17 and 21.
