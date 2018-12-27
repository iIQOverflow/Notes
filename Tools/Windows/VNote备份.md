# VNote备份

## 相关文件及其路径
* 软件：C:\Program Files\VNote
* 与软件设置有关的文件：%AppData%\vnote\vnote.ini
* 笔记：C:\Users\90388\OneDrive\git_repository\Notes
* 与笔记有关的文件：%AppData%\vnote\session.ini
## 方法
* 如果是重装系统，D 盘一般不动，而 C 盘文件一般会被清除，则仅需要备份 %AppData%\vnote\vnote.ini 和 %AppData%\vnote\session.ini 两个文件，其中 session.ini 文件中 [history] 和 [geometry] 两部分可以删除掉，其中最关键的是 [notebooks] 部分。
* 如果是迁移到其他计算机，则 D:\MyVNote 复制到目标计算机 D:\，%AppData%\vnote\vnote.ini 和 %AppData%\vnote\session.ini 复制到目标计算机 %AppData%\vnote\，如果路径不存在，则自行创建路径。
* 或者，还有一种方法更为实用，将 vnote.ini 和 session.ini 移动到 D:\MyVNote\VNote 路径下（与 VNote.exe 同路径），则 VNote 便不会再创建 %AppData%\vnote 路径并在这里存放配置文件了，但 %LocalAppData%\VNote 路径依然还在（文件夹下有 cache 和 QtWebEngine 两个文件夹），原先在 %AppData%\vnote 路径下的相关文件（snippets（存放片段，可以备份）、styles、templates、themes、session.ini、vnote.ini、vnote.log）直接存放于 D:\MyVNote\VNote 路径下，这样软件和笔记更加便携了。