# git-analysis

count number of code lines in git repository for a specific user by file
```bash
author="<user>"
git ls-files | grep -v -E "\.md|\.lock|\.gitignore" | while read f; do replaceEscaped=$(sed 's/[&/\]/\\&/g' <<<"$f"); git blame -w --line-porcelain -- "$f" | grep -I '^author ' | sed s/^/"$replaceEscaped"" "/; done | grep "$author" | awk '{print $1}' | sort -f | uniq -ic | sort -n | tee /dev/tty | awk '{s+=$1} END {printf "SUM: %.0f\n", s}'
```

count number of code lines in git repository by user with the sum of all lines output at the bottom
```bash
git ls-files | grep "^src/app\|^src/server" | grep -v "model.js\|.json" | while read f; do git blame -w -M -C -C --line-porcelain -- "$f" | grep -I '^author '; done | sort -f | uniq -ic | sort -n | tee "$(tty)" | awk '{sum += $1} END {print sum " total lines"}'
```

total number of code lines written by all users
```bash
git ls-files | grep "^src/app\|^src/server" | grep -v "model.js\|.json" | while read f; do git blame -w -M -C -C --line-porcelain -- "$f" | grep -I '^author '; done | sort -f | uniq -ic | sort -n | awk '{s+=$1} END {print s}'
```

total number of code lines written by all users with percentages
```bash
git ls-files | grep -v ".json" | while read f; do git blame -w -M -C -C --line-porcelain -- "$f" | grep -I '^author '; done | sort -f | uniq -ic | sort -n | { temp=$(</dev/stdin); IFS=$'\n' read -rd '' -A lines <<< $temp; total_lines=$(awk '{sum += $1} END {print sum}' <(echo $temp)); for line in $lines; do line_count=$(awk '{print $1}' <(echo $line)); percent=$(bc <<< "scale=3;$line_count/$total_lines*100") && echo "${line} ${percent}%"; done; echo "\n$total_lines total lines"}
```
