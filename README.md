# The Rubyist: An Unexpected Journey
the learning path to become a rubyist

## 1. [Ruby Version Manager](https://rvm.io/)
To install this Ruby Version Manager please issue the following commands on your machine:
```bash
gpg --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
\curl -sSL https://get.rvm.io | bash -s stable
```
First command might produce few different errors. If you receive following error:

> zsh: command not found: gpg

You will need to install gpg(s):
```sh
brew install gnupg gnupg2
```

If you receive following error:

> gpg: keyserver receive failed: No name

You will need to use --keyserver option with one of the newer GPG servers. 

Try one of the following:
```sh
gpg --keyserver hkp://keyserver.ubuntu.com --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB

gpg --keyserver hkp://keys.openpgp.org --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB

gpg --keyserver hkp://pgp.mit.edu --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
```


## 2. VSCode settings
```json
{
  "search.exclude": {
    "**/node_modules": true,
    "**/public/assets": true
  },
  "editor.scrollBeyondLastLine": false,
  "editor.tabSize": 2,
  "editor.suggestSelection": "first",
  "editor.largeFileOptimizations": false,
  "files.insertFinalNewline": true,
  "files.trimTrailingWhitespace": true,
  "files.trimFinalNewlines": true,
  "extensions.ignoreRecommendations": true,
  "terminal.integrated.scrollback": 2000,
  "security.workspace.trust.untrustedFiles": "open",
  "javascript.updateImportsOnFileMove.enabled": "always",
  "yard.spacers.afterDescription": false,
  "yard.spacers.afterTags": false,
  "yard.spacers.beforeDescription": false,
  "yard.spacers.beforeTags": false,
  "yard.spacers.separateTags": false,
  "ruby.rubocop.executePath": "/Users/ydwang/.asdf/shims/",
  "editor.formatOnSave": false,
  "[ruby]": {
    "editor.defaultFormatter": "Shopify.ruby-lsp",
    "editor.semanticHighlighting.enabled": true
  }
}

```


### rubocop errors
rubocop on VScode not working.Error “rubocop is not executable”, or somthing empty, it means vscode couldn't find rubocop path. [solution](https://www.lynnbright.com/vs-code-rubocop-ruby_executable_hooks-no-such-file-or-directory/)
```sh
"ruby.rubocop.executePath": "/Users/yaowang/.rvm/gems/ruby-2.6.8/wrappers/",
```

## 3. [vscode-ruby-debug-ide](https://github.com/rubyide/vscode-ruby/blob/main/docs/debugger.md)
Debugging in this extension implements the ruby debug ide protocol to allow VS Code to communicate with ruby debug, it requires ruby-debug-ide to be installed on your machine. This is also how RubyMine/NetBeans implement debugging by default.
```sh
gem install ruby-debug-ide
gem install debase
```



### launch.json for individual ruby
```json
{
  // Use IntelliSense to learn about possible attributes.
  // Hover to view descriptions of existing attributes.
  // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Debug Local File",
      "type": "Ruby",
      "request": "launch",
      "program": "${file}"
    }
  ]
}
```
### launch.json for rails (new project)
```json
{
  // Use IntelliSense to learn about possible attributes.
  // Hover to view descriptions of existing attributes.
  // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Rails Debug",
      "type": "Ruby",
      "request": "launch",
      "program": "${workspaceRoot}/bin/rails",
      "args": ["server"]
      "env": {
        "PATH": "/Users/yaowang/.rvm/gems/ruby-2.7.6/bin:/Users/yaowang/.rvm/gems/ruby-2.7.6@global/bin:/Users/yaowang/.rvm/rubies/ruby-2.7.6/bin:/Users/yaowang/.rvm/bin:/Users/yaowang/.nvm/versions/node/v16.15.0/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Users/yaowang/.nvm/versions/node/v16.15.0/bin:/Users/yaowang/.cargo/bin",
        "GEM_HOME": "/Users/yaowang/.rvm/gems/ruby-2.7.6",
        "GEM_PATH": "/Users/yaowang/.rvm/gems/ruby-2.7.6:/Users/yaowang/.rvm/gems/ruby-2.7.6@global",
        "RUBY_VERSION": "ruby-2.7.6"
      }
    }
  ]
}
```
> **Note**
> "env" can be gotten from this script : `printf "\n\"env\": {\n  \"PATH\": \"$PATH\",\n  \"GEM_HOME\": \"$GEM_HOME\",\n  \"GEM_PATH\": \"$GEM_PATH\",\n  \"RUBY_VERSION\": \"$RUBY_VERSION\"\n}\n\n"`

> **Warning**
> if a new version of ruby is installed, need update the env, `gem install rdebug-ide` (if missing), `gem install rake` ( if version conflicts)


[more config about rails debugger](https://rahul-arora.medium.com/debugging-ruby-on-rails-server-in-vs-code-819b45113e78)

### fix if debuger hangs
In my .env file set:
```sh
WEB_CONCURRENCY=0
RAILS_MAX_THREADS=1
```
## 4. Ruby Debug ( ruby 3 )
new for ruby 3, and default in Rails7
```
gem "debug", ">= 1.0.0".   # https://github.com/ruby/debug

{
  "type": "rdbg",
  "name": "Rails Server",
  "request": "launch",
  "command": "bin/rails",
  "script": "s",
  "rdbgPath": "bundle exec rdbg",
  "useBundler": true,
  "useTerminal": true,
  "env": {
    "WEB_CONCURRENCY": 0,
    "RAILS_MAX_THREADS": 1
  }
},
```
ref 
- https://zenn.dev/igaiga/books/rails-practice-note/viewer/ruby_rails_vscode
- https://github.com/ruby/debug

## 5. Example VSCode for ICN project
- [launch.json](vscode_config/launch.json)
- [settings.json](vscode_config/settings.json)

## 6. RuboCop (VS Code) from [conflucne](https://icapitalnetwork.atlassian.net/wiki/spaces/IR/pages/2531131477/RuboCop+VS+Code)
Make sure gems are installed under vendor/bundle. You can set that up using a bundle config command followed by the bundle install command. This is the preferred way of installing the project gems.

All bundle related command (like rails, rake, sidekiq…) from here onwards must be prepended with the bundle exec prefix otherwise they’ll point to a gem outside our vendor folder which might have a wrong version for example.

```sh
bundle config --local path 'vendor/bundle'
bundle install
```
Then, on VS Code do CMD + Shift + P and type “Open Settings (JSON)“. Make sure you have both extensions - ruby and ruby-rubocop - installed and paste the following configs in there. Remember to replace <user_name> with your user.

> **Warning**
> The RuboCop command below is for rbenv. If you use rvm it may look a little different. You may be able to locate it by doing which rubocop. Also note that if you have additional ruby/rubocop configs they may be interfering with these.
```sh
"ruby.rubocop.suppressRubocopWarnings": true,
"ruby.useLanguageServer": true,
"ruby.lint": {
    "rubocop": {
        "command": "/Users/yaowang/.rvm/gems/ruby-2.6.8/bin/rubocop" # here already changed to rvm
    }
},
```
> **Note**
> All done

