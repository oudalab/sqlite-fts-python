Dr. Grant and I did some research on this, it looks like the way fts virtual table indexing does not work well with latin tokenizer.
For Phrase searching it is not working well. For Example: 
we have contents: "This is a test sentence"
and when we use the porter tokenizer provided by sqlite, it is being parsed into
"('thi', 0, 4, 0)
('is', 5, 7, 1)
('a', 8, 9, 2)
('test', 10, 14, 3)
('sentenc', 15, 23, 4)"
and for our customized latin tokenizer it goes to
"('This', 0, 4, 0)
('is', 5, 7, 1)
('a', 8, 9, 2)
('te', 10, 12, 3)
('est', 13, 16, 4)
('sentence.', 17, 26, 5)"
because of this difference say if we want to search "test sentence" using porter tok, it will return true, for latin tok, it return false.
I have more test cases for this sence in the moretest sheet under this folder. 
