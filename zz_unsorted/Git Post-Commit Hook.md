## Triggering Scripts on Git Commit

For running scripts when a file is committed, you can use Git hooks:

1. **Post-Commit Hook**

Create a file named `post-commit` in your `.git/hooks/` directory ([Visual Studio Community Run command after git commit - Stack Overflow](https://stackoverflow.com/questions/57827585/visual-studio-community-run-command-after-git-commit)):

bash

`#!/bin/sh python /path/to/your/script.py`

Make sure to make the file executable:

bash

`chmod +x .git/hooks/post-commit`

This script will run automatically after every commit.