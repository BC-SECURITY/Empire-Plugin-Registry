# Empire-Plugin-Registry

Starkiller's plugin marketplace is backed by plugin registries. In particular, it defaults to this repository as the primary registry.

## Branch layout

Each Empire **major** line reads from its own `<major>.x` branch of this registry. Point releases within a major (7.0, 7.1, …) all share that branch. An Empire release pins its registry branch in `config.yaml`, and that pin can never change once the release ships — so every release line has a permanent, dedicated branch:

| Empire line | Registry branch |
| ----------- | --------------- |
| 6.x         | `main`          |
| 7.x         | `7.x`           |

`main` is **not** "latest" — it is the branch that already-shipped 6.x installs are pinned to, so it must stay on 6.x-compatible plugin refs. A new major line always gets a new `<major>.x` branch; never repoint a shipped release at `main`.

To submit a plugin, open a PR updating `registry.yaml` with the new plugin.

To add new versions of a plugin, open a PR updating the `versions` list in the plugin's entry.

`ref` must point to an immutable git commit, not a branch or tag.

Example:
```yaml
  - name: Example Plugin
    homepage_url: https://github.com/bc-security/example-plugin
    source_url: https://github.com/bc-security/example-plugin
    authors:
      - name: John Doe
        handle: "@johndoe"
        link: "https://github.com/johndoe"
    description: |
      A sample Empire plugin that demonstrates the plugin architecture.
      Includes example stagers, listeners, and modules to help developers
      get started with plugin development.
    versions:
      - name: '1.0.0'
        git_url: https://github.com/BC-SECURITY/example-plugin
        ref: 'abcdefg1234567'
```


## Hosting your own registry

You can host your own plugin registry by creating a repository with a `registry.yaml` file. The schema for this file is documented in [here](./schema.json)

To add your custom registry, add it to the `plugin_registries` block in Empire's `config.yaml`.
