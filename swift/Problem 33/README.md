## Description

This problem was asked by Microsoft.

Compute the running median of a sequence of numbers. That is, given a stream of numbers, print out the median of the list so far on each new element.

Recall that the median of an even-numbered list is the average of the two middle numbers.

For example, given the sequence `[2, 1, 5, 7, 2, 0, 5]`, your algorithm should print out:

```
2
1.5
2
3.5
2
2
2
```

## Solution

The first important information:

> Recall that the median of an even-numbered list is the average of the two middle numbers.

So for the list [0,1,2,3], the median is 1.5, therefore, it seems important to have an ordered list in our algorithm.

- We will need a function that finds the median of our array (it will order the array if not ordered yet and get its mid values sum divided by 2). While they didn't mention it, the example shows that the median of an odd list is the mid value.

- It would be great if we could keep an ordered array and append the values in the good index everytime. We should be able to achieve this by populating an empty list and using a binary search algo to determine the position of the next item in the queue at each iteration:

```swift
extension Array where Element == Int {
    /**
     *
     * This function should only be used for ordered array
     *
     */
    func binarySearchInsertIndex(_ element: Int) -> Int {
        var leftBoundary = 0
        var rightBoundary = self.count-1
        
        while leftBoundary <= rightBoundary {
            let mid = (leftBoundary + rightBoundary) / 2
            
            if self[mid] > element {
                rightBoundary = mid - 1
            } else if self[mid] < element {
                leftBoundary = mid + 1
            } else {
                return mid
            }
        }
        
        return leftBoundary
    }
}
```

Now we can build our main function.
 
It builds a new array of integers as it loop through the initial array, insert them at the correct position and calculate the median

```swift
extension Array where Element == Int {
    func spawnProgressiveMedians() -> [Double] {
        var result: [Double] = []
        
        var orderedArray: [Int] = []
        
        for element in self {
            orderedArray.insert(element, at: orderedArray.binarySearchInsertIndex(element))
            
            if orderedArray.count % 2 == 0 {
                let median = Double(orderedArray[orderedArray.count/2-1] + orderedArray[orderedArray.count/2]) / 2
                result.append(median)
                print(median)
            } else {
                let median = Double(orderedArray[orderedArray.count/2])
                result.append(median)
                print(median)
            }
        }
        
        return result
    }
}
```

## Tests

```swift
class Problem_33Tests: XCTestCase {

    func testExample() {
        let input = [2, 1, 5, 7, 2, 0, 5]
        print(input.spawnProgressiveMedians())
    }
}
```