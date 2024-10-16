# Longest-Happy-String

A string s is called happy if it satisfies the following conditions:

s only contains the letters 'a', 'b', and 'c'.
s does not contain any of "aaa", "bbb", or "ccc" as a substring.
s contains at most a occurrences of the letter 'a'.
s contains at most b occurrences of the letter 'b'.
s contains at most c occurrences of the letter 'c'.
Given three integers a, b, and c, return the longest possible happy string. If there are multiple longest happy strings, return any of them. If there is no such string, return the empty string "".

A substring is a contiguous sequence of characters within a string.

 
Example 1:

Input: a = 1, b = 1, c = 7
Output: "ccaccbcc"
Explanation: "ccbccacc" would also be a correct answer.
Example 2:

Input: a = 7, b = 1, c = 0
Output: "aabaa"
Explanation: It is the only correct answer in this case.
 

Constraints:

0 <= a, b, c <= 100
a + b + c > 0

# SOLUTION

* Intuition
The problem asks to construct the longest possible string using the letters 'a', 'b', and 'c' such that:

No three consecutive characters are the same.
The counts of 'a', 'b', and 'c' do not exceed the given values a, b, and c respectively.
The intuition here is that we should always aim to use the character with the highest count, while ensuring that it does not violate the "no three consecutive characters" rule. A greedy approach works well here because we can construct the string step by step by selecting the most available character, and if adding that character leads to three consecutive letters, we switch to the next most available character.

* Approach
Greedy Strategy: Always try to use the character with the highest remaining count. If that character has been used twice consecutively, switch to the next most frequent character.
Priority Queue: Use a max-heap (or priority queue) to keep track of the character counts. This ensures that at every step, we can easily pick the character with the highest remaining count.
Condition Handling: If the character with the highest count has been used twice in a row, we temporarily switch to the second-highest character to avoid three consecutive characters.
Stop When No Characters Left: The process stops when no more characters can be added without violating the conditions.

* Complexity
Time complexity:
O(n) where (n = a + b + c) since each character is processed at most once and we make at most 3 log operations per step (for the heap).

* Space complexity:
O(1) for the priority queue, as we store a maximum of three characters. The result string takes (O(n)), but that is required for the output, so the effective auxiliary space is constant.

# JAVA CODE 

import java.util.PriorityQueue;

class Solution {

    public String longestDiverseString(int a, int b, int c) {
    
        // Priority queue to store the characters and their counts.
        
        PriorityQueue<int[]> pq = new PriorityQueue<>((x, y) -> y[0] - x[0]);
        
        if (a > 0) pq.offer(new int[]{a, 'a'});
        
        if (b > 0) pq.offer(new int[]{b, 'b'});
        
        if (c > 0) pq.offer(new int[]{c, 'c'});

        StringBuilder result = new StringBuilder();

        while (!pq.isEmpty()) {
        
            int[] first = pq.poll();

            // Check if last two characters are the same.
            
            if (result.length() >= 2 && result.charAt(result.length() - 1) == first[1] &&
            
                result.charAt(result.length() - 2) == first[1]) {

                if (pq.isEmpty()) break;  // No more valid characters.

                // Pick the second character.
                
                int[] second = pq.poll();
                
                result.append((char) second[1]);
                
                second[0]--;

                if (second[0] > 0) pq.offer(second);
                
                pq.offer(first);
                
            } else {
            
                result.append((char) first[1]);
                
                first[0]--;

                if (first[0] > 0) pq.offer(first);
                
            }
            
        }

        return result.toString();
    }
}

* Step-by-Step Detailed Explanation
Priority Queue Setup: In each solution, we use a priority queue (or similar structure) to maintain the counts of 'a', 'b', and 'c'. This allows us to efficiently pick the character with the highest remaining count.

* Main Loop: The loop continues as long as there are valid characters to add to the result string. In each iteration, we pick the character with the highest count. If it forms three consecutive characters, we switch to the second-highest count character.

* Character Addition: We append the selected character to the result string. If it doesn't violate the "no three consecutive" rule, we reduce its count and push it back to the queue for future consideration.

* Stopping Condition: The process stops when no more valid characters can be added without breaking the rule. The result string is then returned as the final output.

