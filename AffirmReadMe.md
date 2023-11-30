# Affirm R.swift

We use our own version of R.swift forked from [https://github.com/mac-cain13/R.swift](https://github.com/mac-cain13/R.swift).

The Affirm repo is at [https://github.com/Affirm/R.swift](https://github.com/Affirm/R.swift).

Extensions:

* Adds a new CLI flag to turn off missing string warnings.
  - This is the `silenceLanguageWarnings` command flag.

* Adds a default language to each generated string function.
  - Defaults to the current value of `R.affirm_preferredLanguageIdentifier`.
  - Changeable during runtime.

* R.swift internal change that allows struct generators to create either var or let variables.
  - This was added to allow a `R.affirm_preferredLanguageIdentifier` to change during runtime.

* Added `AffirmLocalizedString` and `AffirmDisplayKey` to R.swift string generation to prevent
  leaking string key names if a language bundle is not found. This is only affects keys in
  `Sensitive.strings` language tables.

## Development Process

1. Pull the repo from [https://github.com/Affirm/R.swift](https://github.com/Affirm/R.swift)

2. Create your development branch off of `affirm-main` branch. The `affirm-main` is the branch
   with all the Affirm specific changes.

3. When opening a PR, use `Affirm` / `affirm-main` as the base branch and land your changes there.
   You'll need a PR review to land to `affirm-main`.

4. You can open the R.swift `Package.swift` in Xcode like a project file and code using the IDE.

The R.swift coding philosophy is helpful in understanding their design choices:
[R.swift development philosophy](https://github.com/mac-cain13/R.swift/issues/177)

We don't strictly follow this but it's nice to know.

### Command Line Builds

```bash
cd ./R.swift
swift build # Build for debug.
swift test  # Run tests
swift build -c release --arch arm64 --arch x86_64 # Make a fat binary.
```

The build products are in the `./.build` folder which is helpfully a hidden dot directory. Open it
with `open ./build` and it'll open in a Finder window.

## Updating Affirm R.Swift to the latest origin version

All the Affirm changes are on `affirm-main` so we can pull the fork-origin `master` branch and rebase
`affirm-main` onto it, preserving our changes when updating to the latest R.swift.

1. Checkout `master`.
2. Pull from their remote to refresh our master and push.
3. Rebase `affirm-main` on top off the new master.
