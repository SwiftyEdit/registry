# SwiftyEdit Registry

Official plugin & theme registry for SwiftyEdit.

This repository is a flat-file, metadata-only registry — it does ___not___
contain plugin or theme code. Each entry is a JSON file pointing to the
actual GitHub repository where the plugin/theme lives.

The data here is synced automatically into the SwiftyEdit catalog
service, which powers the plugin/theme browser in the SwiftyEdit backend
and the public catalog on [swiftyedit.org](https://swiftyedit.org).

## Structure

```
registry/
├── plugins/
│   └── {slug}.json
├── themes/
│   └── {slug}.json
└── schema/
    └── entry.schema.json
```

## Submitting a plugin or theme

See [CONTRIBUTING.md](CONTRIBUTING.md) for the submission format and
process.

## License

Registry entries are metadata only. Each plugin/theme is licensed
separately by its own author in its own repository.
