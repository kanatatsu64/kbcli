# kbcli
CLI client for Elasticsearch

## PREREQUISITE
- bash version > 4
- `curl` command
- `jq` command

## INSTALLATION

```shell
git clone https://github.com/kanatatsu64/kbcli
cd kbcli
cp kbcli <your bin folder>
```
## USAGE

```shell
kbcli [<command>] <url> <index> <type> [<command specific arguments>]
```

If `<command>` is not specified, `find` is used.

### Note
When you use the commands which read query body from standard input (`search`, `analyze`), you can specify the body either with HERE document or with file redirect.

- Here document
```shell
kbcli <command> <url> <index> <type> [<command specific arguments>] <<'EOF'
<query body>
EOF
```

- File redirect
```shell
kbcli <command> <url> <index> <type> [<command specific arguments>] < <query body file>
```

## ALIAS
You can make aliases for `<url>` and `<index> <type>`.
Aliases are defined at the top of `kbcli` executable file.

### url ALIAS
In order to define a url alias, add a item to `URL` hash.
The addition should be done below the `URL setting` comment.

- #### example
```shell
# URL settings
URL["EXAMPLE"]="https://example.com"
```

With this definition, you can now use `EXAMPLE` alias for url. For example,
```shell
kbcli find EXAMPLE documents example-documents 1
```
is equivalent to
```shell
kbcli find https://example.com documents example-documents 1
```

### index & type ALIAS
In order to define a index & type alias, add a item to `DOCUMENT` hash.
The addition should be done below the `Document setting` comment.

- #### example
```shell
# Document settings
DOCUMENT["Document", "index"]="example-documents"
DOCUMENT["Document", "type"]="documents"
```
With this definition, you can now use `Document` alias for url. For example,
```shell
kbcli find https://example.com Document 1
```
is equivalent to
```shell
kbcli find https://example.com documents example-documents 1
```

## COMMANDS

### search
```shell
kbcli search <url> <index> <type> <<'EOF'
<your query>
EOF
```

- #### query
```
POST url/index/type/_search
<your query>
```

- #### example
```shell
kbcli search https://example.com documents example-documents <<'EOF'
{
  "query": {
    "term": {
      "name": "test"
    }
  }
}
EOF
```

### find
```shell
kbcli [find] <url> <index> <type> <id> [<sources>]
```

`sources` is a comma separated list of \_source name

- #### query
```
GET url/index/type/id?_source=sources
```

- #### example
```shell
kbcli find https://example.com documents example-documents 1 "name,author"
```

### show
```shell
kbcli show <url> <index> <type>
```

- #### query
```
<no query>
```

- #### example
```shell
kbcli show https://example.com documents example-documents

# output: https://example.com/documents/example-documents
```

### analyze
```shell
kbcli analyze <url> <index> <type> <<'EOF'
<your analyze>
EOF
```

- #### query
```
POST url/index/_analyze
<your analyze>
```

- #### example
```shell
kbcli analyze https://example.com documents example-documents <<'EOF'
{
  "analyze": "standard"
}
EOF
```

### mapping / mappings
```shell
kbcli mapping <url> <index> <type>
```

- #### query
```
GET url/index/_mappings
```

- #### example
```shell
kbcli mapping https://example.com documents example-documents
```

### setting / settings
```shell
kbcli setting <url> <index> <type>
```

- #### query
```
GET url/index/_settings
```

- #### example
```shell
kbcli setting https://example.com documents example-documents
```
