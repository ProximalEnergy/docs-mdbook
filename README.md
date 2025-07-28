# Proximal Energy Documentation

## Getting Started

- Download `rustup` by running `brew install rustup`. This command also installs `cargo` which is Rust's package manager
- Install the rust toolchain `rustup-init` .
- Navigate to this directory in your terminal.
- Run `cargo build` to install the dependencies.
- Run `mdbook serve --open` to serve the book locally!

## Adding New Pages

- To create new pages, add a new section to `SUMMARY.md`. `mdBook` will automatically update your file structure based off of what is in your `SUMMARY.md` file.
- After a new Markdown file is generated you can add content to it.

## CI/CD

- The CI/CD pipeline is set up to build the book and push it to via github pages
- Any push to main will trigger a build and republish of the documentation book

## Updating the Changelog

- Run `git log --since="<start_date>" --until="<end_date>" --pretty=format:"%h %s%n%b%n" | pbcopy` in the `web-app` on the `main` branch repo to copy the `git log` outputs to your clipboard. Note that `--since` is inclusive and `--until` is exclusive. If you are updating the changelog on a Monday, you can also run `git log --since="7 days ago" --pretty=format:"%h %s%n%b%n" | pbcopy` to get the last 7 days of commits.
- Paste the clipboard contents into an LLM to generate a changelog. The prompt below is a good starting point.
- To update the changelog, edit the `changelog/changelog.md` file.

```
You are a helpful assistant that generates a changelog for a given codebase.

You will be given a list of commits and their descriptions.

Please generate a changelog with sections for New Features, Improvements, and Bug Fixes. This changelog will be user facing, so don't include any technical details such as explicit code changes or issue tracking numbers. Use a professional but exciting tone and be concise. Do not use emojis. Generate the changelog in raw markdown format with section headers and list items. The section headers should be h3 tags.

Below are the commits and their descriptions:

<git commits>
```

## Alternative Setup

Follow these steps to set up `mdbook` on your system:

### 1. Remove Any Existing Rust Installation (Optional)

If you have an existing Rust installation and want to start fresh, you can remove it with:

```bash
rm -rf ~/.cargo ~/.rustup
```

### 2. Install Rust Using Rustup

Run the following command to install Rust and Cargo:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

### 3. Add Cargo to Your Path (If Not Automatically Added)

Ensure Cargo's bin directory is in your PATH:

```bash
echo 'export PATH="$HOME/.cargo/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

### 4. Initialize a New Cargo Project (If Needed)

If you don’t already have a Cargo.toml file, initialize a new Cargo project:

```bash
cargo init
```

### 5. Add mdbook as a Dependency

Inside your project directory, add mdbook as a dependency:

```bash
cargo add mdbook
```

### 6. Install mdbook Globally

To make mdbook accessible globally, install it with Cargo:

```bash
cargo install mdbook
```

### 7. Serve the Book

Run the following command to serve your mdbook locally:

```bash
mdbook serve --open
```

This will start a local server, automatically open your browser, and display your book at http://localhost:3000.
