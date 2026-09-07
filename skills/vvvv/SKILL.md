---
name: vvvv
description: Gate for all vvvv gamma skills — use ONLY when working on vvvv gamma, VL (.vl patches), vvvv C# nodes ([ProcessNode]), VL packages, SDSL/Stride shaders, Spread<T>, or running/debugging vvvv. The actual vvvv skills live in the vvvv-skills submodule and are NOT preloaded; this skill routes to the right one on demand. Skip entirely for any non-vvvv work, including plain C#/.NET tasks unrelated to vvvv.
---

# vvvv skill router

The 14 vvvv sub-skills are NOT loaded into context by default. When this skill is
active, pick the matching sub-skill(s) below and Read its SKILL.md (absolute path)
before working. Supporting reference files sit next to each SKILL.md — read those
too when the task needs depth.

Sub-skill roots live in `~/skills/vvvv-skills/skills/<name>/SKILL.md`:

| Sub-skill | Use for |
| --- | --- |
| vvvv-fundamentals | core concepts: dataflow, frame execution, pins/pads, patch vs code, ecosystem overview |
| vvvv-custom-nodes | writing C# node classes: [ProcessNode], Update(), pins, change detection, services via NodeContext |
| vvvv-channels | IChannelHub, public channels, [CanBePublished], subscriptions, reactive data flow from C# |
| vvvv-spreads | Spread\<T\>/SpreadBuilder collection code |
| vvvv-patching | patch design: regions (If/ForEach/Cache), events (Bang/Toggle/FrameDelay), anti-patterns |
| vvvv-dotnet | NuGet packages, .csproj config, ImportAsIs, System.Numerics/Stride interop, async |
| vvvv-node-libraries | VL package projects: structure, services, publishing, PRs to upstream libraries |
| vvvv-shaders | SDSL shaders for Stride: TextureFX, mixins, compute, ShaderFX |
| vvvv-fileformat | .vl XML format: generating, parsing, modifying patches programmatically |
| vvvv-editor-extensions | editor plugins (.HDE.vl), Command nodes, windows/docking, VL.Lang Session API |
| vvvv-debugging | debugger setup: VS Code launch.json, VS profiles, attaching to running vvvv |
| vvvv-testing | VL.TestFramework + NUnit tests, test patches, CI |
| vvvv-troubleshooting | diagnosing errors: red nodes, shader compile failures, missing nodes, crashes |
| vvvv-startup | launching vvvv from CLI, command-line args, filesystem paths |

Multiple sub-skills can apply (e.g. new C# node → vvvv-custom-nodes + vvvv-dotnet).
If unsure, start with vvvv-fundamentals.

Content source: https://github.com/tebjan/vvvv-skills (submodule at `~/skills/vvvv-skills`).
Update with `git submodule update --remote` in `~/skills`.
