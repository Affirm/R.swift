# Updating R.swift

We use our own version of R.swift forked from https://github.com/mac-cain13/R.swift.

## Development Process

Pull the repo.

Develop off of `affirm-main`.
This is the branch with all the Affirm specific stuff so we can pull the `master` branch and rebase
`affirm-main` onto it, preserving our changes when updating to the latest R.swift.

To make changes, pull affirm-main and create your development branch from it.
When opening a PR, use `affirm-main` as the base branch. You'll need to

## Command Line Build

cd ./R.swift
swift build # Build for debug.
swift test  # Run tests
swift build -c release # Build a release version.

swift build -c release --arch arm64 --arch x86_64 # Fat binary.

Coding philosophy:
https://github.com/mac-cain13/R.swift/issues/177

```
      static func AffirmLocalizedString(_ key: String, tableName: String, bundle: Bundle, comment: String) -> String {
        let result = NSLocalizedString(key, tableName: tableName, bundle: bundle, comment: comment)
        return (tableName.lowercased() == "sensitive" && result == key) ? "" : result
      }

      static func AffirmDisplayKey(_ key: String, tableName: String) -> String {
        return (tableName.lowercased() == "sensitive") ? "" : key
      }

      static func AffirmLocalizedString(_ stringResource: Rswift.StringResource, preferredLanguages: [String]?) -> String {
        guard let preferredLanguages = preferredLanguages else {
          return AffirmLocalizedString(stringResource.key, tableName: stringResource.tableName, bundle: hostingBundle, comment: "")
        }

        guard let (_, bundle) = localeBundle(tableName: stringResource.tableName, preferredLanguages: preferredLanguages) else {
          return AffirmDisplayKey(stringResource.key, tableName: stringResource.tableName)
        }
        return AffirmLocalizedString(stringResource.key, tableName: stringResource.tableName, bundle: bundle, comment: "")
      }

      #if false
      /// en translation: Example string
      ///
      /// Locales: en, en-CA, fr-CA
      static func exampleString(preferredLanguages: [String]? = [affirm_preferredLanguageIdentifier]) -> String {
        guard let preferredLanguages = preferredLanguages else {
          return NSLocalizedString("example.string", tableName: "Sensitive", bundle: hostingBundle, comment: "")
        }

        guard let (_, bundle) = localeBundle(tableName: "Sensitive", preferredLanguages: preferredLanguages) else {
          return "example.string"
        }

        return NSLocalizedString("example.string", tableName: "Sensitive", bundle: bundle, comment: "")
      }
      #else
      static func exampleString(preferredLanguages: [String]? = [affirm_preferredLanguageIdentifier]) -> String {
        return AffirmLocalizedString(exampleString, preferredLanguages: preferredLanguages)
      }
      #endif
```
