---
{"dg-publish":true,"permalink":"/personal-vault/system-design/high-level-design/leaderless-replication/"}
---


![[Pasted image 20240803121306.png \| 500]]

Where it is used ? 
1. Cassandra 
2. Riak
3. Dynamodb ? No bitch

![[Pasted image 20240803121435.png \| 500]]

20 : version.
what value do we read ? read from latest version.
and when the reader sees and outdated value , he sends the update after - Read Repair.


Anti Entropy : 

![Pasted image 20240803123759.png](/img/user/personal%20vault/Attachments/Pasted%20image%2020240803123759.png)

We can use Merkle Trees to quickly send data.
We use them to tell how two db tables differ , quickly and efficiently.

![Pasted image 20240803130038.png](/img/user/personal%20vault/Attachments/Pasted%20image%2020240803130038.png)

we take hash under 1000 , sum them , then take hash again. and again till we are left with one root node.

Now merkle tree is created, for 1 table in db.

Comparing merkle trees : 

![Pasted image 20240803130254.png](/img/user/personal%20vault/Attachments/Pasted%20image%2020240803130254.png)
only diff b = 6 , b = 9

 we do DFS from top to bottom and go to the values that are diff , with each step  we half the tree ( logarithmic time complexity) O(log(n))
we can now only send b =6 over the network.


To be continued : 

![Pasted image 20240803130532.png](/img/user/personal%20vault/Attachments/Pasted%20image%2020240803130532.png)



### Quorums -> Instant Reads of Writes 

There is way that you can write and someone else can instantly read that without worrying about read repair and bunch of anti entropy for changes to propagate.

![Pasted image 20240803141459.png](/img/user/personal%20vault/Attachments/Pasted%20image%2020240803141459.png)


we will make sure in the arrangement that at least one node is updated. so when 3 nodes are read.

when W + R > N we have a quorum read and quorum write


### Are quorums strongly consistent ? 

![Pasted image 20240803142058.png](/img/user/personal%20vault/Attachments/Pasted%20image%2020240803142058.png)

### Another case : failed writes 

![Pasted image 20240803142232.png](/img/user/personal%20vault/Attachments/Pasted%20image%2020240803142232.png)

x = 10 is write , but that write is not propagated correctly , reader reading db 2 and 3 would actually get 6 which is inconsistency.


### Sloppy Qourums

Often people have diff db clusters. 

![Pasted image 20240803142431.png](/img/user/personal%20vault/Attachments/Pasted%20image%2020240803142431.png)
when cluster goes down , the writes are routed to cluster 2 , 
now when cluster 1 comes back up , this guy now reads from cluster one. he is not going to read the previous writes he made before in cluster 2.

sol : take data from cluster 2 and transfer to cluster 1. 
This is not strongly consistent.

![Pasted image 20240803142639.png](/img/user/personal%20vault/Attachments/Pasted%20image%2020240803142639.png)

