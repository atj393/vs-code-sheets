
# Pre-Commit Hook to Prevent Direct Commits to Restricted Branches

This guide explains how to set up a Git pre-commit hook that restricts direct commits to the `develop` branch and any additional branches you specify. This setup will help maintain a clean and organized Git workflow by ensuring that changes are committed only from feature branches.

## Setup Instructions

### 1. Navigate to Your Project's Root Directory
- Open your project in Visual Studio Code (VSCode) or any code editor.
- Ensure that you are in the root directory of your project.

### 2. Create a Pre-Commit Hook Script
- In the `.git/hooks` directory of your project, create a file named `pre-commit`:

```bash
touch .git/hooks/pre-commit
```

- Make the script executable:

```bash
chmod +x .git/hooks/pre-commit
```

### 3. Edit the Pre-Commit Hook Script
- Open the `pre-commit` file in VSCode and add the following script:

```bash
#!/bin/bash

# List of branches where commits should be restricted
restricted_branches=("develop" "branch1" "branch2" "branch3")

# Get the current branch name
current_branch=$(git rev-parse --abbrev-ref HEAD)

# Check if the current branch is in the restricted branches list
for branch in "${restricted_branches[@]}"; do
  if [[ "$current_branch" == "$branch" ]]; then
    echo "ERROR: Direct commits to '$branch' branch are not allowed."
    echo "Please create a new feature branch and commit your changes there."
    exit 1
  fi
done

# If no match is found, allow the commit
exit 0
```

- **Note:** Replace `branch1`, `branch2`, `branch3`, etc., with the names of branches you want to restrict, in addition to the `develop` branch.

### 4. Make the Hook Executable
- If you haven't already made the file executable, run the following command:

```bash
chmod +x .git/hooks/pre-commit
```

### Explanation of the Script
- The script uses `git rev-parse --abbrev-ref HEAD` to get the current branch name.
- It checks if the current branch is in the list of restricted branches.
- If the branch is restricted, the script prevents the commit and displays an error message.
- If the branch is not restricted, the commit proceeds.

### 5. Using the Setup in VSCode
- **Check the `.git/hooks` Folder**: Make sure your hook script is present in the `.git/hooks` folder of your project.
- **Commit Changes in VSCode**: When you try to commit directly to the `develop` branch or any restricted branch, the hook will prevent the commit and display the error message.

### Optional: Customize the Error Message
- You can modify the error message to provide more guidance, such as creating a feature branch.

## Benefits of This Setup
- Prevents accidental commits to important branches like `develop`.
- Encourages the use of feature branches for a cleaner Git history.

## Troubleshooting
- If the script does not execute, ensure the file has the correct permissions using `chmod +x .git/hooks/pre-commit`.
- Make sure you are in a Git-enabled project and the `.git/hooks` directory exists.

---

This setup ensures that no commits are accidentally made to restricted branches, keeping your Git workflow safe and organized.
