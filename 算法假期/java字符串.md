用in.nextLine()读取时要注意，有时第一行是一个数字，这时你希望从第二行开始读取整行。你要写一个in.nextLine()跳过第一行才能正常读取第二行。

java字符串常用方法

1.创建StringBuffer字符串
StringBuffer str = new StringBuffer(str);
public StringBuffer append(String s)：
public delete(int start,int end)：移除此序列中的子字符串的内容,从start到end
public deleteCharAt(int i)：删除指定位置的字符
将StringBuffer转变为String toString()
str.reverse();


2.str.substring(start,end)    从start到end-1
   str.substring(start)          从start到最后

3.str.split("")    神，提取字符串的某些部分很好用，或者分割很好用

4.str1.equals(str2)    str1.compareTo(str2) 

5.字符串转数字Integer.parseInt(str)  Double.parseDouble(str)      数字转字符串String.valueof(int)   Integer.toString(int)

6.String replace(char oldChar, char newChar)：将指定的字符/字符串oldchar全部替换成新的字符/字符串newChar
replace(int start,int end,String str)：用String类型的字符串str替换此字符串的子字符串中的内容
String replaceAll(String regex, String replacement)：使用给定的参数 replacement替换字符串所有匹配给定的正则表达式的子字符串
String replaceFirst(String regex, String replacement)：使用给定replacement 替换此字符串匹配给定的正则表达式的第一个子字符串
regex是正则表达式，替换成功返回替换的字符串，替换失败返回原字符串

7.str1.concat(str2)       str.trim()