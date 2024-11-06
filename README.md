# How to edit locally:
- Download rustup `brew install rustup`
  - This command also automatically installs cargo which is rust's package manager
- Navigate to this directory in your terminal
- Add mdbook to your project by running `cargo add mdbook`
- Serve a local copy by running `mdbook serve --open`
- Make sure to make edits to the folder and files through `SUMMARY.md` as cargo will automatically update your file structure based off of what is in your `SUMMARY.md` file

# CI/CD
- The CI/CD pipeline is set up to build the book and push it to via github pages
- Any push to main will trigger a build and republish of the documentation book


# Setting Up `mdbook`

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
