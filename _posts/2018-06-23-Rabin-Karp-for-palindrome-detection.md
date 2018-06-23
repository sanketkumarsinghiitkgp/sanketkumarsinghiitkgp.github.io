---
layout: post
title: Modified Rabin Karp for longest palindromic substring
---

Rabin Karp is a popular string-search algorithm which uses a rolling hash to search for a the occurence of a string(s) inside an another string(t).

Let t:="abcdfasjas", s:="fas", t.length=m, s.length=n for all further discussion.

## Key Insights from the Rabin Karp algorithm

The degree of the hashing polynomial is fixed(and equal to n)
So, every time we slide our rolling hash ahead (to check if the next substring is the one being searched for), it takes constant time, in contrast to a naive checking of each character which takes linear time.
So, in the best case(when for *any* substring s^'^ h(s^'^)=h(s)=>s^'^=s), the time complexity reduces to O(n).

## Palindrome Detection by Expanding Around Centres(PDEAC for ease of reference)

A palindrome is symmetric about it's center. The centre of symmetry an odd lengthed palindrome is the middle character of the palindrome, while that of an even lengthed palindrome is the space between the two middle characters. An implementation can be found [here](https://www.geeksforgeeks.org/longest-palindromic-substring-set-2/).
It iterates through all potential centres(spaces and characters which can serve as the centre of the longest palindromic substring) of the string and expands about it, checking if it is a palindrome.

## An Observation

Every time we expand about a centre, we start our search with a substring of length of 2, even if we already know of a palindromic substring of a greater length. ***We cannot search straight away for a pallindromic sequence of a specified length without first searching for smaller length pallindromic sequences.***
This can be sokved by modifying Rabin Karp.

## The Modification

The rolling hash used for Rabin Karp at each step removes a character and adds a character and thus 'rolls' along the string with a constant length. Let us skip the part involving removing of characters. So, this has rolls and increase in length each time.
I intend to use this hash for checking if the centre in PDEAC algorithm is the centre of a palindrome. We check it by using two rolling hashes which roll in two opposite directions along the string.
The advantage of using a rolling hash as opposed to a naive character-wise comparision(as used in native PDEAC), is that once a pallindrome of length **M** is found, for all the subsequent centers in the string, we would have the starting length of the hashing start from **M**.
 