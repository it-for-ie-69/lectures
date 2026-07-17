# Mac Setup (Homebrew)

## 1. Install Xcode Command Line Tools

Homebrew needs Apple developer tools first.

- Open Terminal
- Run:

```bash
xcode-select --install
```

## 2. Install Homebrew

Run the official install command:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

## 3. Add Homebrew to your shell

After installation, follow the instructions shown in Terminal.

For Apple Silicon Macs (M1/M2/M3), it is usually:

```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

For Intel Macs, it is usually:

```bash
echo 'eval "$(/usr/local/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/usr/local/bin/brew shellenv)"
```

## 4. Verify installation

```bash
brew --version
brew doctor
```

If `brew doctor` shows warnings, read and fix them.

## 5. Install common software for class

```bash
brew install git
brew install node
brew install --cask visual-studio-code
brew install --cask dbeaver-community
brew install --cask insomnia
```

## 6. Useful Homebrew commands

```bash
brew update
brew upgrade
brew list
brew uninstall <package-name>
```
