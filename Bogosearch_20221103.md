# Bogosearch

BogoSearch spec:

- Set `searched_count` to 1
- Loop until `searched_count` is the length of the list or the correct item is
  found
  - Pick a random index
  - If the index is not in `searched_indexes` (find using linear search),
    increment `searched_count`
  - Add the index to a list `searched_indexes`
  - If the item at the index is the item you are searching for, return the index

BogoBogoSearch spec:

- Label start
- Pick a random number
- Execute BogoSearch for this random number
- If the random number wasn't the number you're searching for, goto start
- Return the result of the last BogoSearch
