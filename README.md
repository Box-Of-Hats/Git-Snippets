# Git-Snippets

## Stop tracking a file/directory

1. `Add file/directory to .gitignore`
2. `git rm -r --cached .`
3. `git add .`
4. `git commit -m "Fix .gitignore files"`
5. `git push`

## Remove sensitive information from an accidentally published file

1. `git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch FILE_NAME_HERE.TXT' --prune-empty --tag-name-filter cat -- --all`
2. `git push REPO_NAME_HERE --force --all`

## Force empty commit to publish a branch

1. `git commit --allow-empty -m "Publish branch message"`
2. `git push`

## Undo last commit

`git reset --hard HEAD^`

## Undo last X commits

Change the '3' to be the number of commits you want to undo.

`git reset --hard HEAD~3`


## .gitignore Syntax

|  Char  | Action | Example |
| :----: | ------ | ------- |
| !      | Exclude a file or directory   | `!/js/*.js` |

## Prevent banned phrases in commits

### .git/hooks/pre-commit

```bash
#!/bin/sh
# Run a Powershell script 
c:/Windows/System32/WindowsPowerShell/v1.0/powershell.exe -ExecutionPolicy RemoteSigned -Command '.git\hooks\pre-commit.ps1'
```


### ./.git/hooks/pre-commit.ps1 

```powershell
$cachedChanges = git diff --cached

$banList = @(
    "TODO",
    "debugger"
)

foreach ($line in $cachedChanges) {
    if ($line.StartsWith("diff --git ")){
        $currentFile = $line.Split(" ") | Select-Object -Last 1
    }

    if ($line.StartsWith("+")) {
        foreach ($bannedPhrase in $banList) {
            if ($line.Contains($bannedPhrase)) {
                Write-Output "Can't commit with TODO  |  Line: '$line'  |  File: $currentFile"
                exit 1
            }
        }
    }
}
exit 0
```
