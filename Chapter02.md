#CLSR Chapter 2 - Getting Started
##2.1 Insertion Sort
1. Efficient for small input  
2. In place  
3. Stable

When implementation in C++, *bidirectional iterator* is needed.

```C++
// Insertion Sort
template <typename BidirectionalIt, typename Compare>
void insertion_sort(BidirectionalIt first, BidirectionalIt last, Compare comp) {
    if(first == last)
        return;
    
    auto j = first; ++j;
    for(; j != last; ++j) {
        auto key = *j;
        // Insert A[j] into the sorted sequence A[1...j-1]
        auto i = j; --i;
        while (i != first && !comp(*i, key)) {
            // A[i+1] = A[i]
            auto tmp = i; ++tmp; 
            *tmp = *i;  
            --i;
        }
        *(++i) = key;
    }
}


// Insertion Sort (Default version)
template <typename BidirectionalIt>
void insertion_sort(BidirectionalIt first, BidirectionalIt last) {
    insertion_sort(first, last, std::less<typename std::iterator_traits<BidirectionalIt>::value_type>());
}
```
With `<algorithm>`, we can implement this way
```C++
template <typename BidirectionalIt, typename Compare>
void insertion_sort(BidirectionalIt first, BidirectionalIt last, Compare comp) {
    for(auto i = first; i != last; ++i) {
        auto j = i;
        ++j;
        std::rotate(std::upper_bound(first, i, *i), i, j);
    }
}
```