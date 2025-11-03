Configure VS Code
9. Once VS Code opens, click the `x` (boxed in yellow), you do not need anything on this screen <br>![alt text](<Screenshot 2025-06-11 092335.png>)
10. Click `File` in the top left corner, then `Open Folder` <br>![alt text](<Screenshot 2025-06-11 092702-1.png>)
11. Navigate to the folder that you want to open in File Explorer
12. On the `Trust Authors` page click the check box then`Yes, I trust the authors` (both boxed in yellow) <br>![alt text](<Screenshot 2025-06-11 092850.png>)


## Configure Git
20. Enter the following commands into Git BASH

* Automatically Set Upstream for New Branches
```bash
git config --global push.autoSetupRemote true # Auto-sets upstream when pushing new branches
```

* This ensures Git uses the Windows Credential Manager to save your GitLab login info.
```bash
git config --global credential.helper manager-core
```

22. Close Git Bash, you won’t need it again.