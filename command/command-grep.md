# grep


## 計算行數

加入 `-c` 參數即可顯示行數

```shell
cat * | grep -c
```

**參考資料**

* [Count all occurrences of a string in lots of files with grep - Stack Overflow](https://stackoverflow.com/questions/371115/count-all-occurrences-of-a-string-in-lots-of-files-with-grep)


## 搜尋目錄下指定字串

```
grep -r "[STRING TO SEARCH FOR]" "[DIRECTORY TO SEARCH]"
```

```
grep -r "search text" "/tmp/search/folder"
```

## 參考資料
* [command line - how to find a word in text files from a directory - Ask Ubuntu](https://askubuntu.com/questions/462276/how-to-find-a-word-in-text-files-from-a-directory)
