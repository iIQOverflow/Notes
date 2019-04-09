# __init__.py
python包中必须含有`__init__.py`文件，否则导入包将会报错。开发者这么做的原因是担心后期写的模块与有效的模块构成冲突，为了缓解这种情况，Python会忽视包内没有`__init__.py`文件的包。

`__init__.py`文件的作用是将文佳佳变为一个Python模块，__init__.py主要控制包的导入行为。

通常`__init__.py `文件为空，但是我们还可以为它增加其他的功能。我们在导入一个包时，实际上是导入了它的`__init__.py`文件。这样我们可以在`__init__.py`文件中批量导入我们所需要的模块，而不再需要一个一个的导入。

```
# package
# __init__.py
import re
import urllib
import sys
import os

# a.py
import package 
print(package.re, package.urllib, package.sys, package.os)
```

注意这里访问`__init__.py`文件中的引用文件，需要加上包名。

`__init__.py`中还有一个重要的变量，`__all__`, 它用来将模块全部导入。

```
# __init__.py
__all__ = ['os', 'sys', 're', 'urllib']

# a.py
from package import *
```

## Reference
[What is __init__.py for?](https://stackoverflow.com/questions/448271/what-is-init-py-for)

[Python __init__.py 作用详解](https://www.cnblogs.com/Lands-ljk/p/5880483.html)