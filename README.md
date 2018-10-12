# SDWebImageSVGCoder

[![CI Status](https://img.shields.io/travis/lizhuoli1126@126.com/SDWebImageSVGCoder.svg?style=flat)](https://travis-ci.org/lizhuoli1126@126.com/SDWebImageSVGCoder)
[![Version](https://img.shields.io/cocoapods/v/SDWebImageSVGCoder.svg?style=flat)](https://cocoapods.org/pods/SDWebImageSVGCoder)
[![License](https://img.shields.io/cocoapods/l/SDWebImageSVGCoder.svg?style=flat)](https://cocoapods.org/pods/SDWebImageSVGCoder)
[![Platform](https://img.shields.io/cocoapods/p/SDWebImageSVGCoder.svg?style=flat)](https://cocoapods.org/pods/SDWebImageSVGCoder)
[![Carthage compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/dreampiggy/SDWebImageSVGCoder)


## What's for
SDWebImageSVGCoder is a SVG coder plugin for [SDWebImage](https://github.com/rs/SDWebImage/) framework, which provide the image loading support for [SVG](https://en.wikipedia.org/wiki/Scalable_Vector_Graphics) using [SVGKit](https://github.com/SVGKit/SVGKit) SVG engine.

## Example

To run the example project, clone the repo, and run `pod install` from the Example directory first.

You can modify the code or use some other SVG files to check the compatibility.

## Requirements

+ iOS 8
+ tvOS 9

## Installation

#### CocoaPods

SDWebImageSVGCoder is available through [CocoaPods](https://cocoapods.org). To install
it, simply add the following line to your Podfile:

```ruby
pod 'SDWebImageSVGCoder'
pod 'SVGKit', :git => 'https://github.com/SVGKit/SVGKit.git', :branch => '2.x'
```

**Note:** Because the dependency third party library [SVGKit](https://github.com/SVGKit/SVGKit#versions) does not release the 2.x version, and the current latest release version is not compatible for Xcode 9/10. So you should specify `2.x` branch dependency or using commit-id to import the dependency through CocoaPods.

#### Carthage

SDWebImageSVGCoder is available through [Carthage](https://github.com/Carthage/Carthage).

Note that because the dependency SDWebImage currently is in beta. You should use `Carthage v0.30.1` or above to support beta [sem-version](https://semver.org/).

```
github "SDWebImage/SDWebImageSVGCoder"
github "SVGKit/SVGKit" "2.x"
```

## Usage

### Use UIImageView (render SVG as bitmap image)

To use SVG coder, you should firstly add the `SDImageSVGCoder` to the coders manager. Then you can call the View Category method to start load SVG images.

Because SVG is a [vector image](https://en.wikipedia.org/wiki/Vector_graphics) format, which means it does not have a fixed bitmap size. However, `UIImage` or `CGImage` are all [bitmap image](https://en.wikipedia.org/wiki/Raster_graphics). For `UIImageView`, we will only parse SVG with a fixed image size (from the SVG viewPort information). But we also support you to specify a desired size during image loading using `vectorImageSize` context option.

+ Objective-C

```objectivec
SDImageSVGCoder *SVGCoder = [SDImageSVGCoder sharedCoder];
[[SDImageCodersManager sharedManager] addCoder:SVGCoder];
UIImageView *imageView;
// this arg is optional, if don't provide, use the viewport size instead
CGSize vectorImageSize = CGSizeMake(100, 100);
[imageView sd_setImageWithURL:url placeholderImage:nil options:0 context:@{SDWebImageContextVectorImageSize : @(vectorImageSize)];
```

+ Swift

```swift
let SVGCoder = SDImageSVGCoder.shared
SDImageCodersManager.shared.addCoder(SVGCoder)
let imageView: UIImageView
imageView.sd_setImage(with: url)
// this arg is optional, if don't provide, use the viewport size instead
let vectorImageSize = CGSize(width: 100, height: 100)
imageView.sd_setImage(with: url, placeholderImage: nil, options: [], context: [.vectorImageSize : vectorImageSize])
```

### Use SVGKImageView (render SVG as vector image)

[SVGKit](https://github.com/SVGKit/SVGKit) also provide some built-in image view class for vector image loading (scale to any size without losing detail). The `SVGKLayeredImageView` && `SVGKFastImageView` are the subclass of `SVGKImageView` base class. We supports these image view class as well. You can just use the same API like normal `UIImageView`.

For the documentation about `SVGKLayeredImageView`, `SVGKFastImageView` or `SVGKImageView`, check [SVGKit](https://github.com/SVGKit/SVGKit) repo for more information.

**Note**: If you only use these image view class and don't use SVG on `UIImageView`, you don't need to register the SVG coder to coders manager. These image view loading was using the [Custom Image Class](https://github.com/rs/SDWebImage/wiki/Advanced-Usage#customization) feature of SDWebImage.

**Attention**: These built-in image view class does not works well on `UIView.contentMode` property, you need to re-scale the layer tree after image was loaded. We provide a simple out-of-box solution to support it. Set the `sd_adjustContentMode` property to `YES` then all things done.

+ Objective-C

```objectivec
SVGKImageView *imageView; // can be either `SVGKLayeredImageView` or `SVGKFastImageView`
imageView.contentMode = UIViewContentModeScaleAspectFill;
imageView.sd_adjustContentMode = YES; // make `contentMode` works
[imageView sd_setImageWithURL:url];
```

+ Swift:

```swift
let imageView: SVGKImageView // can be either `SVGKLayeredImageView` or `SVGKFastImageView`
imageView.contentMode = .aspectFill
imageView.sd_adjustContentMode = true // make `contentMode` works
imageView.sd_setImage(with: url)
```

## Screenshot

<img src="https://raw.githubusercontent.com/dreampiggy/SDWebImageSVGCoder/master/Example/Screenshot/SVGDemo.png" width="300" />

## Author

DreamPiggy

## License

SDWebImageSVGCoder is available under the MIT license. See the LICENSE file for more info.


