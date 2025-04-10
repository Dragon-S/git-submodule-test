# vlc 二次开发说明

## 一、修改原则：
1.源码尽可能不要修改，如要修改只允许使用打patch的方式

## 二、同步更新官方代码：
1.直接同步官方master代码
2.合并master代码到tvp分支
3.编译测试，官方代码是否可编译通过
4.应用所有patch
5.重新编译

## 三、给vlc打patch方法
1.首次打patch，首先使用git-import-patches，添加reference为refs/patches/upstream-head
在vlc源码目录下执行下面命令
../script/git-import-patches ../patches/vlc 

2.修改vlc源码，然后commit

3.打patch
在vlc源码目录下执行下面命令
../script/git-export-patches -o ../patches/vlc

4.每次同步完成vlc官方代码后，应用patch
在vlc源码目录的上层目录（/Users/user/Desktop/git-test/git-submodule-test）下执行下面命令
python ./script/apply_all_patches.py patches/config.json

5.编辑补丁
（1）对vlc已有补丁的相同文件进行了修改
（2）git add . 添加更改
（3）git commit --fixup “要编辑的补丁的git提交哈希”
（4）git rebase --autosquash -i “要编辑的补丁的git提交哈希”
（5）第四不执行完会进入vim，然后直接wq保存即可
（6）../script/git-export-patches -o ../patches/vlc 即可更新补丁
（7）然后进入git-submodule-test目录，提交修改后的补丁
参考：https://www.electronjs.org/zh/docs/latest/development/patches#编辑现有补丁

## 四、注意：
在重复使用./script/apply_all_patches.py时，如果报错，则可以设置环境变量
export ELECTRON_USE_THREE_WAY_MERGE_FOR_PATCHES=1
