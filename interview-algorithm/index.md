# Interview Algorithm

<!--more-->

## 常用的排序算法

### 冒泡排序

```swift
func bubbleSort(_ arr: inout [Int]) {
    let n = arr.count
    for i in 0..<n {
        for j in 0..<n-i-1 {
            if arr[j] > arr[j+1] {
                arr.swapAt(j, j+1)
            }
        }
    }
}
```

### 选择排序

```swift
func selectionSort(_ arr: inout [Int]) {
    let n = arr.count
    for i in 0..<n {
        var minIndex = i
        for j in i+1..<n {
            if arr[j] < arr[minIndex] {
                minIndex = j
            }
        }
        arr.swapAt(i, minIndex)
    }
}
```

### 插入排序

```swift
func insertionSort(_ arr: inout [Int]) {
    let n = arr.count
    for i in 1..<n {
        let value = arr[i]
        var j = i - 1
        while j >= 0 && arr[j] > value {
            arr[j+1] = arr[j]
            j -= 1
        }
        arr[j+1] = value
    }
}
```

### 折半查找

```swift
func binarySearch(_ arr: [Int], _ target: Int) -> Int {
    var left = 0
    var right = arr.count - 1
    while left <= right {
        let mid = left + (right - left) / 2
        if arr[mid] == target {
            return mid
        } else if arr[mid] < target {
            left = mid + 1
        } else {
            right = mid - 1
        }
    }
    return -1
}
```

{{< admonition tip "折半查找">}}
折半查找的前提是数组有序，时间复杂度为 O(logn)。

{{< /admonition >}}


