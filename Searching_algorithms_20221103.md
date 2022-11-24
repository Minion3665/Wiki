# Searching algorithms

## Q1

1. False
2. Failed = True
3. L <- M - 1

<!-- > -->

## Q2

1. O(k^n)
2. O(log n)
3. O(1)
4. O(n)
5. In the average case you need to search through half of the items in the list
   (i.e. n/2), but in O notation we ignore coefficients

## Q3

1. -
   1. 271
   2. There are 271 items in the list, each of which will potentially need to be
      looked at
2. -
   1. 9
   2. log_2(271) = 8.08, which rounds up to 9

| Count 1 | Count2 | Temp | A[1] | A[2] | A[3] | A[4] | A[5] |
|---------|--------|------|------|------|------|------|------|
|         |        |      | 23   | 45   | 16   | 12   | 31   |
| 1       | 1      |      |      |      |      |      |      |
|         | 2      | 45   |      | 16   | 45   |      |      |
|         | 3      | 45   |      |      | 12   | 45   |      |
|         | 4      | 45   |      |      |      | 31   | 45   |
| 2       | 1      | 23   | 16   | 23   |      |      |      |
|         | 2      | 23   |      | 12   | 23   | 31   | 45   |
|         | 3      |      |      |      |      |      |      |
|         | 4      |      |      |      |      |      |      |
| 3       | 1      | 16   | 12   | 16   |      |      |      |
|         | 2      |      |      |      |      |      |      |
|         | 3      |      |      |      |      |      |      |
|         | 4      |      |      |      |      |      |      |
| 4       | 1      |      |      |      |      |      |      |
|         | 2      |      |      |      |      |      |      |
|         | 3      |      |      |      |      |      |      |
|         | 4      |      |      |      |      |      |      |

1. Perform a bubble sort
2. -
   1. Add a check to see if the pass changed anything, and replace the outer
      loop with a while loop
   2. Do not go to the last item on the second pass (and so on) to avoid
      checking sorted items
