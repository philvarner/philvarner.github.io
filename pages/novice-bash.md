Bash


Start scripts with:
```
#!/bin/bash
set -Eeuxo pipefail
```

* -E : fire ERR traps correctly
* -e : exit immediately on command failure. Append '|| true' or execute in conditional to disable.
* -u : unset variables trigger failure
* -x : print each command before execution
* -o pipefail : don't prevent piped results from masking non-zero exit code

```
FOO=${VARIABLE:-default} 
VARIABLE=${1:-DEFAULTVALUE}
```

```
RESULT=$(ls)
```

Integer comparison: -eq -ne -gt -ge -lt -le < <= > >= 

String comparison: = == != < > -z (== '') -n (!= '')

File test: -e exists,

```
exec 5>&1
FF=$(echo aaa|tee >(cat - >&5))
echo $FF
```


```
if [ -z "${MY_VAR:-}" ]; then
  echo "MY_VAR was not set"
fi
```

```
function prt
{
  echo "[$(date +"%Y-%m-%d %H:%M:%S")] - $1"
}
```

```
if [ -z $(aws s3 ls ${HUED_URI}) ]; then
    exit 1
fi
```



* https://www.topbug.net/blog/2013/04/14/install-and-use-gnu-command-line-tools-in-mac-os-x/
* https://www.topbug.net/blog/2017/07/31/inputrc-for-humans/
* http://tldp.org/LDP/abs/html/exit-status.html
* https://vaneyckt.io/posts/safer_bash_scripts_with_set_euxo_pipefail/
* https://ryanstutorials.net/bash-scripting-tutorial/    