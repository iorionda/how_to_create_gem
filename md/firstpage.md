## Gemの作り方
💎普通の Gem の簡単な作り方💎
<b><!-- --></b>  
<b><!-- --></b>  
Created by [Iori ONDA](https://github.com/iorionda)  
http://twitter.com/120reset

---
## about me

>>>

![profile](./images/profile.png)

---

## 目的

>>>

Gemの作り方をおぼえて  
共通する処理を気軽にGemにして  
本質的なロジックに注力できるようになる!!1✊

---

## 内容

>>>

- Gemとは何か
- Gemを作る基本的な流れ
- まとめ

---

## そもそも Gem ってなんなの

>>>

- GemはRubyのライブラリ管理の為のコマンド。  
- Rubyのライブラリの検索・インストール・アップデートなどを最小限の苦労で出来る仕組みを提供してくれる。  
- Rubyのバージョン1.9以降は標準添付となっている。

>>>

昔は *echoe gem* や *Jeweler* などがあったけれど、今は *Bundler* で大勢が決まっている。
なので、今回も *Bundler* を使ってGemを作る方法を紹介する💎💎💎

>>>

今まで便利に使ってはいるけれど  
あまりgemを作って公開している人はいないはず😌

>>>

今日から自分で作ったものもgemでインストールできるようにしてみよう😄😄😄  
<b><!-- --></b>  
果たして簡単にできるのか!?👏👉

---

## 作業環境

>>>

- MacBook Pro Retina15 OSX 10.9.5
- Ruby
```shellscript
% ruby --version
ruby 2.2.2p95 (2015-04-13 revision 50295) [x86_64-darwin13]
```

- Gem  
```
% gem --version
2.4.8
```

---

## 基本的な流れ

---

## ジェネレータ

>>>

`bundle gem` というコマンドでプロジェクトのひな形をつくる。

```ruby
% bundle gem gem_name
```

>>>

実際にやってみる

```
% bundle gem retriable
Creating gem 'retriable'...
/Users/ONDA/.rbenv/versions/2.2.2/lib/ruby/gems/2.2.0/gems/bundler-1.10.5/lib/bundler/cli/gem.rb:26: warning: Insecure world writable dir /Users/ONDA/bin/FDK/Tools/osx in PATH, mode 040777
Do you want to include a code of conduct in gems you generate?
<u are prepared to do that. For suggestions about how to enforce codes of conduct, see bit.ly/coc-enforcement. y/(n): y
Code of conduct enabled in config
Do you want to license your code permissively under the MIT license?
<ong as they admit you created it. You can read more about the MIT license at choosealicense.com/licenses/mit. y/(n): y
MIT License enabled in config
Do you want to generate tests with your gem?
Type 'rspec' or 'minitest' to generate those test files now and in the future. rspec/minitest/(none): minitest
      create  retriable/Gemfile
      create  retriable/.gitignore
      create  retriable/lib/retriable.rb
      create  retriable/lib/retriable/version.rb
      create  retriable/retriable.gemspec
      create  retriable/Rakefile
      create  retriable/README.md
      create  retriable/bin/console
      create  retriable/bin/setup
      create  retriable/CODE_OF_CONDUCT.md
      create  retriable/LICENSE.txt
      create  retriable/.travis.yml
      create  retriable/test/test_helper.rb
      create  retriable/test/retriable_test.rb
Initializing git repo in /Users/ONDA/Dropbox/github/retriable
```

>>>

途中で3回くらい質問があるので y/(n) で回答する。  

>>>

設定した項目は  `~/.bundle/config` に保存される。

```
% cat ~/.bundle/config
---
BUNDLE_GEM__COC: true
BUNDLE_GEM__MIT: true
BUNDLE_GEM__TEST: minitest
```

>>>

## gem名について

>>>

gemの名前の - はディレクトリ構造に展開される。  
例えば foo-bar という名前でgemを作成すると  
<b><!-- --></b>  
`lib/foo-bar.rb` ではなく  
<b><!-- --></b>  
`lib/foo/bar.rb` となる。

>>>

## 閑話休題

>>>

これでプロジェクトのひな形はできたので  
肝心の中身を作っていく。

---

## gemspec

>>>

gem_name.gem_specというファイルが作成されているはず

>>>

![gemspec](./images/gemspec.png)

>>>

gemspecファイルにはgemのメタデータが書かれている。

>>>

生成された直後はこんな感じ

```ruby
# coding: utf-8
lib = File.expand_path('../lib', __FILE__)
$LOAD_PATH.unshift(lib) unless $LOAD_PATH.include?(lib)
require 'retriable/version'

Gem::Specification.new do |spec|
  spec.name          = "retriable"
  spec.version       = Retriable::VERSION
  spec.authors       = ["Iori ONDA"]
  spec.email         = ["iori.onda@gmail.com"]

  spec.summary       = %q{TODO: Write a short summary, because Rubygems requires one.}
  spec.description   = %q{TODO: Write a longer description or delete this line.}
  spec.homepage      = "TODO: Put your gem's website or public repo URL here."
  spec.license       = "MIT"

  # Prevent pushing this gem to RubyGems.org by setting 'allowed_push_host', or
  # delete this section to allow pushing this gem to any host.
  if spec.respond_to?(:metadata)
    spec.metadata['allowed_push_host'] = "TODO: Set to 'http://mygemserver.com'"
  else
    raise "RubyGems 2.0 or newer is required to protect against public gem pushes."
  end

  spec.files         = `git ls-files -z`.split("\x0").reject { |f| f.match(%r{^(test|spec|features)/}) }
  spec.bindir        = "exe"
  spec.executables   = spec.files.grep(%r{^exe/}) { |f| File.basename(f) }
  spec.require_paths = ["lib"]

  spec.add_development_dependency "bundler", "~> 1.10"
  spec.add_development_dependency "rake", "~> 10.0"
  spec.add_development_dependency "minitest"
end
```

>>>

まずは TODO を埋めていこう

>>>

## このファイルの大事なところ

>>>

- .add_dependency
- .add_development_dependency

>>>

## .add_dependency  
<b><!-- --></b>  
依存するgemを書く

>>>

## .add_development_dependency  
<b><!-- --></b>  
開発時に依存するgemを書く

>>>

## 他の項目の意味は???

>>>

- name (gemのpackage名)
- version (gemのversion)
- authors (開発者名)
- email (連絡先)
- description (詳細説明)
- summary (1行の簡易説明)
- homepage (関連URL)
- license (gemの属するライセンス)
- files (公開ファイル一覧)
- executables (実行可能ファイル一覧)
- test_files (テスト用ファイル一覧)
- require_paths (実際のgemの処理を行うファイルのパス指定)

>>>

## それ以外の設定値は?

>>>

default値を書き出してみた

```
[1] 2.2.2-p95(main)> Gem::Specification.new do |s|
[1] 2.2.2-p95(main)*   Gem::Specification.instance_methods.grep(/\w+=$/).sort.each do |i|
[1] 2.2.2-p95(main)*   default = s.send(i[0..-2]) rescue nil
[1] 2.2.2-p95(main)*     next if [nil, [], {}].include?(default)
[1] 2.2.2-p95(main)*     print i, " "
[1] 2.2.2-p95(main)*     p default
[1] 2.2.2-p95(main)*   end
[1] 2.2.2-p95(main)* end
activated= false
base_dir= "/Users/ONDA/.rbenv/versions/2.2.2/lib/ruby/gems/2.2.0"
bindir= "bin"
date= 2015-07-22 00:00:00 UTC
extension_dir= "/Users/ONDA/.rbenv/versions/2.2.2/lib/ruby/gems/2.2.0/extensions/x86_64-darwin-13/2.2.0-static/-"
full_gem_path= "/Users/ONDA/.rbenv/versions/2.2.2/lib/ruby/gems/2.2.0/gems/-"
has_rdoc= true
installed_by_version= #<Gem::Version "0">
original_platform= "ruby"
platform= "ruby"
require_path= "lib"
require_paths= ["lib"]
required_ruby_version= #<Gem::Requirement:0x007febd14dcaa0 @requirements=[[">=", #<Gem::Version "0">]]>
required_rubygems_version= #<Gem::Requirement:0x007febd14dc9d8 @requirements=[[">=", #<Gem::Version "0">]]>
rubygems_version= "2.4.5"
specification_version= 4
```

>>>

でも意識することはないです😇

>>>

## 注意点

>>>

世界中の開発者が見るので極力英語で書いた方がいい  
👱👲👳

---

## 中身の作り方

>>>

まずは *lib/retriable.rb* をみてみよう。

>>>

```ruby
require "retriable/version"

module Retriable
  # Your code goes here...
end
```

>>>

class じゃなくて module になっている。

>>>

ここは module のままで  
実際の処理は別の class に任せる。

>>>

RetryLimitError という class を作るなら  
*lib/retriable/retry_error.rb* というファイルを作る。

>>>

```
% touch lib/retriable/retry_error.rb
```

>>>

こんな感じに書いておく

```ruby
module Retriable
  class RetryError
  # Your code goes here...
end
```

>>>

クラス構造とディレクトリ構造を一致させないとダメ 😖

>>>

作ったファイルを *lib/retriable.rb* の中でロードする。

>>>

```ruby
require "retriable/version"
require 'retriable/retry_error'

module Retriable
  # Your code goes here...
end
```

---

## ビルドとリリース

>>>

ビルドやリリースは rake コマンドが用意されている

>>>

```
% rake -T
rake build          # Build retriable-0.1.0.gem into the pkg directory
rake install        # Build and install retriable-0.1.0.gem into system gems
rake install:local  # Build and install retriable-0.1.0.gem into system gems without network access
rake release        # Create tag v0.1.0 and build and push retriable-0.1.0.gem to Rubygems
rake test           # Run tests
```

>>>

## ビルド

>>>

```
% rake build
retriable 0.1.0 built to pkg/retriable-0.1.0.gem.
```

>>>

gem ファイルが完成したぞ ✊

>>>

## インストール

>>>

```
% rake install
retriable 0.1.0 built to pkg/retriable-0.1.0.gem.
retriable (0.1.0) installed.
```

>>>

### RubyGemsのAPIキーの取得

>>>

まずは RubyGems のアカウントを作成する

>>>

```
% curl -u [user-name] https://rubygems.org/api/v1/api_key.yaml > ~/.gem/credentials
```

>>>

## リリース

>>>

リリースをするには、手動で lib/gem_name/version.rb のバージョンをあげなければいけない😕

>>>

```
% rake release
```

---

## まとめ

>>>

これでもういつでもgemを作れる🙏🙏🙏

---

## 参考資料

>>>

- http://masarakki.github.io/blog/2014/02/15/how-to-create-gem/
- http://morizyun.github.io/blog/ruby-gem-easy-publish-library-rails/
- http://d.hatena.ne.jp/zariganitosh/20141016/minimum_making_of_gem
