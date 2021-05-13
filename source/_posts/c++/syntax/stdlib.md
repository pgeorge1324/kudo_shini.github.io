```c++
// 大小写转换 不是大写原字符返回
int tolower(char c);
int toupper(char c);
// 判断是否为字母
isalnum(char c);
//用位运算的技巧就行了。。。

//大写变小写、小写变大写 : 字符 ^= 32;

//大写变小写、小写变小写 : 字符 |= 32;

//小写变大写、大写变大写 : 字符 &= -33;
```

```c++
// leetcode1108
// 使用replace并修改索引
string defangIPaddr(string address) {
        for (int i = 0; i < address.length(); i ++) 
            if (address[i] == '.') address.replace(i,1,"[.]"), i += 2;
        return address;
        }
```

```c++
// leetcode 1071 gcdString
string gcdOfStrings(string str1, string str2) {
        if (str1 + str2 != str2 + str1) return "";
        return str1.substr(0, __gcd((int)str1.length(), (int)str2.length())); 
        // __gcd() 为c++自带的求最大公约数的函数
    }
```

```c++
// 生成长度为n全是某个字符的字符串
string res = string(n,'a');
// 查询子字符串时判断是否查询到
auto it = words[j].find(words[i]);
if(it!=string::npos){
    vec.push_back(words[i]);
}
```

```c++
// leetcode1455
int isPrefixOfWord(string sentence, string searchWord) {
        istringstream ss(sentence);
        string str;
        for (int i = 1; ss >> str; i ++)
            if (str.find(searchWord) == 0) return i;
        return -1;
    }
int isPrefixOfWord(string sentence, string searchWord) {

        int sizeT = searchWord.size(),sizeS = sentence.size();
        int idx=0,count=0,wordsCount=0;
        while(idx+count<sizeS){
            if(sentence[idx+count]!=' '){
                count++;
                if(sentence[idx+count]==' '||idx+count==sizeS){
                    wordsCount++;
                    if(count>=sizeT){
                        string word = sentence.substr(idx,count);
                        auto id = word.find(searchWord);
                        if(id==0){
                            return wordsCount;
                        }
                    }
                    idx = idx+count;
                    count = 0;
                }
            }else{
                idx++;
            }
        }
        return -1;
    }
```

```c++
// leetcode 1544 字符串当作栈使用
string makeGood(string s) {

        string res;
        for(auto c: s){
            if(!res.empty()&&res.back()!=c&&tolower(res.back())==tolower(c)){
                res.pop_back();
            }else{
                res.push_back(c);
            }
        }
        return res;
    }
```

