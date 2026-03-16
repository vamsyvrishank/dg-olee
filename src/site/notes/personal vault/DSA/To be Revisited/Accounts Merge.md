---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/accounts-merge/"}
---

##### Question:

![Screenshot 2024-05-26 at 12.45.36 PM.png](/img/user/personal%20vault/Attachments/Screenshot%202024-05-26%20at%2012.45.36%20PM.png)
#### Solution:
> For Disjoint Set class copy template from [[personal vault/DSA/To be Revisited/DisJoint Set#UNION BY RANK (LESS INTUITIVE)\|DisJoint Set#UNION BY RANK (LESS INTUITIVE)]].
```java
public List<List<String>> accountsMerge(List<List<String>> accounts) {
        int n = accounts.size();
        DisjointSet dsu = new DisjointSet(n);

        // Step 1: Create a mapping from email to user index
        Map<String, Integer> emailToIndex = new HashMap<>();
        for (int i = 0; i < n; i++) {
            List<String> account = accounts.get(i);
            for (int j = 1; j < account.size(); j++) {
                String email = account.get(j);
                if (!emailToIndex.containsKey(email)) {
                    emailToIndex.put(email, i);
                } else {
                    // Step 2: Merge accounts with the same email
                    dsu.unionByRank(i, emailToIndex.get(email));
                }
            }
        }

        // Step 3: Create a mapping from [user index] to [list] of emails
        Map<Integer, List<String>> indexToEmails = new HashMap<>();
        for (Map.Entry<String, Integer> entry : emailToIndex.entrySet()) {
            String email = entry.getKey();
            int index = dsu.findUltimateParent(entry.getValue());

            indexToEmails.putIfAbsent(index, new ArrayList<>());//initialize the index with empty array
            indexToEmails.get(index).add(email); //get that array and add email to that list
        }

        // Step 4: Create the merged accounts list
        List<List<String>> mergedAccounts = new ArrayList<>();
        for (Map.Entry<Integer, List<String>> entry : indexToEmails.entrySet()) {
            int userIndex = entry.getKey();
            List<String> emails = entry.getValue();
            
            Collections.sort(emails);
            
            List<String> account = new ArrayList<>();
            account.add(accounts.get(userIndex).get(0)); // Add the account name
            account.addAll(emails);
            mergedAccounts.add(account);
        }

        return mergedAccounts;
    }
```