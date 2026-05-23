# Review Policies

## Core object/extension runtime semantics
- **Paths**: `core/object/**`, `core/extension/**`, `core/variant/**`
- **Severity**: critical
- **Reason**: Small changes can silently break scripting bindings, ABI behavior, and object lifecycle semantics that automated checks may miss.

## API compatibility and extension surface
- **Paths**: `**/*.compat.inc`, `misc/extension_api_validation/**`, `scene/scene_string_names.h`
- **Severity**: critical
- **Reason**: Compatibility-surface edits can break downstream projects and bindings even when builds/tests pass.

## Resource format/loader/parser behavior
- **Paths**: `core/io/resource_format_binary.cpp`, `scene/resources/resource_format_text.cpp`, `core/variant/variant_parser.cpp`
- **Severity**: high
- **Reason**: Serializer/parser changes risk compatibility breakage or asset misreads that require human validation on real projects.

## Resource loading threading and lifecycle
- **Paths**: `core/io/resource_loader.*`, `main/main.*`, `editor/file_system/**`, `core/io/**`
- **Severity**: critical
- **Reason**: Threaded loading and teardown changes can introduce deadlocks, races, or shutdown hangs that are timing-sensitive.

## Archive extraction path safety
- **Paths**: `core/io/**zip**`, `core/io/**archive**`
- **Severity**: high
- **Reason**: Archive handling changes may reintroduce traversal or metadata-related security flaws not fully covered by automation.

## Rendering backend/driver/shader changes
- **Paths**: `servers/rendering/**`, `drivers/vulkan/**`, `drivers/gles3/**`, `drivers/d3d12/**`, `scene/3d/**`
- **Severity**: critical
- **Reason**: Rendering changes can cause GPU/driver-specific crashes, corruption, or performance regressions not reproducible in CI.

## Audio real-time thread behavior
- **Paths**: `drivers/pulseaudio/**`, `servers/audio/**`
- **Severity**: critical
- **Reason**: Blocking or failure-path mistakes in audio real-time threads can cause dropouts, deadlocks, or unrecoverable device errors.

## Platform runtime lifecycle and windowing
- **Paths**: `platform/android/**`, `platform/windows/**`, `platform/macos/**`, `platform/linuxbsd/**`, `platform/ios/**`, `platform/web/**`, `platform/visionos/**`, `scene/main/window.cpp`, `scene/gui/caption_button_overlay.cpp`
- **Severity**: high
- **Reason**: Platform-specific lifecycle/input/windowing changes require manual OS/device validation beyond automated checks.

## Android runtime plugin proxy invocation
- **Paths**: `platform/android/java/lib/src/main/java/org/godotengine/godot/plugin/AndroidRuntimePlugin.kt`
- **Severity**: high
- **Reason**: Proxy equals/hash/argument-forwarding changes can create recursion or null-argument crashes missed by static checks.

## OpenXR runtime integration
- **Paths**: `modules/openxr/**`, `scene/3d/xr/**`, `modules/mobile_vr/**`
- **Severity**: high
- **Reason**: Extension negotiation and session lifecycle behavior depend on real runtime/headset matrices not covered by CI.

## Editor export pipeline and platform plugins
- **Paths**: `editor/export/**`, `platform/**/export/**`
- **Severity**: high
- **Reason**: Export logic changes can silently break packaging/notifications across targets without full end-to-end coverage.

## Editor UX, live-edit, and debugger contracts
- **Paths**: `editor/**`, `editor/scene/**`, `editor/docks/**`, `editor/inspector/**`, `editor/script/**`, `scene/debugger/**`
- **Severity**: high
- **Reason**: Interaction and remote-debug/live-edit behavior requires human workflow testing and protocol-level validation.

## GUI layout and RTL behavior
- **Paths**: `scene/gui/**`
- **Severity**: medium
- **Reason**: Locale- and platform-specific layout/input edge cases require manual visual and interaction verification.

## Documentation and localization contracts
- **Paths**: `doc/classes/**`, `modules/**/doc_classes/**`, `editor/translations/**`
- **Severity**: medium
- **Reason**: Docs/localization edits can introduce contract inaccuracies or translation churn that need human editorial judgment.

## Build and CI workflow changes
- **Paths**: `.github/workflows/**`, `.github/actions/**`, `SConstruct`, `site_scons/**`, `SCsub`, `**/*.py`
- **Severity**: high
- **Reason**: Build/CI changes can alter coverage, artifacts, or release behavior in ways passing CI alone does not validate.

## Third-party vendored code updates
- **Paths**: `thirdparty/**`
- **Severity**: high
- **Reason**: Vendored updates can introduce ABI, licensing, security, or long-term maintenance divergence risks requiring human review.

## Instructions
- When a change removes, renames, or changes semantics of existing APIs/signals/properties, a human must decide whether compatibility shims, deprecation path, and migration strategy are acceptable.
- For broad documentation rewrites, a human must judge whether clarity gains justify translation invalidation and maintenance cost.
- If a PR changes failures between error, warning, or silence, a human must assess user impact, debuggability, and noise tradeoffs.
- If fallback behavior changes (defaults, retries, strictness, or feature disablement), a human must evaluate compatibility, correctness, and availability tradeoffs.
- When editor-facing labels, ordering, controls, or interaction flows change, a human must decide whether usability and consistency improve.
- When behavior differs by platform/renderer/locale/runtime capability, a human must validate assumptions with representative environments.
- When synchronization, waiting, or task-lifecycle behavior changes, a human must review for race/deadlock risk and public semantics shifts.
- If defensive checks are removed as redundant, a human must confirm misuse-resilience and crash-risk remain acceptable.
- When new core configuration knobs or convenience APIs are exposed, a human must judge long-term surface-area and maintenance coherence.
- When contributors disagree whether behavior is intentional or a regression, a human must confirm product/design intent before accepting a fix.
- If a PR combines fix logic with unrelated refactors or speculative follow-ups, a human must decide whether it should be split for safer review.
- When CI job conditions, matrices, or timeouts change, a human must decide whether runtime savings are worth any reduction in confidence.
