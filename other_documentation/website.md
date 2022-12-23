# Install (Mac)

- [Guide](https://jekyllrb.com/docs/installation/macos/)

```bash
brew install chruby ruby-install xz
ruby-install ruby

echo "source $(brew --prefix)/opt/chruby/share/chruby/chruby.sh" >> ~/.zshrc
echo "source $(brew --prefix)/opt/chruby/share/chruby/auto.sh" >> ~/.zshrc
echo "chruby ruby-3.1.3" >> ~/.zshrc # run 'chruby' to see actual version

ruby -v

gem install jekyll
```

# Setup

## Init

```bash
gem install jekyll bundler
jekyll new docs
cd docs
```

And follow the guide [here](https://github.com/lorepirri/cayman-blog)

## Development

```bash
bundle exec jekyll serve --livereload
```

## Build

```bash
jekyll build
```