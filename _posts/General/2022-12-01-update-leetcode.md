---
layout: post
author: Timothy Hill
title: An Update and A Unique Leetcode Challenge
---

## An Update and A Unique Leetcode Challenge

Greetings. It has been sometime since I have written a post, and I have not pushed many (public) commits to my Github. What gives Tim?!?!?

There have been some updates in my life that explain this:

- A few weeks ago, Amazon announced that they would be going through a hiring freeze, [at least until the end of the year](https://techcrunch.com/2022/11/03/amazon-exec-confirms-corporate-hiring-freeze-through-end-of-year/).
- This meant that the role I had been preparing my interview skills for was no longer available
- I started a new project that I do not want to share at the moment since I am going slow and seeing where my experimentation takes me (ie: I am not sure how long it will take and don't want to make it public while it doesn't look great)
- I am working towards my [AWS Solutions Architect Associate Certification](https://aws.amazon.com/certification/certified-solutions-architect-associate/?ch=sec&sec=rmg&d=1). It shouldn't take me too long to get, but it is still taking up some time
- I have started surfing!! It's fun!!
- I am trying to 're-learn' how to type (instead if just using 4 fingers like a velociraptor), so for the moment I type far slower

So I have been a bit away from my regular programming activities. Don't worry though, I do have something interesting to share in this article.

## Custom Data Structure

To keep up my skills, I have been doing some challenges on Leetcode. This particular challenge I found interesting so I thought I would document my current solution (which I have confirmed works) after which I will review the given solution (which I have not yet read) and see where I went right/wrong. Since I was somewhat lazy when coming up with my current solution I don't think it is necessarily the best.

The challenge, which you can find [here](https://leetcode.com/problems/insert-delete-getrandom-o1/), is to create the 'RandomizedSet' class with the following properties:

- On average O(1) insert
- On average O(1) delete
- On average O(1) random retrieval


At first I was a bit confused, since I initially thought that the requirement was perfect O(1) time complexity for all operations. This wasn't helped by the fact that I was initially being lazy when trying to implement my solution.


My solution came about from the following thought process:

- Since O(1) insert and delete is required, it seemed that a hash data structure would be fitting to store values.
- Since the values being store are their own keys, a set is better used than a map which would use more space
- In order to ensure we could randomly access elements of the set we need some way of indexing valid elements: that is, we need to have a way of keeping track of all valid values and selecting one of them at random (instantly, or at least with minimal iteration)

This lead me to the following solution:

- The values to be store are signed integers with range [2<sup>32-1</sup>-1, -2<sup>32-1</sup>] in c++
- This means there are 2<sup>32</sup> possible values
- This coincidentally is how many indices we can represent with a single unsigned int in c++
- To be efficient, we want to have a certain number of buckets that store roughly the same amount of values (since hash chaining will also be used)
- As such we will have roughly $\sqrt{2^(32)}$ = 65536 buckets, and use the number of buckets to create our hash function (rand() % num_buckets)
- Eventually I settled on using the nearest prime smaller than 65536 which is 65521 since at the time of programming I thought this would let to a more even distribution of hashes (looking back now, I don't think that's the case)
- Hash collisions will be resolved via [chaining](https://www.geeksforgeeks.org/separate-chaining-collision-handling-technique-in-hashing/)  using linked lists.
- To keep track of which values are valid, we will store the indices of non-empty buckets in a c++ set ([a binary search tree](https://cplusplus.com/reference/set/set/))
- Since the max value for an index is 65521, this can be represented using an unsigned short to save some space

Below is the code I submitted that passed the tests (in its initial glory, 'cout' statements and all).

~~~ cpp
#include <ctime>
#include<list>
#include<set>
#include <algorithm>
#include<cstdlib>
/**
    IDEA:
    - Create a Custom Hash Map
    - Since range of Ints is essentially unsigned int MAx
    - we take the root of this value and find the nearest prime 
    - less than this number
    - we will have a fixed array of size 65521 (that number) 
    - and use this number for the mod
*/

//TODO:
//

class RandomizedSet {
    
private:
    const int PRIME = 65521;
    list<int> buckets[65521];
    set<unsigned short> valid_buckets;
    
public:
    RandomizedSet() { 
        srand((unsigned) time(NULL));
	
    }
    
    bool insert(int val) {
        unsigned int hash = static_cast<unsigned int>(val) +INT_MAX;
        hash = hash % PRIME;
        
        if(buckets[hash].empty()){
            valid_buckets.insert(hash);
            buckets[hash].push_front(val);
        }
        else{
            if(find(buckets[hash].begin(), buckets[hash].end(), val) != buckets[hash].end()){
                return false;
            }
            buckets[hash].push_back(val);
        }
        
        //print();
        
        return true;
    }
    
    bool remove(int val) {
        //cout<<"REMOVE+++++++++++++++++"<<endl;
        unsigned int hash = static_cast<unsigned int>(val) +INT_MAX;
        hash = hash % PRIME;
        //cout<<"Hash: "<<hash<<endl;
        
        if(buckets[hash].empty()){
            return false;
        }
        
        if(find(buckets[hash].begin(), buckets[hash].end(), val) == buckets[hash].end())
            return false;
        
        buckets[hash].remove(val);
        if(buckets[hash].empty())
            valid_buckets.erase(hash);
        
        //print();
        
        return true;
    }
    
    int getRandom() {
        //cout<<"GET RANDOM+++++++++++++++++"<<endl;
        
        int bucket_index  = rand() % valid_buckets.size();
        set<unsigned short>::iterator it = valid_buckets.begin();
        for(int i=0; i<bucket_index; ++i)
            it ++;
        bucket_index = *it;
        
        list<int>::iterator it2 = buckets[bucket_index].begin();
        int rand2 = rand() % buckets[bucket_index].size();
        //cout<<"Bucket Index "<<bucket_index<<" rand2: "<<rand2<<endl;
        for(int i=0; i<rand2; ++i)
            it2 ++;
        
        return *it2;
    }
    
    void print(){
        for(set<unsigned short>::iterator it = valid_buckets.begin(); it != valid_buckets.end(); ++ it){
            cout<<"Bucket at index: "<<*it<<"-----------------"<<endl;
            for(list<int>::iterator it2 = buckets[*it].begin(); it2!= buckets[*it].end(); ++it2){
                cout<<*it2<<" ";
            }
            cout<<endl<<endl;
        }
    }
    
};

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * RandomizedSet* obj = new RandomizedSet();
 * bool param_1 = obj->insert(val);
 * bool param_2 = obj->remove(val);
 * int param_3 = obj->getRandom();
 */
~~~

This class run each operation as follows:

### 1. INSERT worst case

Calculating the hash for the bucket index is constant time. Retrieving the bucket at the index of the hash is O(1) and insertion at the head
and tail of a bucket are both [constant](https://cplusplus.com/reference/list/list/push_back/). The issues is searching through this bucket to
determine whether or not an element already exists in the bucket. However, since each list has a max size of 65521+- the lookup at worst
case is log<sub>2</sub>(65521)and on average would be far less than this (since the test would at most make 2 * 10<sup>5</sup> insertions).

### 2. Delete worst case

Again, calulating hashes are constant as is retrieving a bucket. Searching for the value to delete is at worse log<sub>2</sub>(65521) complexity,
as is removing a key in the set (tree) [see here](https://cplusplus.com/reference/set/set/erase/). Deletion of this

### 3. getRandom worst case

The problem in this case is we need to iterate linearly through the set to convert our randomly generated index into a bucket from which a value can be retrieved. Similarly, we iterate linearly through a bucket to retrieve a random value. Worst case for both of these operations is log<sub>2</sub>(65521).

Correct me if I am wrong on these points, since I am running out of time to finish this post.

## Potential improvements

Before looking through some of the other solutions for this challenge, I should say that my algorithm is more so time optimized than storage optimized. I could possibly use a tree instead of linked lists for buckets so that searching for insert is O(log(n)) but deletion will suffer if I do this. I also imagine that I could have tinkered with the hash function and number of buckets and hash function to be more efficient for this particular test.

## After examining other solutions

It seems that I was close to a better solution with one of my original ideas.

A better way to approach this problem is to store values and their indices in a hashmap. The index for each value maps to the index of that value in a vector. This gives
us O(1) insert (without resizing too often) and getRandom.

To ensure O(1) deletem we can swap the value we want to delete to the end of the array and then pop it off (which is roughly O(1) without vector resize).

## Conclusion

The conclusion I have for this exercise is twofold:

- We can combine different data structures together to make use of both of their strengths
- If you try to come up with a lazy solution without planning your code thoroughly you will likely miss a far easier and more elegant solution.
