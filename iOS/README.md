# Coding Guidelines

## iOS

- Swiftlint File Example
- Fastlane Configuration / Example / Guideline / Changelog
- Code Snippets (Touch ID, Instantiable, etc)


### Testing

All test methods should follow the naming scheme

`func test<Action>Should<Consequences> () {}`

such as:

`func testTotalAmountSetterShouldApplyNegativeStyleWhenAmountIsNegative() {}`

These names should be as descriptive as possible, so they add the most value
when they show up in log files.
In tests, we never have to call these functions, so the length does not matter.
