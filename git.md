For pushing a branch to remote (origin)  turn off tracking first (right click) then you can push the branch (smartgit) is checked out or right click on local bbranch to push branch if it is not.

need to add the open command to everyone's smartgit for submline in that directory.

post-receive hook script on a windows box
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