# Namespace: `files`

The `files` namespace allows reading, writing, inspecting, and deleting local files and directories on disk.

---

## Methods

### `files.read`
Reads the entire contents of a file as a string.

```lua
files.read(path: string) -> string
```

#### Example
```lua
local content = files.read("driphook/scripts/custom_data.txt")
```

---

### `files.write`
Writes string data to a file. Overwrites existing contents.

```lua
files.write(path: string, data: string) -> void
```

---

### `files.create_folder`
Creates a directory path recursively.

```lua
files.create_folder(path: string) -> void
```

---

### `files.exists`
Checks whether a file or directory exists.

```lua
files.exists(path: string) -> boolean
```

---

### `files.delete_file`
Deletes a file on disk. Returns `true` if successfully removed.

```lua
files.delete_file(path: string) -> boolean
```

---

### `files.list_dir`
Returns an array of filename strings contained within a directory.

```lua
files.list_dir(directory_path: string) -> table
```

#### Example
```lua
local script_files = files.list_dir("driphook/scripts")
for i, name in ipairs(script_files) do
    print(i .. ": " .. name)
end
```

---

### `files.is_directory`
Returns whether a given path is a directory.

```lua
files.is_directory(path: string) -> boolean
```

---

### `files.file_size`
Returns the size of a file in bytes.

```lua
files.file_size(path: string) -> integer
```
