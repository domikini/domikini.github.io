https://stackoverflow.com/questions/52522565/git-is-not-working-after-macos-update-xcrun-error-invalid-active-developer-pa

## Xcode Command-line needs to be updated

Go back to your terminal and hit:


`xcode-select --install`


You'll then receive:

```
xcode-select: note: install requested for command line developer tools
You will be prompted at this point in a window to update Xcode Command Line tools. (which may take a while)
```

open a new terminal window and your development tools should be returned.

Addition: With any major or semi-major update you'll need to update the command line tools in order to get them functioning properly again. Check Xcode with any update. This goes beyond Mojave...

After that restart your terminal