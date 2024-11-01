# chaoslabs

Given two sorted and distinct lists of ranges A and B, return a merged list C containing all intersections between any two ranges in A and B.
[a,b] is the mathematical notation of a range. It represents the set of all integers between a and b, including a and b.
Example:
Input:
```
A = {[2,3], [6, 9], [15, 19]}
B = {[1,4], [5, 7], [8, 16], [22, 27]}
```
Output:
```
C= {[2,3], [6, 7], [8, 9], [15, 16]}
```

```go
type Range struct {
	Begin int
	End   int
}

func merge(A, B, C []Range) {
	refA, refB := 0, 0 // a = [2,3], b = [1,4]

	// len(A) = 3, len(B) = 4
	for refA < len(A) && refB < len(B) {
		// if intersection - update C
		if ok, intersection := intersection(A[refA], B[refB]); ok {
			C = append(C, intersection)
		}
		if A[refA].End < B[refB].End {
			// 3 < 4
			// refA = 1 ([6, 9]), refB = 0 ([1,4])
			refA++
		} else { // [6,9] vs [1, 4], 9 > 4
			refB++
		}
	}
}

func intersection(range1, range2 Range) (bool, Range) {
	// a = [2,3], b = [1,4]
	// 0 1 2 3 4 5 5 6 7
	//     X X
	//   X     X
	intersectionStart := max(range1.Begin, range2.Begin)
	intersectionEnd := min(range1.End, range2.End)
	if intersectionStart < intersectionEnd {
		return true, Range{intersectionStart, intersectionEnd}
	}
	return false, Range{}
}
```
