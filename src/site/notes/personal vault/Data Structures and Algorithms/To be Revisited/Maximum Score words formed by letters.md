---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/maximum-score-words-formed-by-letters/"}
---

##### Question:
![Screenshot 2024-05-26 at 1.40.33 PM.png](/img/user/personal%20vault/Attachments/Screenshot%202024-05-26%20at%201.40.33%20PM.png)

### Solution:
```Java
class Solution {
    public int maxScoreWords(String[] words, char[] letters, int[] score) {
        int[] freqOfLetters = new int[score.length];
        for (char ch : letters) {
            freqOfLetters[ch - 'a']++;
        }
        int res = backtrack(words, score, freqOfLetters, 0);
        return res;
    }

    private int backtrack(String[] words, int[] score, int[] farr, int idx) {
        if (idx == words.length) {
            return 0;
        }

        int scoreNo = 0 + backtrack(words, score, farr, idx + 1); // No call

        boolean isValid = true;
        int scoreOfWord = 0;
        for (char ch : words[idx].toCharArray()) {
            if (farr[ch - 'a'] == 0) {
                isValid = false;
                //break; (break won't be a good Idea because while recursing back we are incrementing the
                // freq of each char in that word but you broke out of the loop toh wahan freq jayada 
                // ho jayegi, ya toh remember kahan pe break huye wahi tak ki freq badhao, instead let the
                // loop happen and correct it recursively.)
            }
            farr[ch - 'a']--;
            scoreOfWord += score[ch - 'a'];
        }

        int scoreYes = 0;
        if (isValid) {
            scoreYes = scoreOfWord + backtrack(words, score, farr, idx + 1); // Yes call
        }

        //Correct the freq array after backtracking
        for (char ch : words[idx].toCharArray()) {
            farr[ch - 'a']++;
        }

        return Math.max(scoreYes, scoreNo);
    }
}
```