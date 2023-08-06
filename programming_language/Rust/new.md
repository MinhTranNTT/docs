```rust
~> cargo new design_patterns
warning: compiling this new package may not work due to invalid workspace configuration

current package believes it's in a workspace when it's not:
current:   D:\.github\rust\design_patterns\Cargo.toml
workspace: D:\.github\rust\Cargo.toml

this may be fixable by adding `design_patterns` to the `workspace.members` array of the manifest located at: D:\.github\rust\Cargo.toml
Alternatively, to keep it out of the workspace, add the package to the `workspace.exclude` array, or add an empty `[workspace]` table to the package's manifest.
     Created binary (application) `design_patterns` package
```


```rust
This rust file does not belong to a loaded cargo project. It looks like it might belong to the workspace at /d:/.github/rust/design_patterns/Cargo.toml, do you want to add it to the linked Projects?
```



```rust
~> git push -u origin main
error: src refspec main does not match any
error: failed to push some refs to 'https://github.com/zonglin-hub/rust.git'
```