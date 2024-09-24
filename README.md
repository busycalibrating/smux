smux
====

smux is a wrapper around slurm and tmux. It allows tmux sessions to
run on a compute node under a slurm job and takes care of deternining 
which compute node the job is on and running ssh. Its supposed to be simliar to
tmux, but with slurm job ids or names instead of tmux session names.

It replaces sinteractive.
smux supports a lot of slurm sbatch options but not all of them. You can add them
pretty easily by looking at the code, I've only added the ones asked for


Installation
============

`pip install https://gitlab.erc.monash.edu.au/hpc-team/smux.git`

You probably want to create a virutalenv (say /usr/local/smux/0.0.1) before you do this
If you don't increment the version in setup.py you may want to 

`pip install --upgrade --force-reinstall https://gitlab.erc.monash.edu.au/hpc-team/smux.git`


Deployment
==========

I'm basing this off of the guide at https://gitlab.erc.monash.edu.au/hpc-team/clusterinfo.

First ensure the `version` has been updated in `setup.py`, and that the corresponding changes have been merged into the `master` branch.

Then run the following commands, ensuring the version you set here matches that in `setup.py`.

```bash
VENV=/usr/local/smux/0.0.5

# Create virtual environment and install latest smux into it
python -m venv $VENV
. $VENV/bin/activate
pip install --upgrade pip
pip install "git+ssh://git@gitlab.erc.monash.edu.au/hpc-team/smux.git"

# Update shebang to ignore user's environment, ensures system python is always used
sed -i '1 s/^\(#!.*python3\?\)$/\1 -E/' $VENV/bin/smux
```

At this stage, you should test that this is working by opening a new shell, and
testing it with e.g.
```bash
VENV=/usr/local/smux/0.0.5
$VENV/bin/smux new-session
```

Once you are happy with that, you can confirm the deployment with
```bash
VENV=/usr/local/smux/0.0.5
ln -s $VENV/bin/smux /usr/local/bin/smux
```