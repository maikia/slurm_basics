# configure git prompt

wget https://raw.githubusercontent.com/git/git/8976500cbbb13270398d3b3e07a17b8cc7bff43f/contrib/completion/git-prompt.sh
source ~/git-prompt.sh
PS1='[\u@\h \W$(__git_ps1 " (%s)")]\$ ' 

# add pythonpath

echo "export PYTHONPATH=$PYTHONPATH:/home/mtelenczuk/maja/code/neuroneap" >> ~/.bashrc
