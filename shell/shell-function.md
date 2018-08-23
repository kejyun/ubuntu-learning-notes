# Shell Function 函式

## 函式命名

```shell
function function_name {
   command...
}
```

```shell
function_name () {
   command...
}
```

## 參數

shell 使用 `$1`、`$2` 去接收參數資料

```shell
foo() {
    echo "Parameter #1 is $1"
}
```

```shell
# Parameter #1 is KJ
foo KJ
```

## 參考資料
* [Passing parameters to a Bash function - Stack Overflow](https://stackoverflow.com/questions/6212219/passing-parameters-to-a-bash-function)
