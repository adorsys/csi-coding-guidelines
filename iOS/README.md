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

### Speed up UI (and Unit) Tests

You can skip most animations by disabling them in UI tests:

```swift
UIView.setAnimationsEnabled(false)
```

It's probably a good idea to only do this in `DEBUG` builds and hide it behind
a launch argument.

The same applies to Unit Tests. Here, it's elegant to do this in a class that
implements `XCTestObservation`.

See also the [`snapshot` tips] for more info.

An alternative to this is to modify the layer speed of windows, which keeps
animations around but makes them run really fast.
You can read more about this in the "[ludicrous mode][]" blog post at PSPDFKit.

[snapshot tips]: https://github.com/fastlane/fastlane/blob/master/fastlane/lib/fastlane/actions/docs/capture_ios_screenshots.md#tips
[ludicrous mode]: https://pspdfkit.com/blog/2016/running-ui-tests-with-ludicrous-speed/

### Files

#### .swiftlint.default.yml

This file contains our default swiftlint rules that we consider best practice. These should be used in all projects.
