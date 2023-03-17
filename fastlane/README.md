https://fastlane.tools/

### 1. 安装Fastlane

#### Ruby

```Shell
# fastlane supports Ruby versions 2.5 or newer.
ruby --version
```
#### Bundler

-   Install _Bundler_ by running `gem install bundler`
-   Create a `./Gemfile` in the root directory of your project with the content
```Shell
source "https://rubygems.org"
gem "fastlane"
```

-   Run `bundle update` and add both the `./Gemfile` and the `./Gemfile.lock` to version control
-   Every time you run _fastlane_, use `bundle exec fastlane [lane]`
-   On your CI, add `bundle install` as your first build step
-   To update _fastlane_, just run `bundle update fastlane`

#### Homebrew
```Shell
brew install fastlane
```
#### System Ruby + RubyGems (macOS/Linux/Windows)
不推荐
```Shell
sudo gem install fastlane
```

### 2. 设置Fastlane
进入App目录，运行如下命令：
```Shell
fastlane init
```
fastlane 会自动检测你的项目，并询问任何缺失的信息。

- Learn more about how to automatically generate localized App Store screenshots
	https://docs.fastlane.tools/getting-started/ios/screenshots/

- Learn more about distribution to beta testing services
	https://docs.fastlane.tools/getting-started/ios/beta-deployment/

- Learn more about how to automate the App Store release process
	https://docs.fastlane.tools/getting-started/ios/appstore-deployment/

- Learn more about how to setup code signing with fastlane
	https://docs.fastlane.tools/codesigning/getting-started/

#### 2.1. iOS

```Shell
# Xcode command line tools (macOS)
xcode-select --install

```

