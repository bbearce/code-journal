# Impersonate

To impersonate a user on a linux machine with docker, do this:

```bash
# Become someone else
WHO_DO_YOU_WANT_TO_BE=bearceb;

# Raw users
echo $WHO_DO_YOU_WANT_TO_BE
export UID_faked=$(id -u $WHO_DO_YOU_WANT_TO_BE);
export GID_faked=$(id -g $WHO_DO_YOU_WANT_TO_BE);

echo $UID_faked
echo $GID_faked

# Get group id by name if different group
group_name="soms-slce-oph1-users"
GID_faked=$(getent group "$group_name" | cut -d: -f3)
echo $GID_faked

## It's best to not mount / but a specific directory and laser focus on what you're doing
VOL_OF_INTEREST=/projects/Chronicle/couchdb_data;

# Another tutorial mentioned using the --user to attach as this user but 
# That user doesn't exist inside with mounting other directories ... /etc/gropups /etc/passwd...etc
# I think we need to build the image with this user in there or add them and edit uid and gid to 
# match local. It's too complex...or just overly so.
# Ex: --user $UID_faked:$GID_faked \

# Below is a better way:
docker run -it --rm \
    --workdir="$VOL_OF_INTEREST" \
    --volume="$VOL_OF_INTEREST:$VOL_OF_INTEREST" \
    -e UID_faked=$UID_faked \
    -e GID_faked=$GID_faked \
    ubuntu:latest bash

# Check them
echo $UID_faked
echo $GID_faked

# Now we can just change owner to the actual ids, not names
chown $UID_faked:$GID_faked /projects/Chronicle/couchdb_data
```