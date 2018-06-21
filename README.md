#Virtual Environment
A Virtual Environment, put simply, is an isolated working copy of Python which
allows you to work on a specific project without worry of affecting other projects

It enables multiple side-by-side installations of Python, one for each project.

It doesn’t actually install separate copies of Python, but it does provide a
clever way to keep different project environments isolated.

###Install Virtualenv:
    pip install virtualenv

####Create Virtualenv:
    virtualenv env_name
    >with default Python

    virtualenv -p /usr/bin/python3 env_name
    >with Python3

###Active your virtual environment:    
    source env_name/bin/activate

###Deactivate:
    deactivate
