# Quick Notes

## Deploy the server\site

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

## Detailed Github Notes

[github deploy](https://www.mkdocs.org/user-guide/deploying-your-docs/#readthedocs)

## Note for the venv

```pip freeze``` lists these packages:

```bash
Click==7.0
Jinja2==2.10.1
livereload==2.6.1
Markdown==3.1.1
MarkupSafe==1.1.1
mkdocs==1.0.4
mkdocs-rtd-dropdown==1.0.2
mkdocs-windmill-dark==0.2.0
PyYAML==5.1.2
six==1.12.0
tornado==6.0.3
```
I am pretty sure I only installed ```mkdocs``` and ```mkdocs-windmill-dark```