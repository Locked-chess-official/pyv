# PYV

## Install

```sh
pip install pyv_file_running
```

## Usage

There is a new file extension `.pyv` which is used to write python code with conditional compilation.

```pyv
print("start")

$ version <= 3.7; platform == "win32"
print("old windows")

$ version == 3.8
print("python 3.8")

$ platform == "win32"
print("windows")

$ _
print("other")

$

s = f"""{
$ version <= 9.99
"this is contain"
$
}
"""

ss = """
$ version == 9.99
this is still here
$
"""

print("end")

```

The special syntax `$` is used to write conditional compilation code.

The syntax is as follows:

```pyv
$ condition
code
$
```

The condition only supports "version", "platform", and "implement" three keywords, and supports the following operators: `==`, `!=`, `<=`, `>=`, `<`, `>`. For "platform" and "implement", it only support "==" or "!=". The version must contain the major and minor version number (e.g. 3.7).

The "$ _" means that if the condition above are not satisfied, the code below will be analyse.

The "$" means the end of the analyse.

If the syntan appeared in string, it will be ignored.

## Comment

You can also use "\#" to write comments.

```pyv
$ version <= 3.7; platform == "win32" # this is a comment
code
$
```

## Exec the file

You can use this module to exec it:

```python
import pyv
global_dict = {}
local_dict = {}
pyv.exec_pyv_file("test.pyv", global_dict, local_dict)
```

## Set a new environment

You can set a new environment:

```python
import pyv
pyv.set_new_env(version=(3, 8), platform="win32", implement="cpython")
```

Then you can use this environment in the `.pyv` file:

```python
import pyv
new_env = pyv.set_new_env(version=(3, 8), platform="win32", implement="cpython")
with open("test.pyv", "r") as f:
    code = f.read()
final_code = pyv.preprocess_pyv(code, new_env)
with open("test.py", "w") as f:
    f.write(final_code)
```

## All apis

```python
pyv.set_new_env(version: tuple[int, int] | None=None, platform: str | None=None, implement: str | None=None) -> dict[str, Any]: ...
pyv.preprocess_pyv(source: str, env=None) -> str: ...
pyv.exec_pyv_source(source: str, filename="<pyv>", globals=None, locals=None, env=None) -> None: ...
pyv.exec_pyv_file(path: str, globals=None, locals=None, env=None) -> None: ...
```

## License

MIT
