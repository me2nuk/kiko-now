---
layout: post
title:  "Python sys module Guide"
tags: [Guide,Python]
excerpt: "Python sys 모듈 사용법 ( Guide )"
---
---

## sys.abiflags

---

> [Python docs](https://docs.python.org/3/library/sys.html#sys.abiflags)

sys.abiflags는 POSIX 시스템에서만 존재하며, [PEP 3149 Proposal](https://peps.python.org/pep-3149/#proposal)에 나와 있는 것 처럼 인터프리어 빌드 시 옵션에 따라 플래그가 달라집니다. 기본 Default 값은 비어있습니다.

\-\-with-pydebug : d<br>
\-\-with-pymalloc : m<br>
\-\-with-wide-unicode : u

###### Linux Python interpreter

```py
>>> import sys
>>> sys.abiflags
''
```

###### Python Interpreter Build with \-\-with\-debug Option Use

```py
>>> import sys
>>> sys.abiflags
'm'
```

###### Windows Python Interpreter

```py
>>> import sys
>>> 'abiflags' in dir(sys)
False
>>> sys.abiflags
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: module 'sys' has no attribute 'abiflags'
```

### sya.addaudithook(hook: Callable[[str, tuple]])

> [Python docs](https://docs.python.org/3/library/sys.html#sys.addaudithook)

아래의 Audit 이벤트 테이블에 있는 것 처럼 Python의 이벤트에 대한 Hook를 추가할 수 있습니다.

[Audit Event Table](https://docs.python.org/3/library/audit_events.html#audit-events-table)

```py
>>> import sys
>>> sys.addaudithook(lambda event, args: print(event, args))
compile (None, '<stdin>')
>>> f = open('/etc/passwd')
exec (<code object <module> at 0x7ff6239faa80, file "<stdin>", line 1>,)
open ('/etc/passwd', 'r', 524288)
compile (None, '<stdin>')
>>> f.read()
exec (<code object <module> at 0x7ff6239faa80, file "<stdin>", line 1>,)
'root:x:0:0:root:/root:/bin/bash[ ... ]\n'
compile (None, '<stdin>')
```

### sys.argv

> [Python docs](https://docs.python.org/3/library/sys.html#sys.argv)

Python이 실행되 때 전달된 인수 즉 옵션을 리스트로 반환 합니다.

```bash
me2nuk@LAPTOP-FICGSDFP:~$ python3 -c "import sys;print(sys.argv)"
['-c']
me2nuk@LAPTOP-FICGSDFP:~$ 
me2nuk@LAPTOP-FICGSDFP:~$ cat argv.py
import sys
print(sys.argv)
me2nuk@LAPTOP-FICGSDFP:~$ python3 argv.py 1 2 3 4 5
['argv.py', '1', '2', '3', '4', '5']
me2nuk@LAPTOP-FICGSDFP:~$ python3 argv.py --file=filename -d
['argv.py', '--file=filename', '-d']
```

### sys.audit(str, *args)

> [Python docs](https://docs.python.org/3/library/sys.html#sys.audit)

audit 함수는 addaudithook 

```py
me2nuk@LAPTOP-FICGSDFP:~$ cat child.py
a = 'hello'
b = 'world'
me2nuk@LAPTOP-FICGSDFP:~$ python3
Python 3.8.10 (default, Mar 15 2022, 12:22:08)
[GCC 9.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import sys
>>> sys.addaudithook(lambda event, args: print(event, args))
compile (None, '<stdin>')
>>> sys.audit("argument", "1", "2", "3")
exec (<code object <module> at 0x7f7c7b07ea80, file "<stdin>", line 1>,)
argument ('1', '2', '3') # highlight
compile (None, '<stdin>')
```

```py
static PyObject *
import_find_and_load(PyObject *abs_name)
{
    _Py_IDENTIFIER(_find_and_load);
    PyObject *mod = NULL;
    PyInterpreterState *interp = _PyInterpreterState_GET_UNSAFE();
    int import_time = interp->config.import_time;
    static int import_level;
    static _PyTime_t accumulated;

    _PyTime_t t1 = 0, accumulated_copy = accumulated;

    PyObject *sys_path = PySys_GetObject("path");
    PyObject *sys_meta_path = PySys_GetObject("meta_path");
    PyObject *sys_path_hooks = PySys_GetObject("path_hooks");
    if (PySys_Audit("import", "OOOOO",
                    abs_name, Py_None, sys_path ? sys_path : Py_None,
                    sys_meta_path ? sys_meta_path : Py_None,
                    sys_path_hooks ? sys_path_hooks : Py_None) < 0) {
        return NULL;
    }


    /* XOptions is initialized after first some imports.
     * So we can't have negative cache before completed initialization.
     * Anyway, importlib._find_and_load is much slower than
     * _PyDict_GetItemIdWithError().
     */
    if (import_time) {
        static int header = 1;
        if (header) {
            fputs("import time: self [us] | cumulative | imported package\n",
                  stderr);
            header = 0;
        }

        import_level++;
        t1 = _PyTime_GetPerfCounter();
        accumulated = 0;
    }

    if (PyDTrace_IMPORT_FIND_LOAD_START_ENABLED())
        PyDTrace_IMPORT_FIND_LOAD_START(PyUnicode_AsUTF8(abs_name));

    mod = _PyObject_CallMethodIdObjArgs(interp->importlib,
                                        &PyId__find_and_load, abs_name,
                                        interp->import_func, NULL);

    if (PyDTrace_IMPORT_FIND_LOAD_DONE_ENABLED())
        PyDTrace_IMPORT_FIND_LOAD_DONE(PyUnicode_AsUTF8(abs_name),
                                       mod != NULL);

    if (import_time) {
        _PyTime_t cum = _PyTime_GetPerfCounter() - t1;

        import_level--;
        fprintf(stderr, "import time: %9ld | %10ld | %*s%s\n",
                (long)_PyTime_AsMicroseconds(cum - accumulated, _PyTime_ROUND_CEILING),
                (long)_PyTime_AsMicroseconds(cum, _PyTime_ROUND_CEILING),
                import_level*2, "", PyUnicode_AsUTF8(abs_name));

        accumulated = accumulated_copy + cum;
    }

    return mod;
}
```

### sys.base_exec_prefix

> [Python docs](https://docs.python.org/3/library/sys.html#sys.base_exec_prefix)

### sys.base_prefix

> [Python docs](https://docs.python.org/3/library/sys.html#sys.base_prefix)

### sys.byteorder

> [Python docs](https://docs.python.org/3/library/sys.html#sys.byteorder)

### sys.builtin_module_names

> [Python docs](https://docs.python.org/3/library/sys.html#sys.builtin_module_names)

### sys.call_Tracing

> [Python docs](https://docs.python.org/3/library/sys.html#sys.call_Tracing)

### sys.copyright

> [Python docs](https://docs.python.org/3/library/sys.html#sys.copyright)

### sys._clear_type_cache()

> [Python docs](https://docs.python.org/3/library/sys.html#sys._clear_type_cache)

### sys._current_frames()

> [Python docs](https://docs.python.org/3/library/sys.html#sys._current_frames)

### sys._current_exceptions()

> [Python docs](https://docs.python.org/3/library/sys.html#sys._current_exceptions)

### sys.breakpointhook()

> [Python docs](https://docs.python.org/3/library/sys.html#sys.breakpointhook)

### sys._debugmallocstats()

> [Python docs](https://docs.python.org/3/library/sys.html#sys._debugmallocstats)

### sys.dllhandle

> [Python docs](https://docs.python.org/3/library/sys.html#sys.dllhandle)

### sys.displayhook(value)

> [Python docs](https://docs.python.org/3/library/sys.html#sys.displayhook)

### sys.dont_write_bytecode

> [Python docs](https://docs.python.org/3/library/sys.html#sys.dont_write_bytecode)

### sys.pycache_prefix

> [Python docs](https://docs.python.org/3/library/sys.html#sys.pycache_prefix)

### sys.excepthook()

> [Python docs](https://docs.python.org/3/library/sys.html#sys.excepthook)

### sys.__breakpointhook__, __display__hook, __excepthook__, __unraiseblehook__

> [Python docs](https://docs.python.org/3/library/sys.html#sys.__breakpointhook__)

### sys.exc_info()

> [Python docs](https://docs.python.org/3/library/sys.html#sys.exc_info)

### sys.exec_prefix

> [Python docs](https://docs.python.org/3/library/sys.html#sys.exec_prefix)

### sys.executable

> [Python docs](https://docs.python.org/3/library/sys.html#sys.executable)

### sys.exit()

> [Python docs](https://docs.python.org/3/library/sys.html#sys.exit)

### sys.flags

> [Python docs](https://docs.python.org/3/library/sys.html#sys.flags)

### sys.float_info

> [Python docs](https://docs.python.org/3/library/sys.html#sys.float_info)

### sys.float_repr_style

> [Python docs](https://docs.python.org/3/library/sys.html#sys.float_repr_style)

### sys.getallocatedblocks()

> [Python docs](https://docs.python.org/3/library/sys.html#sys.getallocatedblocks)

### sys.getandroidapilevel()

> [Python docs](https://docs.python.org/3/library/sys.html#sys.getandroidapilevel)

### sys.getdefaultencoding()

> [Python docs](https://docs.python.org/3/library/sys.html#sys.getdefaultencoding)

### sys.getdlopenflags()

> [Python docs](https://docs.python.org/3/library/sys.html#sys.getdlopenflags)

### sys.getfilesystemencoding()

> [Python docs](https://docs.python.org/3/library/sys.html#sys.getfilesystemencoding)

### sys.get_int_max_str_digits()

> [Python docs](https://docs.python.org/3/library/sys.html#sys.get_int_max_str_digits)

### sys.getrefcount()

> [Python docs](https://docs.python.org/3/library/sys.html#sys.getrefcount)

### sys.getrecursionlimit()

> [Python docs](https://docs.python.org/3/library/sys.html#sys.getrecursionlimit)

### sys.getsizeof()

> [Python docs](https://docs.python.org/3/library/sys.html#sys.getsizeof)

### sys.getswitchinterval()

> [Python docs](https://docs.python.org/3/library/sys.html#sys.getswitchinterval)

### sys._getframe()

> [Python docs](https://docs.python.org/3/library/sys.html#sys._getframe)

### sys.getprofile()

> [Python docs](https://docs.python.org/3/library/sys.html#sys.getprofile)

### sys.gettrace()

> [Python docs](https://docs.python.org/3/library/sys.html#sys.gettrace)

### sys.getwindowsversion()

> [Python docs](https://docs.python.org/3/library/sys.html#sys.getwindowsversion)

### sys.get_asyncgen_hookos()

> [Python docs](https://docs.python.org/3/library/sys.html#sys.get_asyncgen_hookos)

### sys.getcoroutine_origin_tracking_depth()

> [Python docs](https://docs.python.org/3/library/sys.html#sys.getcoroutine_origin_tracking_depth)

### sys.hash_info

> [Python docs](https://docs.python.org/3/library/sys.html#sys.hash_info)

### sys.hexversion

> [Python docs](https://docs.python.org/3/library/sys.html#sys.hexversion)

### sys.implementation

> [Python docs](https://docs.python.org/3/library/sys.html#sys.implementation)

### sys.int_info

> [Python docs](https://docs.python.org/3/library/sys.html#sys.int_info)

### sys.__interactivehook__

> [Python docs](https://docs.python.org/3/library/sys.html#sys.__interactivehook__)

### sys.intern

> [Python docs](https://docs.python.org/3/library/sys.html#sys.intern)

### sys.is_finalizing()

> [Python docs](https://docs.python.org/3/library/sys.html#sys.is_finalizing)

### sys.last_type, last_value, last_traceback

> [Python docs](https://docs.python.org/3/library/sys.html#sys.last_type)

### sys.maxsize

> [Python docs](https://docs.python.org/3/library/sys.html#sys.maxsize)

### sys.maxunicode

> [Python docs](https://docs.python.org/3/library/sys.html#sys.maxunicode)

### sys.meta_path

> [Python docs](https://docs.python.org/3/library/sys.html#sys.meta_path)

### sys.modules

> [Python docs](https://docs.python.org/3/library/sys.html#sys.modules)

### sys.orig_argv

> [Python docs](https://docs.python.org/3/library/sys.html#sys.orig_argv)

### sys.path

> [Python docs](https://docs.python.org/3/library/sys.html#sys.path)

### sys.path_hooks

> [Python docs](https://docs.python.org/3/library/sys.html#sys.path_hooks)

### sys.path_importer_cache

> [Python docs](https://docs.python.org/3/library/sys.html#sys.path_importer_cache)

### sys.platform

> [Python docs](https://docs.python.org/3/library/sys.html#sys.platform)

### sys.platlibdir

> [Python docs](https://docs.python.org/3/library/sys.html#sys.platlibdir)

### sys.prefix

> [Python docs](https://docs.python.org/3/library/sys.html#sys.prefix)

### sys.ps1, ps2

> [Python docs](https://docs.python.org/3/library/sys.html#sys.ps1, ps2)

### sys.setdlopenflags()

> [Python docs](https://docs.python.org/3/library/sys.html#sys.setdlopenflags)

### sys.set_int_max_str_digits()

> [Python docs](https://docs.python.org/3/library/sys.html#sys.set_int_max_str_digits)

### sys.setprofile()

> [Python docs](https://docs.python.org/3/library/sys.html#sys.setprofile)

### sys.setrecursionlimit()

> [Python docs](https://docs.python.org/3/library/sys.html#sys.setrecursionlimit)

### sys.setswitchinterval()

> [Python docs](https://docs.python.org/3/library/sys.html#sys.setswitchinterval)

### sys.settrace()

> [Python docs](https://docs.python.org/3/library/sys.html#sys.settrace)

### sys.set_asyncgen_hooks()

> [Python docs](https://docs.python.org/3/library/sys.html#sys.set_asyncgen_hooks)

### sys.set_coroutine_origin_tracking_depth()

> [Python docs](https://docs.python.org/3/library/sys.html#sys.set_coroutine_origin_tracking_depth)

### sys._enablelegacywindowsfsencoding()

> [Python docs](https://docs.python.org/3/library/sys.html#sys._enablelegacywindowsfsencoding)

### sys.stdin, stdout, stderr

> [Python docs](https://docs.python.org/3/library/sys.html#sys.stdin)

### sys.__stdin__, __stdout__, __stderr__

> [Python docs](https://docs.python.org/3/library/sys.html#sys.__stdin__)

### sys.stdlib_module_names

> [Python docs](https://docs.python.org/3/library/sys.html#sys.stdlib_module_names)

### sys.thread_info

> [Python docs](https://docs.python.org/3/library/sys.html#sys.thread_info)

### sys.tracebacklimit

> [Python docs](https://docs.python.org/3/library/sys.html#sys.tracebacklimit)

### sys.unraisablehook()

> [Python docs](https://docs.python.org/3/library/sys.html#sys.unraisablehook)

### sys.version

> [Python docs](https://docs.python.org/3/library/sys.html#sys.version)

### sys.api_version

> [Python docs](https://docs.python.org/3/library/sys.html#sys.api_version)

### sys.version_info

> [Python docs](https://docs.python.org/3/library/sys.html#sys.version_info)

### sys.warnoptions

> [Python docs](https://docs.python.org/3/library/sys.html#sys.warnoptions)

### sys.winver

> [Python docs](https://docs.python.org/3/library/sys.html#sys.winver)

### sys._xoptions

> [Python docs](https://docs.python.org/3/library/sys.html#sys._xoptions)
