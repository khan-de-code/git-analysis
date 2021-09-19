# git-analysis

count number of code lines in git repository for a specific user by file
```
author="<user>"
git ls-files | grep -v -E "\.md|\.lock|\.gitignore" | while read f; do replaceEscaped=$(sed 's/[&/\]/\\&/g' <<<"$f"); git blame -w --line-porcelain -- "$f" | grep -I '^author ' | sed s/^/"$replaceEscaped"" "/; done | grep "$author" | awk '{print $1}' | sort -f | uniq -ic | sort -n | tee /dev/tty | awk '{s+=$1} END {printf "SUM: %.0f\n", s}'
```

count number of code lines in git repository by user
```
git ls-files | grep "^src/app\|^src/server" | grep -v "model.js\|.json" | while read f; do git blame -w -M -C -C --line-porcelain -- "$f" | grep -I '^author '; done | sort -f | uniq -ic | sort -n
```

total number of code lines written by all users
```
git ls-files | grep "^src/app\|^src/server" | grep -v "model.js\|.json" | while read f; do git blame -w -M -C -C --line-porcelain -- "$f" | grep -I '^author '; done | sort -f | uniq -ic | sort -n | awk '{s+=$1} END {print s}'
```
