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
