The `SOURCES.SML` file contained in the [Methods distribution](https://archive.org/download/digitalk_202401/methods11.zip) is lightly compressed.  High ascii values are used in two ways:

1. Immediately following a newline character, the value indicates how many spaces to emit
2. Otherwise it serves as an index into an array of strings to emit

The decompression array can be recovered from within Methods by evaluating:

```
| file |
file := Disk newFile: (File fileName: 'tokens' extension: 'txt').
file lineDelimiter: 10 asCharacter.
DecompressionArray do: [ :each |
file nextPutAll: each; cr.
].
file close.
```

![Screenshot 2023-12-31 at 21 10 18](https://github.com/pablomarx/Methods/assets/179162/4b5c6172-8e1b-44d8-8af7-063cd6cd420f)


And the following Python code can be used to then decompress the sources:

```
codes = (
    'receiver', 'the', 'self', 'Answer', ' := ', ': ', 'String', 'Integer', 'Object', 'index', 'Stream', 'Collection', 'True', 'answer', 'Character', 'Rectangle',
    'position', 'class', 'Private', 'False', ': [', '."', 'instance', 'Point', 'origin', 'primitive', 'with', 'selected', 'corner', 'new', 'selection', 'if',
    'Block', 'Number', 'display', ' - ', 'Symbol', 'source', 'value', 'element', 'next', 'Index', 'an', 'of', 'collection', 'false', 'extent', 'Selection',
    'pane', 'and', 'error', ': [^', 'to', 'offset', 'number', 'Dictionary', 'size', 'Class', 'elements', 'file', 'Position', 'true', 'character', 'Directory',
    'Pane', 'dispatcher', ' + ', 'Cursor', 'Display', 'Name', 'stream', '.  ', 'directory', 'frame', 'Association', 'variable', 'Function', 'Dispatcher', ' == ', 'is',
    'basic', 'method', 'containing', 'Array', 'height', 'first', 'variables', 'name', 'attribute', 'at', 'initialize', ': [ :', 'current', 'representation', 'string', 'copy',
    'from', 'cursor', 'put', 'denominator', 'width', 'while', ': (', 'else', 'Form', 'replace', 'line', '].', 'nil', 'window', 'Put', 'for',
    'result', ' |', 'selectors', 'print', 'argument', 'top', 'remove', 'Size', 'object', 'Origin', 'File', 'changed', 'Method', 'From', 'start', '',
    'to', 'offset', 'number', 'Dictionary', 'size', 'Class', 'elements', 'file', 'Position', 'true', 'character', 'Directory', 'Pane', 'dispatcher', ' + ', 'Cursor',
    'Display', 'Name', 'stream', '.  ', 'directory', 'frame', 'Association', 'varia',
)

with open("SOURCES.SML", "rb") as file:
    output = ""
    data = bytearray(file.read())
    lf = True
    for idx in range(0, len(data)):
        byte = data[idx]
        if byte < 128:
            output += chr(byte)
            lf = (byte == 10)
        else:
            byte = byte - 128
            if lf:
                lf = False
                for i in range(0, byte):
                    output += ' ' 
            else:
                output += codes[byte-1]
    print(output)

```
