# Confapp

Library to be used as a singleton to manages modules and applications configurations.


## How to use

Image an application that uses the modules 'module_x' and 'module_y' as depences, which one with their own default settings.

```
module-x
    ├── module_x
    │   ├── __init__.py
    │   ├── settings.py
    ├── setup.py
    
module-y
    ├── module_y
    │   ├── __init__.py
    │   ├── settings.py
    ├── setup.py

app
    ├── __init__.py
    ├── __main__.py
    ├── settings.py
```
    
module-x/module_x/settings.py
```python

MODULE_X_SETTINGS_0 = 'something'
MODULE_X_SETTINGS_1 = 1

```
module-x/module_x/\_\_init\_\_.py
```python

from confapp import conf; 
conf += 'module_x.settings'

class X:
	pass
```

module-x/module_x/settings.py
```python

MODULE_Y_SETTINGS_0 = 'else'
MODULE_Y_SETTINGS_1 = 2

```
module-y/module_y/\_\_init\_\_.py
```python

from confapp import conf;
from module_y import settings
conf += settings

class Y:
	pass
```


module-x/module_x/settings.py
```python
SETTINGS_PRIORITY = 0 # defines the priority of the settings. It can be used to overwrite third modules configurations.

MODULE_Y_SETTINGS_0 = 'overwritten'
MODULE_X_SETTINGS_0 = 'overwritten'
```


```python
from confapp import conf
conf += 'app.settings'

from module_x import X
from module_y import Y

print( conf.MODULE_X_SETTINGS_0 )
# out: overwritten
print( conf.MODULE_Y_SETTINGS_0 )
# out: overwritten

conf.MODULE_X_SETTINGS_0 = 'empty'

print( conf.MODULE_X_SETTINGS_0 )
# out: empty

```