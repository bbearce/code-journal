# Run On Startup


> Proper instructions aren't working so I made a ```run.sh``` script that I can run from ```/home/bbearce```
```bash
#!/bin/bash

### BB - 01/11/2020 - Add startup commands here ###


# Mounting via sshfs
if [ "$1" == mount ]; then
  sshfs bbearce@medici-codalab-master.eastus.cloudapp.azure.com:/home/bbearce/srr
c/MedICI/codalab-competitions /home/bbearce/mounts/medici;

  sshfs azureuser@cbibop3.cloudapp.net:/home/azureuser/codalab-competitions /homm
e/bbearce/mounts/cbibop3;

# Postgres and pgAdmin
elif [ "$1" == postgres ] || [ "$1" == pg ]; then

  cd /home/bbearce/pgAdmin4/pgAdmin4;
  . bin/activate;
  python lib/python2.7/site-packages/pgadmin4/pgAdmin4.py;

elif [ "$1" == code-journal ]; then
  cd /home/bbearce/Documents/code-journal;
  . venv/bin/activate;
  mkdocs serve;

elif [ "$1" == cj-commit ]; then
  git add /home/bbearce/Documents/code-journal/.;
  git commit -m ""$2"";
  echo "$2"
  git push origin master;
  cd /home/bbearce/Documents/code-journal;
  . venv/bin/activate;
  cd /home/bbearce/Documents/bbearce.github.io;
  mkdocs gh-deploy --config-file ../code-journal/mkdocs.yml --remote-branch mastt
er;

elif [ "$1" == key_github ] || [ "$1" == gk ]; then
  # add github ssh key to agent
  eval "$(ssh-agent -s)";
  ssh-add ~/.ssh/github;


elif [ "$1" == qtim-site ] || [ "$1"  == qs ]; then
  cd /home/bbearce/Documents/qtim-lab.github.io;
  bundle exec jekyll serve;

elif [ "$1" == qs-commit ]; then
  cd /home/bbearce/Documents/qtim-lab.github.io;
  git add .;
  git commit -m ""$2"";
  git push origin master;

elif [ "$1" == thegratefulbrauer ] || [ "$1"  == tgb ]; then
  cd /home/bbearce/Documents/thegratefulbrauer;
  . venv3.6/bin/activate;
  python app.py;

else
  echo "not sure what you want..."

fi


### BB - Add startup commands here ###
```


> Courtesy of [ghacks](https://www.ghacks.net/2009/04/04/get-to-know-linux-the-etcinitd-directory/)

Find file ```/etc/rc.local```. You can place scripts that you want to run at startup.

*/etc/rc.local*
```bash
#!/bin/sh -e
#
# rc.local
#
# This script is executed at the end of each multiuser runlevel.
# Make sure that the script will "exit 0" on success or any other
# value on error.
#
# In order to enable or disable this script just change the execution
# bits.
#
# By default this script does nothing.



### BB - 01/11/2020 - Add startup commands here ###

# Mounting via sshfs
sshfs bbearce@medici-codalab-master.eastus.cloudapp.azure.com:/home/bbearce/src/MedICI/codalab-competitions /home/bbearce/mounts/medici;
sshfs azureuser@cbibop3.cloudapp.net:/home/azureuser/codalab-competitions /home/bbearce/mounts/cbibop3;

# add github ssh key to agent
eval "$(ssh-agent -s)";
ssh-add ~/.ssh/github;

### BB - Add startup commands here ###


exit 0

```