# week3
## rpm 已編譯好，只要下載安裝到指定位置就可以了
* 在Linux下安裝軟體包，主要有3種辦法
 1. rpm工具（redhat package manager，手動安裝，難點在於包的依賴關係）
rpm包類似於windows下的.exe檔案，安裝路徑和檔名基本都是固定的。
rpm -ivh [rpm完整包名] 
 2. yum工具（python開發出來的工具，操作物件rpm包，能自動解決軟體包的依賴關係，是最常用的方式）
yum install -y 【包名簡稱】
 3. 原始碼包（需要通過編譯器把該原始碼包編譯成可執行的檔案）【安裝難度大】  
./configure---->make---->make install
  * 例如安裝htop 先google 搜尋到檔案後 wget下載下來 然後cd到htop ./configure---->make---->make install 即可安裝完成
## x86 是64位元的作業系統
## 在linux若要下載網路上的檔案，可執行 wget 加上檔案的網址即可立即下載

