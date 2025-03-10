# Problems

SNo | Name | Logic Used | Link |
----|------|------------|------|
1 | [Sort Color](https://leetcode.com/problems/sort-colors/) (DNF (Dutch National Flag) Algo) | 3 pointer partitioning | [Solution](DNF_sort.cpp)
2 | [Finding Missing and Repeating](https://practice.geeksforgeeks.org/problems/find-missing-and-repeating2512/1) | repeating xor {1 to n} or Playing with Indices| [Solution](missing_repeating.cpp)
3 | [Kadane's Algorithm](https://leetcode.com/problems/maximum-subarray/) : [EXPLANATION](https://leetcode.com/problems/maximum-subarray/discuss/1595195/C%2B%2BPython-7-Simple-Solutions-w-Explanation-or-Brute-Force-%2B-DP-%2B-Kadane-%2B-Divide-and-Conquer) | Max Subarray Sum | [Solution](kadanes_algorithm.cpp)
4 | [Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/) | Cycle Detection Linked List {index -> element} | [Solution](duplicate_number.cpp)
5 | [Merge Intervals](https://leetcode.com/problems/merge-intervals/) | sort, condition of merging | [Solution](merge_intervals.cpp)
6 | [Merge Sorted Arrays](https://practice.geeksforgeeks.org/problems/merge-two-sorted-arrays-1587115620/1) in O(1) Space | 2-Pointer & Sort Lastly| [Solution](merge_sorted_arrays.cpp)

### Q-1) [Sort Color](https://leetcode.com/problems/sort-colors/) : [Solution](DNF_sort.cpp)
### LOGIC:
We ensure that all 0's stay in the First and all 2's stay in the Last, while the Rest (i.e., 1's) are bound to stay in the Middle.
So, we need 3 Pointers (2 Fixed Ponter: start = 0, end = n-1, they are Moved when certian conditions are met and 1 Traversal Pointer: Used for Traversal), So we Loop untill mid<=end is Valid. So, whenever we see A[mid]=0, we Swap(A[start++],A[mid++]) (we increment mid as it has been processed, while we increment start, as we are Ensuring that all elements before start are 0's), else if it's 1, we do mid++ (we Just move on), else we Swap(A[mid],A[end--]) (we have decremented end, Ensuring All elements after End are 2's and we can't increment mid saying its processed, as A[mid] might be 2 too, if done so, it can never be processed again and gives Wrong Results (Say 000 2-mid 111 2 2-end), if mid++ is done, then -> 0002 1-mid 11 2-end 2 & the 2 before the mid can't be Processed, hence we Just Decrement end. While with start, we Increment Both, as Mid has started from 0, so if it had to process, it would've done already and then moved Further ensuring it's not pointing to 0 anymore, failing to do so would lead to Such Error as in 2)
### Q-2) [Find Missing and Repeating](https://practice.geeksforgeeks.org/problems/find-missing-and-repeating2512/1) : [Solution](missing_repeating.cpp)
### Solution-1: Playing with Indices:
Loop through the Array and whenever a value (i.e., abs(Arr[i])) at particular index is seen, mark Arr[val-1] as negative (we are marking element at (val-1)th index as negative to indicate that val is present in the Array) if positive, else if already negative, its the repeated number (as it has been marked prior, indicates it is its 2nd occurance), so store it, and for Missing Number, loop through all Indices of Array again and If Arr[I] is Positive, then I+1 is the Missing number, as had it been in Array, it's corresponding index value would've been negative, Hence Finally return these 2 (missing and repeated).
### Solution-2: XOR Logic:
Loop through the Arr and store the XOR of all elements in the Res, now Loop I from 1-N (as Arr elements lie in the Range oly!) and XOR the previously stored Res with I, so Finally we'll be getting Res = Missing ^ Repeating (as A^A=0, hence Even count values XOR will become 0), Now Lets consider the Right Most Set Bit (i.e., RSB) of the Res (as if Res had a set Bit -> indicates only either Missing or Repeating has that Bit Set, as had it been for Both or Neither, XOR would've been 0 (as 1^1 & 0^0 = 0). Now, loop through All elements of Arr and Store the XOR of All elements which have RSB as set Bit in it, we can have Many Elements Apart from Repeating or Missing (which will be decided in the END), Say as Repeating. If RSB is not Set, then Store the XOR of those elements in Missing. Now Loop from 1 to N and if RSB is set, then XOR with Repeating, else XOR with missing (Here Except for Repeating and Missing, Rest elements XOR will be 0, as they have Occured even number of times). Now to confirm which one is Repeating or Missing, Loop through Arr and if (missing = Arr[I]), then Swap(missing,repeating) (as had it been Missing it wouldn't be present in the Arr) and Finally Return Repeating and Missing.
### Q-4)[Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/) : [Solution](duplicate_number.cpp)
### LOGIC:
If there is no duplicate in the array, we can map each indexes to each numbers in this array. In other words, we can have a mapping function f(index) = number
For example, let's assume
nums = [2,1,3], then the mapping function is 0->2, 1->1, 2->3.
If we start from index = 0, we can get a value according to this mapping function, and then we use this value as a new index and, again, we can get the other new value according to this new index. We repeat this process until the index exceeds the array. Actually, by doing so, we can get a sequence. Using the above example again, the sequence we get is 0->2->3. (Because index=3 exceeds the array's size, the sequence terminates!)

However, if there is duplicate in the array, the mapping function is many-to-one.
For example, let's assume
nums = [2,1,3,1], then the mapping function is 0->2, {1,3}->1, 2->3. Then the sequence we get definitely has a cycle. 0->2->3->1->1->1->1->1->........ The starting point of this cycle is the duplicate number.
We can use Floyd's Tortoise and Hare Algorithm to find the starting point.

According to Floyd's algorithm, 
**first step**, if a cycle does exist, and you advance the tortoise one node each unit of time but the hare two nodes each unit of time, then they will eventually meet. This is what the first while loop does. The first while loop finds their meeting point.

**Second step**, take tortoise or hare to the start point of the list (i.e. let one of the animal be 0) and keep the other one staying at the meeting point. Now, advance both of the animals one node each unit of time, the meeting point is the starting point of the cycle. This is what the second while loop does. The second while loop finds their meeting point.

![image](https://user-images.githubusercontent.com/66252916/186201130-8377919b-1f54-4666-a2e8-9a0f2e2965f5.png)

**Proof of second step:**
Distance traveled by tortoise while meeting = x + y

Distance traveled by hare while meeting = (x + y + z) + y = x + 2y + z

Since hare travels with double the speed of tortoise,

so 2(x+y)= x+2y+z => x+2y+z = 2x+2y => x=z

Hence by moving tortoise to start of linked list, and making both animals to move one node at a time, they both have same distance to cover .

They will reach at the point where the loop starts in the linked list

### Q-5) [Merge Intervals](https://leetcode.com/problems/merge-intervals/) : [Solution](merge_intervals.cpp)
### LOGIC:
Initially Sort the Intervals Array, so that Initially we can Push the Intervals Directly. Then if Curr_Interval[0] <= Ans.back()[1], Indicates the Range of Previous Interval can be Updated, Now it Depends on 2nd Value (i.e., Max (Ans.back()[1],Curr_Interval[1] is Taken), As current Interval Can be Completely Inside (Say Ans had [1,6], now Curr_Interval is [3,5], so as it lies inside it, hence Ans.back()[1] remains the Same, as it has Max value, So Range is still [1,6]) or Partially Inside (ay Ans had [1,6], now Curr_Interval is [5,8], so as it lies inside it, hence Ans.back()[1] would be updated with Curr_Interval[1], Hence Range would be updated to [1,8]), Else if Curr_Interval[0] > Ans.back()[1], then Curr_Interval has to be Pushed (Say [1,6] is ans.back(), and Curr_Interval is [7,9], then its Pushed back to the Ans) and Finally Return Ans.

### Q-6) [Merge Sorted Arrays](https://practice.geeksforgeeks.org/problems/merge-two-sorted-arrays-1587115620/1) : [Solution](merge_sorted_arrays.cpp)
### LOGIC:
Initially Both Arrays are Sorted. Now 1st Array (size=n) should have smallest n elements, while 2nd Array (size=m) should have largest m elements, Hence Using 2-Pointer I(n-1) & J(0), if(Arr1[I] > Arr2[J]) then swap(Arr1[I--],Arr2[J++]), else Break (as at any Point, if Arr1 has element smaller than that of Arr2's element, then no element before this element in Arr1 can be greater than current Arr2's element as Arr1 is Sorted, So, previous ones would be Smaller.) Finally Sort Both Array's Individually, As we have Desired Elements in Desired Array, but they ain't sorted, Hence Sort them & return)
