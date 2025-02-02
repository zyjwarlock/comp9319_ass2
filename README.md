# BWT Encode and Backward Search

[Project Description](https://github.com/zyjwarlock/COMP9319_ASS2_BWT_Encoding/blob/master/COMP9319%202018s2%20Assignment%202.pdf)

This document is for describing the algorithm and the structure of the code of my assignment solution.

For BWT encoding, I first design a structure array called "Bucket", including 130 elements for each ascII from 0 to 129. Each structure contains an index array for storing indexes of certain character in the original file and a current size of index array. Secondly, I sort the index array of each "Bucket" respectively. Because of the same character in each "Bucket", I sort indexes by comparing the ascII of the charater of it, and the Quick Sort algorithm is appled in this sort function. Thirdly, I traverse the index array of "Bucket" which containing the delimiter at first and write the previous character of it into the BWT file. Finally, I iterate the rest of indexes array of Buckets whose the size of index array is larger than 0, and write into BWT file via using the same algorithm as writing delimiter.

For large file encoding which is larger the 30MB, I first put indexes of previous 30,000,000 elements of the original file array and store in a temporary file after sorted. Then, repeat the last step and store the rest of elements in other temporary file. Next, I load the whole first temporary file and 2.5MB of second temporary file in each time.
Last, compare the characters in these two array via applying the previous algorithm and write the small one in the BWT file.

For index file, I employ twice sort. I store the index of delimiter by the order of writing BWT in a temporary file. After the writing BWT file completed, I free both the "Bucket" structure array and the original file string, that I can get enough memory for loading the temporary file into a integer array. In addition, a new integer array whose size is same as the array loaded before, storing ascending order amount from 1,  has been created. Moreover, I sort these two arraies basied on the order of delimiter index array, and make it ordered. Finally, all elements in delimiter index array have been replace the current index from 1, and resort it according to the order of other array. The elements in delimiter index array are the index that we need. I store them into a file named filename.aux.

Forn search, I first judge if there are C table file, which stores the starting position of characters, and Occur table, storing the times of occurence before certain rows. If not, these two files should be create before any search process. To create occur table file, I traverse the BWT array, and write the occurrence of characters in every 1000 row. After the occur table file has been created, I can easily get C table from the occurrence array.

For "-i" Searching, I locate the starting postion and the ending position in BWT file. Then, I travese the each charater from starting postion to ending position, and find out the previous character until the delimiter be found.

For "-m" Searching, I locate the starting postion and the ending position of the last character of search string from in BWT file at first. Then appling the backward search algorithm to find out the previous character step by step. If the whole string has been found, the position of first character will be return, if not, the ending position will be behind to the starting position. Finally, the quantity of the string occuring in file can be got by using ending position minus starting position add 1.

For "-n" Searching, I continue use the result returned by "-m" search, positions of the first character of the string in the file. Using the backward search algorithm continue to find out the previous charaters of each position until the delimiter has been found. Record the index of positon, remove the duplicated one and count the quantity.

For "-a" Searching, a nonredundant delimiter index array of the result from "-n" search can be applied. The order of delimiter can be detected from index file saved in the encode process. 
