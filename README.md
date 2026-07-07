# summer-pep-code-repo-

class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        ListNode *slow = head, *fast = head;
        while (fast && fast->next) {
            slow = slow->next;
            fast = fast->next->next;
        }
        return slow;
    }
};


class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end());
        vector<vector<int>> merged;
        vector<int> prev = intervals[0];

        for (int i = 1; i < intervals.size(); i++) {
            if (intervals[i][0] <= prev[1]) {
                // Merge overlapping intervals
                prev[1] = max(prev[1], intervals[i][1]);
            } else {
                merged.push_back(prev);
                prev = intervals[i];
            }
        }

        merged.push_back(prev);
        return merged;


class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> mp;
        
        for(auto x: strs){
            string word = x;
            sort(word.begin(), word.end());
            mp[word].push_back(x);
        }
        
        vector<vector<string>> ans;
        for(auto x: mp){
            ans.push_back(x.second);
        }
        return ans;
    }
};



class Solution {
public:
    void recursion(vector<int> num, int i, int j, vector<vector<int> > &res) {
        if (i == j-1) {
            res.push_back(num);
            return;
        }
        for (int k = i; k < j; k++) {
            if (i != k && num[i] == num[k]) continue;
            swap(num[i], num[k]);
            recursion(num, i+1, j, res);
        }
    }
    vector<vector<int> > permuteUnique(vector<int> &num) {
        sort(num.begin(), num.end());
        vector<vector<int> >res;
        recursion(num, 0, num.size(), res);
        return res;
    }
};   
    }
};



class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int left = 0;
        int minLength =INT_MAX, currentSum =0, length = 0;
        
        for(int right = 0; right < nums.size(); right++){
            currentSum += nums[right];
            while(currentSum >= target){
                length = right- left + 1;
                minLength = min(length, minLength);
                currentSum -= nums[left];
                left++;
            }
        }
        return (minLength == INT_MAX) ? 0 : minLength;
    }
};


160. Intersection of Two Linked Lists

Given the heads of two singly linked-lists headA and headB, return the node at which the two lists intersect. If the two linked lists have no intersection at all, return null.

For example, the following two linked lists begin to intersect at node c1:

The test cases are generated such that there are no cycles anywhere in the entire linked structure.

Note that the linked lists must retain their original structure after the function returns.

Custom Judge:

The inputs to the judge are given as follows (your program is not given these inputs):

intersectVal - The value of the node where the intersection occurs. This is 0 if there is no intersected node.
listA - The first linked list.
listB - The second linked list.
skipA - The number of nodes to skip ahead in listA (starting from the head) to get to the intersected node.
skipB - The number of nodes to skip ahead in listB (starting from the head) to get to the intersected node.
The judge will then create the linked structure based on these inputs and pass the two heads, headA and headB to your program. If you correctly return the intersected node, then your solution will be accepted.

Input: intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], skipA = 2, skipB = 3
Output: Intersected at '8'
Explanation: The intersected node's value is 8 (note that this must not be 0 if the two lists intersect).
From the head of A, it reads as [4,1,8,4,5]. From the head of B, it reads as [5,6,1,8,4,5]. There are 2 nodes before the intersected node in A; There are 3 nodes before the intersected node in B.
- Note that the intersected node's value is not 1 because the nodes with value 1 in A and B (2nd node in A and 3rd node in B) are different node references. In other words, they point to two different locations in memory, while the nodes with value 8 in A and B (3rd node in A and 4th node in B) point to the same location in memory.


1. Brute Force Solution
Time Complexity : O(m * n)
Space Complexity : O(1)

class Solution {
public:
	ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
		ListNode *temp;
		while(headA != NULL){
			temp = headB;
			while(temp != NULL){
				if(headA == temp){
					return headA;
				}
				temp = temp -> next;
			}
			headA = headA -> next;
		}
		return NULL;
	}
};

2. HashMap Solution
Time Complexity : O(m + n)
Space Complexity : O(m)

class Solution {
public:
	ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
		unordered_map<ListNode*, int> m;
		while(headA != NULL){
			m[headA]++;
			headA = headA -> next;
		}
		while(headB != NULL){
			if(m[headB] > 0){
				return headB;
			}
			headB = headB -> next;
		}
		return NULL;
	}
};


3. Length Difference Solution
Time Complexity : O(m + n)
Space Complexity : O(1)

class Solution {
public:
	ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
		int n = 0;
		int m = 0;
		ListNode* ptr1 = headA;
		ListNode* ptr2 = headB;
		while(ptr1 != NULL){
			n++;
			ptr1 = ptr1 -> next;
		}
		while(ptr2 != NULL){
			m++;
			ptr2 = ptr2 -> next;
		}
		int t = abs(n - m);
		if(n > m){
			while(t){
				headA = headA -> next;
				t--;
			}
		}
		else{
			while(t){
				headB = headB -> next;
				t--;
			}
		}
		while(headA != NULL and headB != NULL){
			if(headA == headB){
				return headA;
			}
			headA = headA -> next;
			headB = headB -> next;
		}
		return NULL;
	}
};

4. Two-Pointers Solution
Time Complexity : O(m + n)
Space Complexity : O(1)

class Solution {
public:
	ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
		ListNode *ptr1 = headA;
		ListNode *ptr2 = headB;
		while(ptr1 != ptr2){
			if(ptr1 == NULL){
				ptr1 = headB;
			}
			else{
				ptr1 = ptr1 -> next;
			}
			if(ptr2 == NULL){
				ptr2 = headA;
			}
			else{
				ptr2 = ptr2 -> next;
			}
		}
		return ptr1;
	}
};







