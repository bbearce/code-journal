# Quick Notes

# Deploy the server\site

Be sure you are in ./code-journal/ or ./&lt;project root&gt;


```bash
code-journal  
  |-docs  
  |-site  
  |-mkdocs.yml  
```

```bash
Benjamins-MBP-2:code-journal bbearce$ pwd
/Users/bbearce/Documents/Code/code-journal
```

Use this code to serve the developer site
```bash
$ mkdocs serve
```

Use this code to build the site
```bash
$ mkdocs build
```

To push to github do this:
```bash
cd ../bbearce.github.io/
mkdocs gh-deploy --config-file ../code-journal/mkdocs.yml --remote-branch master
```

# Detailed Github Notes

[github deploy](https://www.mkdocs.org/user-guide/deploying-your-docs/#readthedocs)