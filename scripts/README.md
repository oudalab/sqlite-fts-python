Dr. Grant and I did some research on this, it looks like the way fts virtual table indexing does not work well with latin tokenizer.
For Phrase searching it is not working well. For Example: 
we have contents: "This is a test sentence"
and when we use the porter tokenizer provided by sqlite, it is being parsed into
```
"('thi', 0, 4, 0)
('is', 5, 7, 1)
('a', 8, 9, 2)
('test', 10, 14, 3)
('sentenc', 15, 23, 4)"
```
and for our customized latin tokenizer it goes to
```
('This', 0, 4, 0)
('is', 5, 7, 1)
('a', 8, 9, 2)
('te', 10, 12, 3)
('est', 13, 16, 4)
('sentence.', 17, 26, 5)
```

because of this difference say if we want to search "test sentence" using porter tok, it will return true, for latin tok, it return false.
I have more test cases for this sence in the moretest sheet under this folder.

I have build the full stack in the DockerCode file, and you can build image with Dockerfile,
using say `docker build -t yan/oulatin .`
then after build the image, you can run the container instance with the command 
"docker run -it yan/oulatin /bin/bash"
And the python oulatin tokenizer test file is in "src" folder inside the container,
so yon can cd to "src" directory and run `python3 pdb__test_oulatin.py` to get into the debug mode of
the ou latin tokenizer.

some of the command you can run at the debug mode can be:
```python
for i in c.execute("select * from sqlite_master").fetchall(): print(i)
for i in c.execute("SELECT * FROM docs WHERE docs MATCH 'test'").fetchall(): print(i)
c.execute("create virtual table tok1 using fts3tokenize('porter')")
for i in c.execute("create virtual table tok1 using fts3tokenize('porter')").fecthall():print(i)
for i in c.execute("select token,start,end,position from tok1 where input='This is a test sentence.'").fetchall():print(i);
```
 
