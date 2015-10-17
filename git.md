
#GIT

##Install

On Linux add the ppa Git core linux
https://launchpad.net/~git-core/+archive/ubuntu/ppa
ppa:git-core/ppa
then update and then install

For windows use download
https://git-scm.com/download/win


##SmartGit

Lots of GUIs out this.  This is my preferred one and is cross platform since it's java based.  http://www.syntevo.com/smartgit/
setting up some launch options from the context menu is helpful

##Gitolite

With Git installed, make a "git" user.  Login as that user "git"
in the home/git folder clone the gitolite repo, git://github.com/sitaramc/gitolite.  instead of linking gitolite/install to the local /bin directory link to /usr/etc/bin.  (need to do that as sudo so need "git" user to be in sudoers group).

Before installing gitolite put the public key of the user you want to be the gitolite admin in the ~/.ssh.  I made a keypair Git-Admin and use that.  Make sure the authorized_keys file is empty.  

Ready.  Run `gitolite setup -pk your-ssh-key.pub`

Now from your local machine (with the private key from the keypair you just used) clone the git@<IP or URL of Server>:gitolite-admin

From that repo you add other user keys and add repos and user access in the config file, then just commit and push and you have new repos and can control access all from a this git repo!




###post-receive hook 

#### on a windows box
on windows need to use bin/sh not bin/bash and also add git-core path 

    #!/bin/sh
    # set git core path if bare git repo is on windows with mysgit
    export PATH="/c/Program Files (x86)/Git/libexec/git-core":$PATH
    echo "===== DEPLOYING TO LIVE SITE ====="
    REPO_PATH="/d/webs/645-system"
    unset GIT_DIR
    cd $REPO_PATH || exit
    echo "move to repo at"
    pwd
    echo " fetch and merge master from bare origin"
    git fetch origin 
    git merge origin/master
    echo "===== DONE ====="


for git repo on linux box I use a modified script form github.  Filename is `post-receive` and goes in the .git/hooks folder and you can't put in the the local repo as it won't copy with push.  Must put it directly in the bare repo's git/hooks folder on the "server"

````
#!/bin/bash
#
# Original Author: "FRITZ Thomas" <fritztho@gmail.com> (http://www.fritzthomas.com)
# GitHub: https://gist.github.com/thomasfr/9691385
#

# Application Name:
export DEPLOY_APP_NAME=`hw-admin-info`

# This is the root deploy dir.
export DEPLOY_ROOT="/var/www/admin/private/admin-info"

# When receiving a new git push, the received branch gets compared to this one.
# If you do not need this, just add a comment
export DEPLOY_ALLOWED_BRANCH="master"

# You could use this to do a backup before updating to be able to do a quick rollback. 
# If you need this just delete the comment and modify to your needs
#PRE_UPDATE_CMD='cd ${DEPLOY_ROOT} && backup.sh'

# Use this to do update tasks and maybe service restarts
# If you need this just delete the comment and modify to your needs
# POST_UPDATE_CMD='cd ${DEPLOY_ROOT} && pwd && chmod -R 644 .'

###########################################################################################

export GIT_DIR="$(cd $(dirname $(dirname $0));pwd)"
export GIT_WORK_TREE="${DEPLOY_ROOT}"
IP="$(ip addr show eth0 | grep 'inet ' | cut -f2 | awk '{ print $2}')"

echo "githook: $(date): Welcome to '$(hostname -f)' (${IP})"
echo

# Make sure directory exists. Maybe its deployed for the first time.
umask 022
mkdir -p "${DEPLOY_ROOT}"


# Loop, because it is possible to push more than one branch at a time. (git push --all)
while read oldrev newrev refname
do

    export DEPLOY_BRANCH=$(git rev-parse --symbolic --abbrev-ref $refname)
    export DEPLOY_OLDREV="$oldrev"
    export DEPLOY_NEWREV="$newrev"
    export DEPLOY_REFNAME="$refname"

    if [ ! -z "${DEPLOY_ALLOWED_BRANCH}" ]; then
        if [ "${DEPLOY_ALLOWED_BRANCH}" != "$DEPLOY_BRANCH" ]; then
            echo "githook: Branch '$DEPLOY_BRANCH' of '${DEPLOY_APP_NAME}' application will not be deployed. Exiting."
            exit 1
        fi
    fi

    if [ ! -z "${PRE_UPDATE_CMD}" ]; then
       echo
       echo "githook: PRE UPDATE (CMD: '${PRE_UPDATE_CMD}'):"
       eval $PRE_UPDATE_CMD || exit 1
    fi

    # Make sure GIT_DIR and GIT_WORK_TREE is correctly set and 'export'ed. Otherwhise
    # these two environment variables could also be passed as parameters to the git cli
    echo "githook: I will deploy '${DEPLOY_BRANCH}' branch of the '${DEPLOY_APP_NAME}' project to '${DEPLOY_ROOT}'"
    git checkout -f "${DEPLOY_BRANCH}" || exit 1
    git reset --hard "$DEPLOY_NEWREV" || exit 1

    if [ ! -z "${POST_UPDATE_CMD}" ]; then
       echo
       echo "githook: POST UPDATE (CMD: '${POST_UPDATE_CMD}'):"
       eval $POST_UPDATE_CMD || exit 1
    fi

done

echo
echo "githook: $(date): Wiki Admin Info for '$(hostname -f)' successfully deployed (${IP})"
exit 0
````



## Tips

For pushing a branch to remote (origin)  turn off tracking first (right click) then you can push the branch (smartgit) is checked out or right click on local branch to push branch if it is not.