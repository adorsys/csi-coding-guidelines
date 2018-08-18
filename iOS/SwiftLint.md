## Integrate SwiftLint to a project

### 1. Install SwiftLint

We recommend using Cocoapods to install SwiftLint.

```
Podfile

pod 'SwiftLint'
```

Alternatively, you can install it via brew

```shell
brew install swiftlint
```

### 2. Integrate in build phase

Create a new Run Script phase in Xcode and paste the following script.

```shell
set -euo pipefail

SWIFTLINT_CONFIG_PATH=".swiftlint.yml"

# If the file does not exist or is older than 1 day, download it
if [[ ! -f $SWIFTLINT_CONFIG_PATH ]] || [[ $(find "$SWIFTLINT_CONFIG_PATH" -mtime +1 -print) ]]; then
    curl -H 'Accept: application/vnd.github.v3.raw' -L 'https://api.github.com/repos/adorsys/csi-coding-guidelines/contents/iOS/.swiftlint.default.yml' > $SWIFTLINT_CONFIG_PATH
fi

"${PODS_ROOT}/SwiftLint/swiftlint" autocorrect --config ${SWIFTLINT_CONFIG_PATH}
"${PODS_ROOT}/SwiftLint/swiftlint" lint --config ${SWIFTLINT_CONFIG_PATH}
```

This will download the [default swiftlint file][] (at most once every day)
and use that to configure SwiftLint.

Please be sure to cache this file during CI since GitHub has a [rate limit][] 
in place for the files API.

Here's an example for [GitLab-CI][gitlab cache]:
```yml
.gitlab-ci.yml

cache:
  paths:
    - .swiftlint.yml
```

### 3. Ignore rules that aren't relevant to the project

You can disable and enable other rules in subfolders, for example:

```
Sources/.swiftlint.yml

disabled_rules:
  - force_unwrap
```

This is especially useful for unit test files.

#### :warning: Limitations:

Nested `.swiftlint.yml` files cannot exclude or include files, only rules.

[default swiftlint file]: https://github.com/adorsys/csi-coding-guidelines/blob/master/iOS/.swiftlint.default.yml
[rate limit]: https://developer.github.com/v3/#rate-limiting
[gitlab cache]: https://docs.gitlab.com/ee/ci/yaml/#cache
