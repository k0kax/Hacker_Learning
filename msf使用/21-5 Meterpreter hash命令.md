# Meterpreter-HASH与时间戳修改命令

hash命令的相关操作类似于21-2网络命令的操作，基本一致。

## timestomp时间戳

修改时间戳主要用于上传恶意文件以及对修改文件，控制修改或保存时间，降低目标的警惕。

修改某个文件需要timestomp 文件名 此处示例上传一个木马文件。

参考[meterpreter之timestomp命令修改文件MACE时间_redwand的博客-CSDN博客](https://blog.csdn.net/redwand/article/details/108314272)

![](C:\Users\Aurora\OneDrive\桌面\Metasploit\21.5.0.png)

    -a   Set the "last accessed" time of the file//设置最后修改时间
    -b   Set the MACE timestamps so that EnCase shows blanks
    -c   Set the "creation" time of the file//设置创建时间
    -e   Set the "mft entry modified" time of the file
    -f   Set the MACE of attributes equal to the supplied file//将某个文件的时间戳复制到另一个文件上
    -h   Help banner
    -m   Set the "last written" time of the file//设置最后修改时间
    -r   Set the MACE timestamps recursively on a directory
    -v   Display the UTC MACE values of the file//输出详细信息
    -z   Set all four attributes (MACE) of the file
