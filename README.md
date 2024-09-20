

# Running Code Journal

```bash
# Source pyenv initialization script
export PATH="$HOME/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
cd /home/bearceb/Documents/code-journal;
pyenv activate code-journal; mkdocs serve;
```

```bash
# Source pyenv initialization script
export PATH="$HOME/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
cd /home/bearceb/Documents/code-journal/;
git pull
pyenv activate code-journal;
cd /home/bearceb/Documents/bbearce.github.io;
mkdocs gh-deploy --config-file ../code-journal/mkdocs.yml --remote-branch master;
```