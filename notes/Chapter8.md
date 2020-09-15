## 第八章 IO库

**声明:**
本文为《C++ Primer 中文版（第五版）》学习笔记。
原书更为详细，本文仅作学习交流使用。

P278-P290

C++语言不直接处理输入输出，而是通过一组定义在标准库中的类型来处理IO。

* iostream处理控制台IO
* fstream处理命名文件IO
* stringstream完成内存string的IO

ifstream和istringstream继承自istream

ofstream和ostringstream继承自ostream

### 8.1 IO类

（1）IO对象无拷贝或复制。

进行IO操作的函数通常以引用方式传递和返回流。

（2）刷新输出缓冲区

flush刷新缓冲区，但不输出任何额外的字符；

ends向缓冲区插入一个空字符，然后刷新缓冲区。

### 8.2 文件输入输出

| 类       | 作用                   |
| -------- | ---------------------- |
| ifstream | 从一个给定文件读取数据 |
| ofstream | 从一个给定文件写入数据 |
| fstream  | 读写给定文件           |

### 8.3 string流

| 类            | 作用                                   |
| ------------- | -------------------------------------- |
| istringstream | 从string读取数据                       |
| ostringstream | 向string写入数据                       |
| stringstream  | 既可从string读数据也可以向string写数据 |



```cpp
	// will hold a line and word from input, respectively
	string line, word;

	// will hold all the records from the input
	vector<PersonInfo> people;

	// read the input a line at a time until end-of-file (or other error)
	while (getline(is, line)) {       
		PersonInfo info;            // object to hold this record's data
	    istringstream record(line); // bind record to the line we just read
		record >> info.name;        // read the name
	    while (record >> word)      // read the phone numbers 
			info.phones.push_back(word);  // and store them
		people.push_back(info); // append this record to people
	}
```



```cpp
	// for each entry in people
	for (vector<PersonInfo>::const_iterator entry = people.begin();
				entry != people.end(); ++entry) {    
		ostringstream formatted, badNums; // objects created on each loop

		// for each number
		for (vector<string>::const_iterator nums = entry->phones.begin();
				nums != entry->phones.end(); ++nums) {  
			if (!valid(*nums)) {           
				badNums << " " << *nums;  // string in badNums
			} else                        
				// ``writes'' to formatted's string
				formatted << " " << format(*nums); 
		}
		if (badNums.str().empty())      // there were no bad numbers
			os << entry->name << " "    // print the name 
			   << formatted.str() << endl; // and reformatted numbers 
		else                   // otherwise, print the name and bad numbers
			cerr << "input error: " << entry->name 
			     << " invalid number(s) " << badNums.str() << endl;
	}
```