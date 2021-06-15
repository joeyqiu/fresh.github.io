## recursive-readdir

Recursively list all files in a directory and it's subdirectories. It does not list the directories themselves.

Because it uses fs.readdir, which call readdir under the hood on OSX and Linux, the order of files inside directories is not guaranteed.



递归列出文件夹中所有文件，以及子文件夹。

