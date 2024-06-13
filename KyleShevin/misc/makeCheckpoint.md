# Checkpoints (Commits)
- `git commit -p` allows for atomic commits
- for personal projects,
    * Makefile -> generate 'checkpoint' (git commit):
        ```makefile
        checkpoint:
            @git add -A
            @git commit -m "checkpoint at $$(date '+%Y-%m-%dT%H:%M:%S%z')"
            @git push
            @echo Checkpoint created and pushed to remote
        ```
- Can now run `make checkpoint` to perform
    * stage all files to be committed, commit with message 'checkpoint @ <timestamp>', push branch, logs out word that has been performed
