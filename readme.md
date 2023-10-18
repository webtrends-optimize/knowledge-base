# Installing the knowledge base locally

## Install python and pip

However works for your OS.

## Clear all dependencies and then install them.

```
pip uninstall -r requirements.txt -y
pip install -r requirements.txt
```

Note: On some devices like Mac, you might have it installed as pip3 instead of pip.

## Run the service 
```
python -m mkdocs serve
```

Note: On some devices like Mac, you might have it installed as python3 instead of python. 

## Preview in browser as you make changes

It'll tell you where the previews are found. But it should be:
http://127.0.0.1:9000/