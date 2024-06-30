---
title: تاثیر Allocate و Deallocate کردن‌های بیهوده بر زمان اجرایی
date: 2024-06-30 15:30:00+0000
description: یک تکه کد golang را که برای حل یک مسئله الگوریتمی نوشته بودم، بررسی می‌کنیم
tags: 
    - leetcode
    - algorithm
    - golang
    - optimization
    - memory
categories:
    - tip
slug: allocate-deallocate-optimization
image: cover.png
---

```go
func isValidSudoku(board [][]byte) bool {
	for _, row := range board {
		seen := map[byte]struct{}{}
		for _, column := range row {
			if column == '.' {
				continue
			}
			if _, ok := seen[column]; ok {
				return false
			}
			seen[column] = struct{}{}
		}
	}

	for column := 0; column < 9; column++ {
		seen := map[byte]struct{}{}
		for _, row := range board {
			if row[column] == '.' {
				continue
			}
			if _, ok := seen[row[column]]; ok {
				return false
			}
			seen[row[column]] = struct{}{}
		}
	}

	for i := 0; i < 9; i += 3 {
		for j := 0; j < 9; j += 3 {
			seen := map[byte]struct{}{}
			for k := i; k < i+3; k++ {
				for l := j; l < j+3; l++ {
					if board[k][l] == '.' {
						continue
					}
					if _, ok := seen[board[k][l]]; ok {
						return false
					}
					seen[board[k][l]] = struct{}{}
				}
			}
		}
	}

	return true
}
```

```diff
func isValidSudoku(board [][]byte) bool {
+	seen := map[byte]struct{}{}
	for _, row := range board {
+		clear(seen)
-		seen := map[byte]struct{}{}
		for _, column := range row {
			if column == '.' {
				continue
			}
			if _, ok := seen[column]; ok {
				return false
			}
			seen[column] = struct{}{}
		}
	}

	for column := 0; column < 9; column++ {
+		clear(seen)
-		seen := map[byte]struct{}{}
		for _, row := range board {
			if row[column] == '.' {
				continue
			}
			if _, ok := seen[row[column]]; ok {
				return false
			}
			seen[row[column]] = struct{}{}
		}
	}

	for i := 0; i < 9; i += 3 {
		for j := 0; j < 9; j += 3 {
+			clear(seen)
-			seen := map[byte]struct{}{}
			for k := i; k < i+3; k++ {
				for l := j; l < j+3; l++ {
					if board[k][l] == '.' {
						continue
					}
					if _, ok := seen[board[k][l]]; ok {
						return false
					}
					seen[board[k][l]] = struct{}{}
				}
			}
		}
	}

	return true
}
```