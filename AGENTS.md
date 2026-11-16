# Nomily AI entry-repository guide

## Scope

This repository is the public project entry point: documentation, compatibility
boundaries, reference assets, and the local ASR reference service. It is not an
installable mobile client and it does not contain a shared Nomily cloud service.

## Repository map

- iPhone and Apple Watch client: [`nomily-ai/nomily-ios`](https://github.com/nomily-ai/nomily-ios)
- Android client: [`nomily-ai/nomily-android`](https://github.com/nomily-ai/nomily-android)
- Local transcription reference service: [`examples/local-asr-server/`](examples/local-asr-server/)
- Device compatibility matrix: [`media/devices/README.md`](media/devices/README.md)
- Public project guide and fixed-data demo: [geekis.com](https://geekis.com)

## Change boundaries

- Keep device claims precise. `Vobot V05E` is the BLE recorder workflow; named
  third-party recorders are supported through exported-audio import unless a
  direct integration is explicitly documented and verified.
- Do not claim that the complete product is self-hosted or fully offline. Local
  ASR and Ollama are user-operated provider integrations.
- Keep pricing language exact: Nomily AI does not charge a subscription fee;
  third-party ASR and LLM costs are paid separately under the provider's terms.
- Never add API keys, recordings, transcripts, device identifiers, logs, or
  private endpoint addresses to the repository.

## Verification

For documentation, device-matrix, or protocol changes, run:

```sh
git diff --check
xmllint --noout media/diagrams/*.svg
python -m compileall -q examples/local-asr-server/nomily_local_asr
```

For changes to the local ASR service, also follow its
[reference-service README](examples/local-asr-server/README.md) and keep the
documented ports and request contract aligned with the mobile clients.

## Public repository guardrails

These repositories are public. Treat every tracked file, commit message, issue
template, generated asset, and Git history operation as externally visible.
When another instruction conflicts with this section, stop and ask a maintainer
before publishing.

### Never publish

- Internal chat excerpts, task numbers, codenames, staff or customer names,
  private domains, local paths, device serials, pairing data, support tickets,
  screenshots with personal data, recordings, transcripts, or diagnostics.
- Secrets or secret-shaped data: API keys, passwords, cookies, tokens,
  certificates, provisioning profiles, private endpoints, QR credentials, or
  authentication exports. Use clearly fake values such as `example.com`,
  `192.168.1.10`, and `YOUR_API_KEY` in examples.
- Reverse-engineering notes, decrypted traffic, decompiled-client references,
  credential extraction tools, proprietary protocol specifications, or material
  that depends on a third party's non-public implementation.
- Third-party SDK binaries, headers, images, videos, fonts, or copied product
  copy unless the maintainer has confirmed redistribution rights and recorded
  the source, license, version, and intended use in the change.

### Product and compatibility claims

- Describe only verified, current behavior. Do not infer compatibility from an
  imported file, a prototype, a screenshot, or an unmerged branch.
- Name `Vobot V05E` in full. Do not imply that competitor hardware can connect,
  pair, or work directly with Nomily unless a maintainer has explicitly approved
  that public claim in writing.
- Do not claim that Nomily is fully offline, fully self-hosted, subscription-free
  in all respects, or private by default. The approved wording is: Nomily does
  not charge a subscription fee; third-party ASR and LLM services may charge
  under their own terms.
- State limitations beside claims: local ASR and Ollama are user-operated
  integrations, and availability, retention, pricing, and data handling belong
  to the selected provider.

### Language and documentation rules

- Public source comments, default strings, README content, examples, CLI output,
  generated filenames, and configuration instructions must be English by
  default.
- Keep translated README files and localization resources. Keep non-English test
  fixtures and language-processing vocabulary only when they are functional
  data, not developer notes.
- Route visible UI text through the localization system. Do not add hard-coded
  Chinese text to Swift, Kotlin, JavaScript, Python, YAML, plist, or shell code.
- Do not put private implementation rationale in source comments. Keep comments
  short, factual, and in English; put approved architecture decisions in public
  documentation instead.

### Third-party feature removal

Before removing a device integration, SDK, framework, model, or binary:

1. Create a local rollback branch or tag and record its commit SHA.
2. Map all references with `git grep` or `rg`, including UI, configuration,
   scanning, transfer, tests, build files, and generated project files.
3. Remove the feature as one coherent change. Never delete only a framework or
   only source files while leaving imports, symbols, UI, or build links behind.
4. Regenerate project files where applicable and run the smallest relevant build
   or test. If the baseline build is already broken, report the exact blocker and
   do not present the removal as verified.
5. Remove public documentation and assets that promise the removed feature.

If a feature shares core code with the supported path, stop after the dependency
map and request a maintainer-approved refactor scope. Do not push a half-removed
feature to `main`.

### Required pre-push checks

Run the checks that apply to the changed repository. At minimum:

```sh
git diff --check
git grep -n -e '用户：' -e '饭耳' -e 'oazon' -e 'decompil' -e '逆向' -- . || true
git grep -nP '[\x{4e00}-\x{9fff}]' -- . ':!README.zh-*' ':!*.lproj/Localizable.strings' ':!**/values-zh*' ':!**/values-ja*' || true
```

Then review every remaining match. It must be an approved translation, language
fixture, or functional vocabulary item. Do not use these commands as a reason
to delete legitimate localization files.

Also run the repository's formatter, unit tests, build, and asset validation
that cover the files changed. Record any pre-existing failure separately from
the current change.

### Git and publication safety

- Use focused commits and stage files by name. Do not commit generated build
  numbers, local IDE state, caches, or unrelated changes.
- Never force-push, rewrite history, purge releases, delete tags, delete a
  repository, or change visibility without explicit maintainer approval in the
  current task. Removing a file from `HEAD` does not remove it from Git history.
- For a confirmed secret or private-data exposure, stop publication, preserve a
  local rollback point, notify the maintainer, and propose a separate history
  rewrite and credential-rotation plan. Do not perform either without approval.
- Before replacing an image, video, or poster, check its license, ownership,
  language, visible personal information, and consistency with the latest
  product claim.

### Agent handoff format

Before changing a public repository, state: scope, assumptions, affected public
claims, and verification plan. On completion, report the commit SHA, files
changed, checks run, known limitations, and anything intentionally not removed.
